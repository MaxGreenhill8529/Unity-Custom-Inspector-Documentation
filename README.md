# Unity-Custom-Inspector-Documentation
This documentation is unofficial and was made for my own personal reference and needs. With that said I tried my best to be accurate and extensive.\
This is for the original and thus older version of Unity's custom inspector solution ['IMGUI'](https://docs.unity3d.com/6000.1/Documentation/Manual/GUIScriptingGuide.html) and while it is still supported and a way to implement custom inspectors, the company is leaning towards using their newer UI Toolkit. 
> Unity intends for UI Toolkit to become the recommended UI system for new UI development projects, but it still lacks some features available in uGUI and IMGUI.

>[!Note]
> You will notice that I'm using `serializedObject` instead of `target` for variable grabbing. By using `serializedObject` you allow compatability with Unity's undo and multi-object editing features.

For design principals and best practices refer to the following [Editor Foundations Design System](https://www.foundations.unity.com/getting-started)
# Script Set Up
```
using UnityEngine;
using UnityEditor;
[CustomEditor(typeOf(Monobehavior))]
public class CustomEditor : Editor
{
  public override void OnInspectorGUI()
  {
    

    // Apply changes to serialized properties
    serializedObject.ApplyModifiedProperties();
  }
}
```
Change 'Monobehavior' to the script that you are making the custom inspector for. And make sure to inherit from Editor.\
For added customization, you can change the CanEditMultipleObjects attribute which tells Unity that you can select multiple objects with this editor and change them all at the same time. The default value is `true` `[CustomEditor(typeof(LookAtPoint),bool CanEditMultipleObjects)]`
`serializedObject.ApplyModifiedProperties();` Make sure to include this at the end of the OnInspectorGUI() call. This applies the changes made in the inspector to the variables themselves.
> [!TIP]
> Place the script inside a folder labeled 'Editor' to signal to Unity to handle it as an editor script which excludes it from builds and gives it, it's own assembly behind the scenes. The script will still work outside of an editor folder, but this is best practice.




# Buttons
```
if(GUILayout.Button("Button"))
{
  //Do stuff...
}
```
Anything inside the if statement will get executed once the button has been clicked. This can be any function call that is not reliant on the game running. For example, [Destroy()](https://docs.unity3d.com/6000.2/Documentation/ScriptReference/Object.Destroy.html) wont execute because it is dependent on the Update Loop. The solution would be to use DestroyImmediate().
>[DestroyImmediate](https://docs.unity3d.com/6000.2/Documentation/ScriptReference/Object.DestroyImmediate.html) is intended for use in scripts that run in Edit mode, not at runtime. In Edit mode, the usual delayed destruction performed by Destroy does not occur, so immediate destruction is necessary.[^3]
[^3]: [Source for DestroyImmediate Quote](https://docs.unity3d.com/6000.2/Documentation/ScriptReference/Object.DestroyImmediate.html)
# Displaying Variables
```
SerializedProperty property = serializedObject.FindProperty(string nameOfProperty);
EditorGUILayout.PropertyField(SerializedProperty property);
```
Displays the field found by the FindProperty function. Note that the string is case sensitive!
> [!WARNING]
> If the variable you are attempting to access is marked as `private`, include the `[SerializeField]` to allow the editor script to serialize it.

> [!TIP]
> If you find strings annoying you can use [nameOf()](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/operators/nameof) instead, however this requires the editor script to have 'get' access to the variable which can become a hassle.

# Tooltips And Custom Variable Names
`EditorGUILayout.PropertyField(SerializedProperty property, GUIContent label)`\
Including the `GUIContent label` parameter allows you to override the default variable name and include a tooltip if you'd like. Create a GUIContent using the following constructor [`public GUIContent(string text, string tooltip);`](https://docs.unity3d.com/ScriptReference/GUIContent-ctor.html)


# Dropdowns

```
boolean foldoutVisible = EditorGUILayout.BeginFoldoutHeaderGroup(foldoutVisible, string header);
if(foldoutVisible)
{
  //GUI to display when the foldout is active
}
EditorGUILayout.EndFoldoutHeaderGroup();
```
>[!Tip]
>The header string can be dynamic :D Heres an example from the unity documentation of this happening...
>```
>    bool showPosition = true;
>    string status = "Select a GameObject";
>
>    [MenuItem("Examples/Foldout Header Usage")]
>    static void CreateWindow()
>    {
>        GetWindow<FoldoutHeaderUsage>();
>    }
>
>    public void OnGUI()
>    {
>        showPosition = EditorGUILayout.BeginFoldoutHeaderGroup(showPosition, status);
>
>        if (showPosition)
>            if (Selection.activeTransform)
>            {
>                Selection.activeTransform.position =
>                    EditorGUILayout.Vector3Field("Position", Selection.activeTransform.position);
>                status = Selection.activeTransform.name;
>            }
>
>        if (!Selection.activeTransform)
>        {
>            status = "Select a GameObject";
>            showPosition = false;
>        }
>        // End the Foldout Header that we began above.
>        EditorGUILayout.EndFoldoutHeaderGroup();
>    }
>```

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

# Help Boxes
`HelpBox(string message, MessageType type)`
`enum MessageType {None, Info, Warning, Error}`
> [!Tip]
> * Error: Indicates a function will not complete an action
> * Warning: Indicates a function that will complete an action but may return results that are not what the user intended
> * Info: Used for communicating non-critical information

# Making read only fields
```
`EditorGUI.BeginDisabledGroup(true);`
//Anything between these two lines will not be editable from within the inspector.
`EditorGUI.EndDisabledGroup();
```
