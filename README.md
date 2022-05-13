# Tri Inspector [![Github license](https://img.shields.io/github/license/codewriter-packages/Tri-Inspector.svg?style=flat-square)](#) [![Unity 2020.3](https://img.shields.io/badge/Unity-2020.3+-2296F3.svg?style=flat-square)](#) ![GitHub package.json version](https://img.shields.io/github/package-json/v/codewriter-packages/Tri-Inspector?style=flat-square)
_Advanced inspector attributes for Unity_

- [Attributes](#Attributes)
  - [Misc](#Misc)
  - [Validation](#Validation)
  - [Styling](#Styling)
  - [Collections](#Collections)
  - [Conditionals](#Conditionals)
  - [Buttons](#Buttons)
  - [Debug](#Debug)
  - [Groups](#Groups)
- [Customization](#Customization)
  - [Custom Drawers](#Custom-Drawers)
  - [Validators](#Validators)
  - [Property Processors](#Property-Processors)
- [How to Install](#How-to-Install)
- [License](#License)

## Attributes

### Misc
#### ShowInInspector

Shows non-serialized property in the inspector.

![ShowInInspector](https://user-images.githubusercontent.com/26966368/168230693-a1a389a6-1a3b-4b94-b4b5-0764e88591f4.png)

```csharp
private float _field;

[ShowInInspector]
public float ReadOnlyProperty => _field;

[ShowInInspector]
public float EditableProperty
{
    get => _field;
    set => _field = value;
}
```

#### PropertyOrder

Changes property order in the inspector.

![PropertyOrder](https://user-images.githubusercontent.com/26966368/168231223-c6628a8d-0d0a-47c1-8850-dc4e789fa14f.png)

```csharp
public float first;

[PropertyOrder(0)]
public float second;
```

#### ReadOnly

Makes property non-editable in the inspector.

![ReadOnly](https://user-images.githubusercontent.com/26966368/168231817-948ef153-eb98-42fb-88ad-3e8d17925b43.png)

```csharp
[ReadOnly]
public Vector3 vec;
```

#### OnValueChanged

Invokes callback on property modification.

```csharp
[OnValueChanged(nameof(OnMaterialChanged))]
public Material mat; 

private void OnMaterialChanged()
{
    Debug.Log("Material changed!");
}
```

### Validation

Tri Inspector has some builtin validators such as `missing reference` and `type mismatch` error. 
Additionally you can mark out your code with validation attributes 
or even write own validators.

![Builtin-Validators](https://user-images.githubusercontent.com/26966368/168232996-04de69a5-91c2-45d8-89b9-627b498db2ce.png)

#### Required

![Required](https://user-images.githubusercontent.com/26966368/168233232-596535b4-bab8-462e-b5d8-7a1c090e5143.png)

```csharp
[Required]
public Material mat;
```

#### ValidateInput

![ValidateInput](https://user-images.githubusercontent.com/26966368/168233592-b4dcd4d4-88ec-4213-a2e5-667719feb0b8.png)

```csharp
[ValidateInput(nameof(ValidateTexture))]
public Texture tex;

private TriValidationResult ValidateTexture()
{
    if (tex == null) return TriValidationResult.Error("Tex is null");
    if (!tex.isReadable) return TriValidationResult.Warning("Tex must be readable");
    return TriValidationResult.Valid;
}

```

### Styling

#### HideLabel
```csharp
[HideLabel]
```

#### LabelText
```csharp
[LabelText("My Label")]
```

#### LabelWidth
```csharp
[LabelWidth(100)]
```

#### GUIColor
```csharp
[GUIColor(0, 1, 0)]
```

#### Space
```csharp
[Space]
```

#### Indent
```csharp
[Indent]
```

#### Title
```csharp
[Title("My Title")]
public int val;
```

#### Header
```csharp
[Header("My Header")]
```

#### PropertySpace
```csharp
[PropertySpace(SpaceBefore = 10, 
               SpaceAfter = 20)]
```

#### PropertyTooltip
```csharp
[PropertyTooltip("My Tooltip")]
```

#### InlineEditor

![InlineEditor](https://user-images.githubusercontent.com/26966368/168234617-86a7f500-e635-46f8-90f2-5696e5ae7e63.png)

```csharp
[InlineEditor]
public Material mat;
```

#### InlineProperty

![InlineProperty](https://user-images.githubusercontent.com/26966368/168234909-1e6bec90-18ed-4d56-91ca-fe09118e1b72.png)

```csharp
public MinMax rangeFoldout;

[InlineProperty(LabelWidth = 40)]
public MinMax rangeInline;

[Serializable]
public class MinMax
{
    public int min;
    public int max;
}
```

### Collections

#### ListDrawerSettings

![ListDrawerSettings](https://user-images.githubusercontent.com/26966368/168235372-1a460037-672c-424f-b2f0-6bf4641c0119.png)

```csharp
[ListDrawerSettings(Draggable = true,
                    HideAddButton = false,
                    HideRemoveButton = false,
                    AlwaysExpanded = false)]
public List<Material> list;
```

### Conditionals

#### ShowIf
```csharp
public bool visible;

[ShowIf(nameof(visible))]
public float val;
```

#### HideIf
```csharp
public bool visible;

[HideIf(nameof(visible))]
public float val;
```

#### EnableIf
```csharp
public bool visible;

[EnableIf(nameof(visible))]
public float val;
```

#### DisableIf
```csharp
public bool visible;

[DisableIf(nameof(visible))]
public float val;
```


#### HideInPlayMode / ShowInPlayMode
```csharp
[HideInPlayMode] [ShowInPlayMode]
```

#### DisableInPlayMode / EnableInPlayMode
```csharp
[DisableInPlayMode] [EnableInPlayMode]
```

#### HideInEditMode / ShowInEditMode
```csharp
[HideInEditMode] [ShowInEditMode]
```

#### DisableInEditMode / EnableInEditMode
```csharp
[DisableInEditMode] [EnableInEditMode]
```

### Buttons

#### Button

![Button](https://user-images.githubusercontent.com/26966368/168235907-2b5ed6d4-d00b-4cd6-999c-432abd0a2230.png)

```csharp
[Button("Click me!")]
private void DoButton()
{
    Debug.Log("Button clicked!");
}
```

### Debug

#### ShowDrawerChain
```csharp
[ShowDrawerChain]
```

### Groups

![Groups](https://user-images.githubusercontent.com/26966368/168236396-b28eba4a-7fe7-4a5c-b185-55fabf1aabf5.png)

```csharp
[DeclareHorizontalGroup("header")]
[DeclareBoxGroup("header/left", Title = "My Left Box")]
[DeclareVerticalGroup("header/right")]
[DeclareBoxGroup("header/right/top", Title = "My Right Box")]
[DeclareTabGroup("header/right/tabs")]
[DeclareBoxGroup("body")]
public class GroupDemo : MonoBehaviour
{
    [Group("header/left")] public bool prop1;
    [Group("header/left")] public int prop2;
    [Group("header/left")] public string prop3;
    [Group("header/left")] public Vector3 prop4;

    [Group("header/right/top")] public string rightProp;

    [Group("body")] public string body1;
    [Group("body")] public string body2;

    [Group("header/right/tabs"), Tab("One")] public float tabOne;
    [Group("header/right/tabs"), Tab("Two")] public float tabTwo;
    [Group("header/right/tabs"), Tab("Three")] public float tabThree;

    [Group("header/right"), Button("Click me!")]
    public void MyButton()
    {
    }
}
```

### Customization

#### Custom Drawers

<details>
  <summary>Custom Value Drawer</summary>

```csharp
using TriInspector;
using UnityEditor;
using UnityEngine;

[assembly: RegisterTriValueDrawer(typeof(BoolDrawer), TriDrawerOrder.Fallback)]

public class BoolDrawer : TriValueDrawer<bool>
{
    public override float GetHeight(float width, TriValue<bool> propertyValue, TriElement next)
    {
        return EditorGUIUtility.singleLineHeight;
    }

    public override void OnGUI(Rect position, TriValue<bool> propertyValue, TriElement next)
    {
        var value = propertyValue.Value;

        EditorGUI.BeginChangeCheck();

        value = EditorGUI.Toggle(position, propertyValue.Property.DisplayNameContent, value);

        if (EditorGUI.EndChangeCheck())
        {
            propertyValue.Value = value;
        }
    }
}
```
</details>

<details>
  <summary>Custom Attribute Drawer</summary>

```csharp
using TriInspector;
using UnityEditor;
using UnityEngine;

[assembly: RegisterTriAttributeDrawer(typeof(LabelWidthDrawer), TriDrawerOrder.Decorator)]

public class LabelWidthDrawer : TriAttributeDrawer<LabelWidthAttribute>
{
    public override void OnGUI(Rect position, TriProperty property, TriElement next)
    {
        var oldLabelWidth = EditorGUIUtility.labelWidth;

        EditorGUIUtility.labelWidth = Attribute.Width;
        next.OnGUI(position);
        EditorGUIUtility.labelWidth = oldLabelWidth;
    }
}
```
</details>

<details>
  <summary>Custom Group Drawer</summary>

```csharp
using TriInspector;
using TriInspector.Elements;

[assembly: RegisterTriGroupDrawer(typeof(TriBoxGroupDrawer))]

public class TriBoxGroupDrawer : TriGroupDrawer<DeclareBoxGroupAttribute>
{
    public override TriPropertyCollectionBaseElement CreateElement(DeclareBoxGroupAttribute attribute)
    {
        // ...
    }
}
```
</details>

#### Validators

<details>
  <summary>Custom Value Validator</summary>

```csharp
using TriInspector;

[assembly: RegisterTriValueValidator(typeof(MissingReferenceValidator<>))]

public class MissingReferenceValidator<T> : TriValueValidator<T>
    where T : UnityEngine.Object
{
    public override TriValidationResult Validate(TriValue<T> propertyValue)
    {
        // ...
    }
}
```
</details>

<details>
  <summary>Custom Attribute Validators</summary>

```csharp
using TriInspector;

[assembly: RegisterTriAttributeValidator(typeof(RequiredValidator), ApplyOnArrayElement = true)]

public class RequiredValidator : TriAttributeValidator<RequiredAttribute>
{
    public override TriValidationResult Validate(TriProperty property)
    {
        // ...
    }
}
```
</details>

#### Property Processors

<details>
  <summary>Custom Property Hide Processor</summary>

```csharp
using TriInspector;
using UnityEngine;

[assembly: RegisterTriPropertyHideProcessor(typeof(HideInPlayModeProcessor))]

public class HideInPlayModeProcessor : TriPropertyHideProcessor<HideInPlayModeAttribute>
{
    public override bool IsHidden(TriProperty property)
    {
        return Application.isPlaying;
    }
}
```
</details>

<details>
  <summary>Custom Property Disable Processor</summary>

```csharp
using TriInspector;
using UnityEngine;

[assembly: RegisterTriPropertyDisableProcessor(typeof(DisableInPlayModeProcessor))]

public class DisableInPlayModeProcessor : TriPropertyDisableProcessor<DisableInPlayModeAttribute>
{
    public override bool IsDisabled(TriProperty property)
    {
        return Application.isPlaying;
    }
}
```
</details>

## How to Install
Minimal Unity Version is 2020.3.

Library distributed as git package ([How to install package from git URL](https://docs.unity3d.com/Manual/upm-ui-giturl.html))
<br>Git URL: `https://github.com/codewriter-packages/Tri-Inspector.git`

After installing the package, you need to unpack the `Installer.unitypackage` that comes with the package. 

Then in `ProjectSettings`/`TriInspector` enable `Full` mode for Tri Inspector.

## License

Tri-Inspector is [MIT licensed](./LICENSE.md).