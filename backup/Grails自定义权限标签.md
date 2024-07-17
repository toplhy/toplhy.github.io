grails项目中使用了spring security 插件实现身份认证。

插件提供了一些标签来在页面中进行权限的判断，例如：ifLoggedIn、ifAllGranted、ifAnyGranted、ifNotGranted、sec:access 等，但是基本上都是基于角色来进行验证的。

这次需求是要控制到页面上的每个按钮，而按钮及菜单的权限关联在角色上，角色是用户动态创建的，所以这种基于角色的验证是无法满足需求的。

所以打算这么来做：

### 1. 自定义UserDeatils类
在UserDeatils中添加menus属性

```
import grails.plugin.springsecurity.userdetails.GrailsUser
import org.springframework.security.core.GrantedAuthority

class CustomUser extends GrailsUser{

	private Set<String> menus;

	/**
	* Constructor.
	*
	* @param username the username presented to the
	* <code>DaoAuthenticationProvider</code>
	* @param password the password that should be presented to the
	* <code>DaoAuthenticationProvider</code>
	* @param enabled set to <code>true</code> if the user is enabled
	* @param accountNonExpired set to <code>true</code> if the account has not expired
	* @param credentialsNonExpired set to <code>true</code> if the credentials have not expired
	* @param accountNonLocked set to <code>true</code> if the account is not locked
	* @param authorities the authorities that should be granted to the caller if they
	* presented the correct username and password and the user is enabled. Not null.
	* @param id the id of the domain class instance used to populate this
	*/
	CustomUser(String username, String password, boolean enabled, boolean accountNonExpired, boolean credentialsNonExpired, boolean accountNonLocked, Collection<GrantedAuthority> authorities, Object id, Set<String> menus) {
		super(username, password, enabled, accountNonExpired, credentialsNonExpired, accountNonLocked, authorities, id)
		this.menus = menus;
	}
}
```

### 2. 自定义CustomUserDetailsService
在认证成功后封装menus 

