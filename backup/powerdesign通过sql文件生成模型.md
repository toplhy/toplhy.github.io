记录使用powerdesign通过sql文件生成模型

### 选择Reverse Engineer
File > Reverse Engineer > Database

### 选择sql文件
![选择sql文件](https://raw.githubusercontent.com/toplhy/toplhy.github.io/main/images/database/2020-05-11-1.png)

### 执行脚本
+ 将name改为sql的注释, Tools > Execute Commands > Edit/Run Sricpt 执行以下脚本

```
Option Explicit
ValidationMode = True
InteractiveMode = im_Batch
Dim mdl ' the current model
' get the current active model
Set mdl = ActiveModel
If (mdl Is Nothing) Then
MsgBox "There is no current Model "
ElseIf Not mdl.IsKindOf(PdPDM.cls_Model) Then
MsgBox "The current model is not an Physical Data model. "
Else
ProcessFolder mdl
End If
Private sub ProcessFolder(folder)
On Error Resume Next
Dim Tab 'running table
for each Tab in folder.tables
if not tab.isShortcut then
tab.name = tab.comment
Dim col ' running column
for each col in tab.columns
if col.comment="" then
else
col.name= col.comment
end if
next
end if
next
 Dim view 'running view
 for each view in folder.Views
 if not view.isShortcut then
 view.name = view.comment
 end if
 next
 ' go into the sub-packages 
 Dim f ' running folder 
 For Each f In folder.Packages
 if not f.IsShortcut then
 ProcessFolder f
 end if
 Next
end sub
```
