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
<img width="314" height="248" alt="Blank2D - TestInspector cs_ - Microsoft Visual Studio 8_8_2025 8_14_38 PM" src="https://github.com/user-attachments/assets/1f65e24b-866b-4576-818b-3557a0a21efc" />\
Anything inside the if statement will get executed once the button has been clicked. This can include console prints, functions from the targeted[^2] class, and changing values in the inspector.
# Displaying Variables
`EditorGUILayout.PropertyField(SerializedProperty property)`
Displays the property in the inspector using the name as defined by the variable in the targeted[^2] class.
# Tooltips
`EditorGUILayout.PropertyField(SerializedProperty property, GUIContent label)`\
Include the label parameter by declaring a `new GUIContent(String text)` when displaying the variable to include a tooltip. The label will become the new displayed name instead of the variable name.


# Dropdowns

# Headers
<img width="358" height="286" alt="Messy-Cat-Game - DogContextCustomInspector cs_ - Microsoft Visual Studio 8_8_2025 8_21_04 PM" src="https://github.com/user-attachments/assets/a5b2c86e-f936-439d-a181-b8f682e3e79d" />\

# Custom Spacing
### Before
<img width="286" height="229" alt="Before Spacing" src="https://github.com/user-attachments/assets/ac19703e-1d58-437f-8d85-c0630e2969c6" />

### After
<img width="296" height="236" alt="After Spacing" src="https://github.com/user-attachments/assets/559a0627-a439-407d-93a9-aad7bc69b827" />\
By default `EditorGUILayout.Space()` creates a horizontal gap of of 6[^1]. This can be overridden by using the `EditorGUILayout.Space(float width)` overload.
[^1]: There isn't a measure of unit for the input float.
[^2]: When I refer to the 'Targeted class' I am referring to the class that this script is acting as the custom inspector for.