```
import grails.core.GrailsApplication import grails.gorm.transactions.Transactional
import grails.plugin.springsecurity.SpringSecurityUtils
import grails.plugin.springsecurity.userdetails.GrailsUserDetailsService
import grails.plugin.springsecurity.userdetails.NoStackUsernameNotFoundException
import groovy.util.logging.Slf4j
import org.springframework.security.core.GrantedAuthority
import org.springframework.security.core.authority.SimpleGrantedAuthority
import org.springframework.security.core.userdetails.UserDetails
import org.springframework.security.core.userdetails.UsernameNotFoundException

@Slf4j
class CustomUserDetailsService implements GrailsUserDetailsService{

	/**
	 * Some Spring Security classes (e.g. RoleHierarchyVoter) expect at least one role, so
	 * we give a user with no granted roles this one which gets past that restriction but
	 * doesn't grant anything.
	 */
	static final GrantedAuthority NO_ROLE = new SimpleGrantedAuthority(SpringSecurityUtils.NO_ROLE)

	/** Dependency injection for the application. */
	GrailsApplication grailsApplication


	@Transactional(readOnly=true, noRollbackFor=[IllegalArgumentException, UsernameNotFoundException])
	UserDetails loadUserByUsername(String username, boolean loadRoles) throws UsernameNotFoundException {
		def conf = SpringSecurityUtils.securityConfig
		String userClassName = conf.userLookup.userDomainClassName
		def dc = grailsApplication.getDomainClass(userClassName)
		if (!dc) {
			throw new IllegalArgumentException("The specified user domain class '$userClassName' is not a domain class")
		}

		Class<?> User = dc.clazz
		def user = User.createCriteria().get {
			if(conf.userLookup.usernameIgnoreCase) {
				eq((conf.userLookup.usernamePropertyName), username, [ignoreCase: true])
			} else {
				eq((conf.userLookup.usernamePropertyName), username)
			}
		}

		if (!user) {
			log.warn 'User not found: {}', username
			throw new NoStackUsernameNotFoundException()
		}

		Collection<GrantedAuthority> authorities = loadAuthorities(user, username, loadRoles)
		Set<String> menus = loadMenus(user)
		createUserDetails user, authorities, menus
	}

	UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
		loadUserByUsername username, true
	}

	protected Collection<GrantedAuthority> loadAuthorities(user, String username, boolean loadRoles) {
		if (!loadRoles) {
			return []
		}

		def conf = SpringSecurityUtils.securityConfig

		String authoritiesPropertyName = conf.userLookup.authoritiesPropertyName
		String authorityPropertyName = conf.authority.nameField

		boolean useGroups = conf.useRoleGroups
		String authorityGroupPropertyName = conf.authority.groupAuthorityNameField

		Collection<?> userAuthorities = user."$authoritiesPropertyName"
		def authorities

		if (useGroups) {
			if (authorityGroupPropertyName) {
				authorities = userAuthorities.collect {
					it."$authorityGroupPropertyName"
				}.flatten().unique().collect {
					new SimpleGrantedAuthority(it."$authorityPropertyName")
				}
			} else {
				log.warn 'Attempted to use group authorities, but the authority name field for the group class has not been defined.'
			}
		} else {
			authorities = userAuthorities.collect {
				new SimpleGrantedAuthority(it."$authorityPropertyName")
			}
		}
		authorities ?: [NO_ROLE]
	}

	protected Set<String> loadMenus(user) {
		def conf = SpringSecurityUtils.securityConfig
		String authoritiesPropertyName = conf.userLookup.authoritiesPropertyName
		Collection<?> userAuthorities = user."$authoritiesPropertyName"
		def menus = []
		userAuthorities.collect {
			menus += it?.menus?.url
		}
		menus?.removeAll(Collections.singleton(null))
		new HashSet<String>(menus)
	}

	protected UserDetails createUserDetails(user, Collection<GrantedAuthority> authorities, Set<String> menus) {

		def conf = SpringSecurityUtils.securityConfig

		String usernamePropertyName = conf.userLookup.usernamePropertyName
		String passwordPropertyName = conf.userLookup.passwordPropertyName
		String enabledPropertyName = conf.userLookup.enabledPropertyName
		String accountExpiredPropertyName = conf.userLookup.accountExpiredPropertyName
		String accountLockedPropertyName = conf.userLookup.accountLockedPropertyName
		String passwordExpiredPropertyName = conf.userLookup.passwordExpiredPropertyName

		String username = user."$usernamePropertyName"
		String password = user."$passwordPropertyName"
		boolean enabled = enabledPropertyName ? user."$enabledPropertyName" : true
		boolean accountExpired = accountExpiredPropertyName ? user."$accountExpiredPropertyName" : false
		boolean accountLocked = accountLockedPropertyName ? user."$accountLockedPropertyName" : false
		boolean passwordExpired = passwordExpiredPropertyName ? user."$passwordExpiredPropertyName" : false


		new CustomUser(username, password, enabled, !accountExpired, !passwordExpired,!accountLocked, authorities, user.id, menus)
	}

}
```

### 3. 将Service注入到spring
将自定义的CustomUserDetailsService注入到spring中，覆盖掉原有的userDetailsService

```
import com.rici.cas.center.core.BaseUserPasswordEncoderListener
import com.rici.cas.center.security.CustomUserDetailsService

// Place your Spring DSL code here
beans = {
	userDetailsService(CustomUserDetailsService){
		grailsApplication = ref("grailsApplication") 
	}
}
```

### 4. 自定义标签

```
import org.springframework.security.core.Authentication

class PermissionTagLib {

	static defaultEncodeAs = [taglib:'html']

	//static encodeAsForTags = [tagName: [taglib:'html'], otherTagName: [taglib:'none']]

	static namespace = 'per'

	def springSecurityService

	def isAllowed = { attrs, body ->
		// 获取到用户
		Authentication auth = springSecurityService.authentication
		if(auth){
			// 获取到用户能访问的菜单集合
			def menus = auth?.userDetails?.menus
			// 判断此url是否在在集合中
			def url = attrs.url
			if(!url){
				throwTagError("Tag isAllowed requires a url")
			}
			if(menus?.contains(url)){
				out << body()
			}
		}
	}

}
```

### 5. 使用标签进行验证

