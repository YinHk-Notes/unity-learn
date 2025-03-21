## Control behavior with events

**UI Toolkit** provides events that communicate **user actions or notifications to visual elements**. 
The **UI Toolkit event system** shares the **same terminology** and event naming as **HTML events**.
 
 
The UI Toolkit **event system listens to events**, coming from the operating system or scripts
, and dispatches these events to visual elements using the **EventDispatcher**. 

The **event dispatcher** determines an **appropriate dispatching strategy** for each event it sends. Once determined, the dispatcher **executes the strategy**.
 

**Visual elements** implement default behaviors for several events. 
This can involve the creation and execution of additional events. 
For example, a **MouseMoveEvent** could generate an additional **MouseEnterEvent** and a **MouseLeaveEvent**. These events enter a queue and process after the current event. 
For example, the **MouseMoveEvent** finishes processing before the **MouseEnterEvent** and **MouseLeaveEvent** events.


### Event System
The **Event System** is a way of **sending events to objects** in the application **based on input**, be it **keyboard**, **mouse**, **touch**, or **custom input**. The **Event System** consists of a few components that work together to **send events**.

When you add an Event System component to a GameObject you will notice that it does not have much functionality exposed, this is because the Event System itself is designed as a manager and facilitator of communication between Event System modules.

The primary roles of the Event System are as follows:

-   Manage which GameObject is considered selected
-   Manage which Input Module is in use
-   Manage Raycasting (if required)
-   Updating all Input Modules as required

#### Input Modules
An Input Module is where the **main logic of how you want the Event System to behave lives**, they are used for:

**Handling** Input Managing event state **Sending events to scene objects**. \
Only one Input Module can be active in the Event System at a time, and they must be components on the same GameObject as the Event System component.

If you want to write a custom Input Module, send events supported by existing UI components in Unity. To extend and write your own events, see the Messaging System documentation.

https://docs.unity3d.com/Packages/com.unity.ugui@2.0/manual/script-StandaloneInputModule.html

#### Input System UI Input Module

The **InputSystemUIInputModule** component acts as a drop-in replacement for the **StandaloneInputModule** component that the Unity UI package. InputSystemUIInputModule provides the same functionality as StandaloneInputModule, but it uses the Input System instead of the legacy Input Manager to drive UI input.

If you have a **StandaloneInputModule** component on a GameObject, and the Input System is installed, Unity shows a button in the Inspector offering to automatically replace it with a **InputSystemUIInputModule** for you. The **InputSystemUIInputModule** is pre-configured to use **default Input Actions** to drive the UI, but you can override that configuration to suit your needs.

https://docs.unity3d.com/Packages/com.unity.inputsystem@1.4/api/UnityEngine.InputSystem.UI.InputSystemUIInputModule.html

#### Using Event System in UItool kit along with Input System package

- Select **GameObject > UI > Event System**. This adds an **Event System GameObject** that includes a **Standalone Input Module** in the Scene. The module shows an **error message** along with a button in the Inspector window.
- Select **Replace with InputSystemUIInputModule** to **replace the Standalone Input Module** with the **Input System UI Input Module**. The Input System UI Input Module and its accompanying Event System allows the events of both UI Toolkit elements to be dispatched correctly.



### Event propagation
After the event dispatcher selects the event target, it computes the propagation path of the event. The propagation path is an ordered list of visual elements that receive the event. The propagation path occurs in the following order:

1.  The path starts at the root of the visual element tree and descends towards the target. This is the **trickle-down phase**.
2.  The event target receives the event.
3.  The event then ascends the tree towards the root. This is the **bubble-up phase**.

![](./img/UIElementsEvents.png)

Propagation path

Most event types are sent to all elements along the propagation path. Some event types skip the bubble-up phase, and some event types are sent to the event target only.

If you hide or disable an element, it won’t receive events. Events still propagate to the ancestors and descendants of a hidden or disabled element.

As an event travels along the propagation path, `Event.currentTarget` updates to the element handling the event. **Within an event callback function**, there are **two properties** that log the dispatch behavior:

-   `Event.currentTarget` is the visual element on which the callback was registered.
-   `Event.target` is the element




### Dispatch behavior of event types

Each event type has its own dispatch behavior. The behavior of each event type breaks down into three stages:

-   **Trickles down**: Events sent to elements during the trickle down phase.
-   **Bubbles up**: Events sent to elements during the bubble-up phase.
-   **Cancellable**: Events that can have their default action execution cancelled, stopped, or prevented.


