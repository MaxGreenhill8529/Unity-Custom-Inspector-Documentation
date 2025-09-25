# Unity-Custom-Inspector-Documentation
This documentation is unofficial and was made for my own personal reference and needs. With that said I tried my best to be accurate and extensive.\
This is for the original and thus older version of Unity's custom inspector solution ['IMGUI'](https://docs.unity3d.com/6000.1/Documentation/Manual/GUIScriptingGuide.html) and while it is still supported and a way to implement custom inspectors, the company is leaning towards using their newer UI Toolkit. 
> Unity intends for UI Toolkit to become the recommended UI system for new UI development projects, but it still lacks some features available in uGUI and IMGUI.

# Script Set Up
<img width="370" height="296" alt="Picture of Visual Studio code block" src="https://github.com/user-attachments/assets/6e4c7a7a-2a23-4f4e-8918-856840d1e969" />\
Change 'Monobehavior' to the script that you are making the custom inspector for. And make sure to inherit from Editor.\
For added customization, you can change the CanEditMultipleObjects attribute which tells Unity that you can select multiple objects with this editor and change them all at the same time. The default value is `true` `[CustomEditor(typeof(LookAtPoint),bool CanEditMultipleObjects)]`
> [!TIP]
> Place the script inside a folder labeled 'Editor' to signal to Unity to handle it as an editor script which excludes it from builds and gives it, it's own assembly behind the scenes. The script will still work outside of an editor folder, but this is best practice.




# Button
```
if(GUILayout.Button("Button"))
{
  //Do stuff...
}
```
Anything inside the if statement will get executed once the button has been clicked. This can be any function call that is not reliant on the game running. For example, [Destroy()](https://docs.unity3d.com/6000.2/Documentation/ScriptReference/Object.Destroy.html) wont execute because it is dependent on the Update Loop. The solution would be to use DestroyImmediate().
>[DestroyImmediate](https://docs.unity3d.com/6000.2/Documentation/ScriptReference/Object.DestroyImmediate.html) is intended for use in scripts that run in Edit mode, not at runtime. In Edit mode, the usual delayed destruction performed by Destroy does not occur, so immediate destruction is necessary.[^3]

# Displaying Variables
`SerializedProperty property = serializedObject.FindProperty(string nameOfProperty);`\
`EditorGUILayout.PropertyField(SerializedProperty property);`
Displays the property in the inspector using the name as defined by the variable in the targeted[^2] class.
Everything is displayed in the order it is read from top to bottom in the editor script.

# Tooltips And Custom Variable Names
`EditorGUILayout.PropertyField(SerializedProperty property, GUIContent label)`\
Including the `GUIContent label` parameter allows you to override the default variable name and include a tooltip if you'd like.
[`public GUIContent(string text, string tooltip);`](https://docs.unity3d.com/ScriptReference/GUIContent-ctor.html)

# Dropdowns
`boolean foldoutVisible = EditorGUILayout.BeginFoldoutHeaderGroup(foldoutVisible, string header);`\

```
if(foldoutVisible)
{

}
```
# Headers
`EditorGUILayout.LabelField(string text, EditorStyles style)`
['EditorStyle Options'](https://docs.unity3d.com/6000.2/Documentation/ScriptReference/EditorStyles.html)
# Custom Spacing
### Before
<img width="286" height="229" alt="Before Spacing" src="https://github.com/user-attachments/assets/ac19703e-1d58-437f-8d85-c0630e2969c6" />

### After
<img width="296" height="236" alt="After Spacing" src="https://github.com/user-attachments/assets/559a0627-a439-407d-93a9-aad7bc69b827" />\
By default `EditorGUILayout.Space()` creates a horizontal gap of 6[^1]. This can be overridden by using the `EditorGUILayout.Space(float width)` overload.
[^1]: There isn't a measure of unit for the input float.
[^2]: When I refer to the 'Targeted class' I am referring to the class that this script is acting as the custom inspector for.
[^3]: [Source for DestroyImmediate Quote](https://docs.unity3d.com/6000.2/Documentation/ScriptReference/Object.DestroyImmediate.html)
# Help Boxes
`HelpBox(string message, MessageType type)`
`enum MessageType {None, Info, Warning, Error}`

# Making read only fields
`EditorGUI.BeginDisabledGroup(true);`
anything between these two lines will be faded out and read only.
`EditorGUI.EndDisabledGroup();`
