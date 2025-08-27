# File Header Template Example

Whenever we create a new source file in projects we are getting default generated header which is added to the top of the file. Sometimes we may want to customize those headers and include copyright or license information or maybe remove all the information and keep it minimalistic.
The solution for this is pretty simple and all it takes is creating a `IDETemplateMacros.plist` file.

This project is a guide for setting up a custom file header template in Xcode.

## File Creation
- Open your Xcode project and create a new .plist file using `File > New > File From Template...`(⌘N) and choose `Property List` from the available file templates.
- Name the file `IDETemplateMacros.plist`.
- We now need to move the file to a new location because the location determines the scope of the template. The list of locations is available [here](https://help.apple.com/xcode/mac/9.0/index.html?localePath=en.lproj#/dev91a7a31fc).

For a specific project(per user): `<ProjectName>.xcodeproj/xcuserdata/[username].xcuserdatad/`
For a specific project(shared with team): `<ProjectName>.xcodeproj/xcshareddata/`
For all projects in a specific workspace(per user): `<WorkspaceName>.xcworkspace/xcuserdata/[username].xcuserdatad/`
For all projects in a specific workspace(shared with team): `<WorkspaceName>.xcworkspace/xcshareddata/`
Globally for Xcode: `~/Library/Developer/Xcode/UserData/`

In my case I only needed to use it for this specific project so I've moved it inside `xcuserdata` folder. The easiest way to move this file to that location is by right-clicking on the xcodeproj/xcworkspace file and using `Show Package Contents`.

## File Customization
- First open the IDETemplateMacros.plist file with any text editor e.g. `TextEdit`, `Sublime Text`.
- Add a new entry to the top level dictionary `FILEHEADER` key of type `string`. The `FILEHEADER` key defines the default header for new files created from that template.
```
<dict>
	<key>FILEHEADER</key>
	<string>Our customized template text</string>
</dict>
```
NOTE: We could also do this at the beginning when we create the plist file and add this key directly from Xcode.

- Then set the value of the new entry to the desired text. The string value can include placeholders from the macros available on [this link](https://help.apple.com/xcode/mac/9.0/index.html?localePath=en.lproj#/dev7fe737ce0) wrapped with 3 underscores or use custom ones.
e.g.
```
<dict>
<key>FILEHEADER</key>
<string>
// Project: ___PROJECTNAME___
// Created by: ___FULLUSERNAME___
// ----------------------------------
// ___COPYRIGHT___
</string>
</dict>
```
- To create a custom macro, we just need to add a key inside the dict.
e.g.

```
<dict>
<key>SEPARATOR</key>
<string>// ----------------------------------</string>
<key>FILEHEADER</key>
<string>
___SEPARATOR___
// Project: ___PROJECTNAME___
// Created by: ___FULLUSERNAME___
___SEPARATOR___
// ___COPYRIGHT___
</string>
</dict>
```
You could also use it define a custom author name that should be different from the system's full name
e.g.
```
<key>CUSTOMAUTHOR</key>
<string>Author name</string>
```

- Save the file and restart Xcode. 

## Testing
From now on, every new Swift/Objective-C file you create will include the customized header at the top.