### Handle events
Events in **UI Toolkit** are similar to **HTML events**. When an event occurs, it’s sent to the target visual element
and to all elements within the propagation path in the **visual element tree**.
 
The event handling sequence is as follows:

1.  Execute event callbacks on elements from the root element down to the parent of the event target. This is the **trickle-down phase** of the dispatch process.
2.  Execute event callbacks on the event target. This is the **target phase** of the dispatch process.
3.  Call `ExecuteDefaultActionAtTarget()` on the event target.
4.  Execute event callbacks on elements from the event target parent up to the root. This is the **bubble-up phase** of the dispatch process.
5.  Call `ExecuteDefaultAction()` on the event target.

As an event moves along the propagation path, the `Event.currentTarget` property updates to the element currently handling the event. Within an event callback function:

-   `Event.currentTarget` is the visual element that the callback registers on.
-   `Event.target` is the visual element where the original event occurs.



### Register an event callback

You can register an event callback to customize the behavior of an individual instance of an existing class, such as reacting to a mouse click on a text label.

Each element along the propagation path (except the target) can receive an event twice:

-   Once during the trickle-down phase.
-   Once during the bubble-up phase.

By default, a registered callback executes during the target phase and the bubble-up phase. This default behavior ensures that a parent element reacts after its child element.

> On the other hand, if you want a parent element to react before its child, register your callback with the **`TrickleDown.TrickleDown`** option:

```csharp
using UnityEngine;
using UnityEngine.UIElements;

...
VisualElement myElement = new VisualElement();

// Register a callback for the trickle-down phase.
myElement.RegisterCallback<MouseDownEvent>(MyCallback, TrickleDown.TrickleDown);
...
```

This informs the dispatcher to execute the callback at the target phase and the trickle-down phase.

To add a custom behavior to a specific visual element, register an event callback on that element.


The following example registers a callback for the `MouseDownEvent`:

```csharp
// Register a callback on a mouse down event
myElement.RegisterCallback<MouseDownEvent>(MyCallback);
```

The signature for the callback function looks like this:

```csharp
void MyCallback(MouseDownEvent evt) { /* ... */ }
```

You can register **multiple callbacks** for an event. However, you can **only register the same callback function** on the **same event** and **propagation phase once**.

> To remove a callback from a `VisualElement`, call the `myElement.UnregisterCallback()` method.


> **Note**: registering a callback should be before **`Add()`**, otherwise noting happen with the callback

### Listen to value changes
UI controls can use the `value` property to **hold data for their internal state**. For example:

-   A `Toggle` holds a Boolean value that changes when the `Toggle` is turned on or off.
-   An `IntegerField` holds an integer that holds the field’s value.

You can get the value of the control by the following:

-   Get the value from the control directly: `int val = myIntegerField.value;`.
    
-   Listen to a `ChangeEvent` sent by the control and process the change when it happens. You must register your callback to the event like this:
    
    ```csharp
    //RegisterValueChangedCallback is a shortcut for RegisterCallback<ChangeEvent>. 
    //It constrains the right type of T for any VisualElement that implements an 
    //INotifyValueChange interface.
    myIntegerField.RegisterValueChangedCallback(OnIntegerFieldChange); 
    ```
    
    The signature for the callback function looks like this:
    
    ```csharp
    void OnIntegerFieldChange(ChangeEvent<int> evt) { /* ... */ }
    ```
    

You can change the value of a control by the following:

-   Directly change the `value` variable: `myControl.value = myNewValue;`. This will trigger a new `ChangeEvent`.
-   Use `myControl.SetValueWithoutNotify(myNewValue);`. This won’t trigger a new `ChangeEvent`.


### Handle input events for a control
> You can use an **event handler** or use a **manipulator** to handle input events.

#### Use a manipulator to handle events
If you want to **separate your event logic from your UI code**, use a **manipulator** to handle events. A **manipulator** is a dedicated class that **stores, registers, and unregisters event callbacks**. You can use or inherit from one of the manipulators that UI Toolkit supports to handle events.

| Manipulator | Inherits from | Description |
| --- | --- | --- |
| Manipulator |  | Base class for all provided manipulators. |
| KeyboardNavigationManipulator | Manipulator | Handles translation of device-specific input events to higher-level navigation operations with a keyboard. |
| MouseManipulator | Manipulator | Handles mouse input. Has a list of activation filters. |
| ContextualMenuManipulator | MouseManipulator | Displays a contextual menu when the user clicks the right mouse button or presses the menu key on the keyboard. |
| PointerManipulator | MouseManipulator | Handles pointer input. Has a list of activation filters. |
| Clickable | PointerManipulator | Tracks mouse events on an element and callbacks when a user clicks a mouse button while the pointer hovers over an element. |

