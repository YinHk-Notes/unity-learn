## Unity Scripting

Unity allows you to create your own Components using scripts. These allow you to trigger game events, modify Component properties over time and respond to user input in any way you like.

You can attach a script by dragging the script asset to a GameObject in the hierarchy panel or to the inspector of the GameObject that is currently selected. 

Unity provides **`UnityEngine`** & **`UnityEditor`** scripting API. 

**UnityEngine** is for built in Unity components such as the actual GameObjects and Transform that are used within the engine.

**UnityEditor** is the **GUI of the actual program**. You can also **make custom editors** of yours.

https://www.youtube.com/watch?v=491TSNwXTIg


### Declare variable/array
When declare variables or array, it must specify its data type:   **value type** (eg: string/float…) or **reference type** ( class eg: `Transform/Gameobject`).

**public variable/array**: \
Inside script set variable/array 做`public`, 在unity inspector 會顯示該variable/array, 並可於inspector 修改其value, 作更改. `private` variable can’t.  Value of variable/array in inspector can override that in script.


### Inheritance

**`object`(C#) -> `Object`(Unity) -> `Component` -> `Behaviour` -> `MonoBehaviour` -> ....**
                      
**`object`(C#) -> `Object`(Unity) -> `GameObject`**

### Common classes used frequently for Unity Scripting

- **`MonoBehaviour`** class
- **`GameObject`** class
- **`Transform`** class
- **`Vector3`** class
- **`Physics`** class
- **`Rigidbody`** class
- **`Collider`** & **`Collision`** class

### `Debug.Log()`
```cs
// Use this for initialization
void Start () 
{
    Debug.Log("Hello world!");
}
```


> **`Debug.Log`** is a simple command that just prints a message to Unity’s console output. If you press Play now, you should see the message at the bottom of the main Unity editor window and in the Console window (menu: Window > General > Console
).

- **`Debug.LogError`**
- **`Debug.LogWarning`**
- **`Debug.LogException`**
- **`Debug.DrawRay`**
- **`Debug.DrawLine`**

https://docs.unity3d.com/ScriptReference/Debug.html

### `print()`
**`MonoBehaviour.print`** method identical to `Debug.Log()`
```cs
// Use this for initialization
void Start () 
{
  print("Hello world!");
}
```

https://learn.unity.com/tutorial/introduction-to-the-console-window#5f68b4eeedbc2a002022b83c

### Debugging C# code in Unity using Visual Studio
https://docs.unity3d.com/Manual/ManagedCodeDebugging.html#AttachToEditor

https://unity.com/how-to/debugging-with-microsoft-visual-studio-2022


### How to access another script / get access variable/method from other script?
從script B access variables/method from script A, the variable/method to be accessed must be public,  declare class variable in script B,
```cs
private  scriptName/className  variableName;
```
- If both scripts are on the same GameObject, you can use this:
  ```cs
   gameObject.GetComponent<Script name>();
  ```
- If the scripts are on different GameObjects you can do this:
  ```cs
   GameObject.Find("name Of Object").GetComponent<Script name>();
  ```
  or create  **`public GameObject variable's name`**  then drag the game object into inspecter.
  ```cs
   variable’s name.GetComponent<Script name>();
  ```

- Another method is to use **UnityEvents** or **C# event**. This is much better.

#### Using namespace
For **static** method:

```cs
namespace ImportantStuff
{
    public class Foo
    {
        public static void SomeHelperMethod()
        {
            // [ ... ]
        }
    }
}

```
```cs
using ImportantStuff;

public class someOtherClass : MonoBehaviour
{
    void Start()
    {
        Foo.SomeHelperMethod();
    }
}

```
For non **static** method:

```cs
namespace ImportantStuff
{
    public class Foo
    {
        public void Somenethod()
        {
            // [ ... ]
        }
    }
}
```
```cs
using ImportantStuff;

public class someOtherClass : MonoBehaviour
{
    void Start()
    {
        Foo fool = new Fool();
        foo.SomeMethod();
    }
}

```

https://www.youtube.com/watch?v=Y7pp2gzCzUI

https://www.youtube.com/watch?v=2pCkInvkwZ0

https://discussions.unity.com/t/how-to-reference-a-class-in-another-script-properly/185295/5

### Life cycle
![](./img/monobehaviour_flowchart.svg)

https://docs.unity3d.com/Manual/ExecutionOrder.html

### Scripting backends
Unity uses the open-source `.NET` platform to ensure that applications you make with Unity can run on a wide variety of different hardware configurations.

Unity has two scripting backends; **Mono**, and **IL2CPP** (Intermediate Language To C++), each of which uses a different compilation technique:

- Mono uses **just-in-time (JIT) compilation** and compiles code on demand at runtime.
- IL2CPP uses **ahead-of-time (AOT) compilation** and compiles your entire application before it runs.

The benefit of using a JIT-based scripting backend is that the compilation time is typically much faster than AOT.

By default, Unity uses the Mono backend on platforms that support Mono. When you build a player for your application, you can choose which scripting backend to use. To do this through the Editor, go to **`Edit`** > **`Project Settings`** > **`Player`**, open the Other Settings panel, then **click on the Scripting Backend dropdown** and select which backend you want.

### Managed code stripping
When you build an application, Unity compiles and then searches the assemblies (.DLLs) in your project to detect and remove unused code. This process of stripping code reduces the final binary size of your build, but increases build time. Code stripping is disabled by default when you use Mono but code stripping can’t be disabled for IL2CPP. 

go to `Edit` > `Project Settings` > `Player`, open the Other Settings panel, then click on the Managed Stripping Level dropdown and select a stripping level.

The Unity build process uses a tool called the Unity linker to strip managed code. The Unity linker is a version of the IL Linker customized to work with Unity.

https://docs.unity3d.com/Manual/ManagedCodeStripping.html


### Structure
Eg:
```cs

public class ExampleClass : MonoBehaviour
{
   // field, to decalre variable here

   // aware method
   void Aware()
   {
        //...
   }

   // start method
   void Start()
   {
        //...
   }

   // update method
   void Update()
   {
        //...
   }

   // fixedupdate method
   void FixedUpdate()
   {
        //...
   }

   // Other methods defined by your own

   // Corountine methods
   IEnumerator ExampleMthod()
   {
       //,,,
   }
}
```


### Configure entering play mode
- **Domain Reloading**: Resets the scripting state, by reloading the scripting domain. \
  https://docs.unity3d.com/Manual/DomainReloading.html
- **Scene Reloading**: Resets the Scene state, by reloading the Scene. \
  https://docs.unity3d.com/Manual/SceneReloading.html
  

### ref
https://docs.unity3d.com/ScriptReference/index.html

https://blog.devgenius.io/unity-3d-c-scripting-cheatsheet-for-beginners-be6030b5a9ed

https://gamedev.stackexchange.com/questions

https://learn.unity.com/project/intermediate-gameplay-scripting