### Synthesize and send events

Use **`UnityEngine.Event`** to create customed Event. A UnityGUI event. \
Events correspond to user input (key presses, mouse actions), or are UnityGUI layout or rendering events. \
The event system uses a pool of events to avoid allocating event objects repeatedly. To synthesize and send your **own events**:

1.  Get an `Event` object from the pool of events.
2.  Fill in the `Event` properties.
3.  Enclose the `Event` in a `using` block to ensure it’s returned to the `Event` pool.
4.  Pass the `Event` to `panel.visualTree.SendEvent()`.

You can send operating system events, such as keyboard and mouse events. To do so, use a `UnityEngine.Event` to initialize the **UI** Toolkit event.

The following example demonstrates how to synthesize and send events:

```csharp
void SynthesizeAndSendKeyDownEvent(IPanel panel, KeyCode code,
     char character = '\0', EventModifiers modifiers = EventModifiers.None)
{
    // Create a UnityEngine.Event to hold initialization data.
    var evt = new Event() {
        type = EventType.KeyDownEvent,
        keyCode = code,
        character = character,
        modifiers = modifiers
    };

    using (KeyDownEvent keyDownEvent = KeyDownEvent.GetPooled(evt))
    {
        panel.visualTree.SendEvent(keyDownEvent);
    }
}
```

https://docs.unity3d.com/ScriptReference/Event.html \

**`Interface IEventHandler`** \
https://docs.unity3d.com/Packages/com.unity.ui@1.0/api/UnityEngine.UIElements.IEventHandler.html?q=SendEvent

> **Note**: Don’t send events that don’t come from the operating system and can’t be found in the UnityEngine.Event types. Some events are sent by UI Toolkit as a reaction to internal state changes and must not be sent by external processes. For example, if you send PointerCaptureEvent, visual elements assume that the underlying conditions for that event are met and won’t set pointer capture for them. This might break the internal configurations of the visual element and cause undefined behaviors.



UI Builder is not meant for event management, nor is the UXML document. To handle a button-click or any other event, create a C# Script and link it to the UXML.

eg:
```cs
public class UIEventHandler : MonoBehaviour
{
   [SerializeField]
   private UIDocument m_UIDocument;
 
   private Label m_Label;
   private int m_ButtonClickCount = 0;
   private Toggle m_Toggle;
   private Button m_Button;
 
   public void Start()
   {
       var rootElement = m_UIDocument.rootVisualElement;
 
       // This searches for the VisualElement Button named “EventButton”
       // rootElement.Query<> and rootElement.Q<> can both be used
       m_Button = rootElement.Q<Button>("EventButton");
 
       // Elements with no values like Buttons can register callBacks
       // with clickable.clicked
       m_Button.clickable.clicked += OnButtonClicked;
 
       // This searches for the VisualElement Toggle named “ColorToggle”
       m_Toggle = rootElement.Query<Toggle>("ColorToggle");
 
       // Elements with changing values: toggle, TextField, etc... 
       // implement the INotifyValueChanged interface,
       // meaning they use RegisterValueChangedCallback and 
       // UnRegisterValueChangedCallback
       m_Toggle.RegisterValueChangedCallback(OnToggleValueChanged);
 
       // Cache the reference to the Label since we will access it repeatedly.
       // This avoids the relatively costly VisualElement search each time we update
       // the label.
       m_Label = rootElement.Q<Label>("IncrementLabel");
       m_Label.text = m_ButtonClickCount.ToString();
   }
 
   private void OnDestroy()
   {
       m_Button.clickable.clicked -= OnClicked;
       m_Toggle.UnregisterValueChangedCallback(OnToggleValueChanged);
   }
 
   private void OnButtonClicked()
   {
       m_ButtonClickCount++;
       m_Label.text = m_ButtonClickCount.ToString();
   }
 
   private void OnToggleValueChanged(ChangeEvent<bool> evt)
   {
       Debug.Log("New toggle value is: " + evt.newValue);
   }
}

```



### ref
https://docs.unity3d.com/Manual/UIE-Events.html

https://docs.unity3d.com/Manual/UIE-Events-Handling.html



