## Prefabs

### What are prefabs in Unity?

> **Prefabs** are a special type of component that allows fully configured GameObjects to be saved in the Project for **reuse**.

These assets can then be shared between scenes, or even other projects without having to be configured again. This is quite useful for objects that will be used many times, such as platforms.

**Prefabs is that they are essentially linked copies of the asset** that exist in the Project window. 

For Prefabs, you can
- Instantiate a Prefab using one line of code. Creating equivalent GameObjects from scratch requires many more lines of code.
- Set up, test, and modify the Prefab quickly and easily using the Scene
 view, **Hierarchy** and **Inspector**.


### How to create Prefabs?
Prefabs are created automatically when an object is **dragged from the Hierarchy into the Project window**.
Prefabs look quite similar to other objects that appear in the Project window. However, when selected, their file type will be a **`*.prefab`**. When the Prefab is selected, the Inspector displays all of the components that were configured on the original object.

### How to use Prefabs?
Just **drag Prefabs from project window into Hierarchy or Game Scene** if you want to **reuse**. When prefabs are present in the Hierarchy, they’re represented with **blue text** and a **blue cube**.

### Replacing existing prefabs

You can **replace a Prefab** by **dragging a new GameObject from the Hierarchy window** and **dropping it on top of an existing Prefab asset** in the Project window.

A pop-up show up asking your premission for replacing.

If you are **replacing an existing Prefab**, Unity tries to preserve references to the Prefab itself and the individual parts of the Prefab such as child GameObjects and Components. To do this, it matches the names of GameObjects between the new Prefab and the existing Prefab that you are replacing.

> **Note:** _Because this matching is done by name only, if there are multiple GameObjects with the same name in the Prefab’s hierarchy, it is not possible to predict which will be matched. Therefore if you need to ensure your references are preserved when saving over an existing prefab, you must ensure all GameObjects within the Prefab have unique names.


### Editing a Prefab in Prefab Mode

- In **isolation**: When you edit a Prefab in isolation, Unity **hides the rest of your current working Scene**, and you only see the GameObjects that relate to the Prefab itself.

- In **context**: When you edit a Prefab in context, the **rest of your current working Scene remains visible**, **but locked for editing**.

Editing in **isolation**

You can begin to edit a Prefab in Prefab Mode in several ways. To open a Prefab Asset and edit it in **isolation** you can do it in the following ways:

- Double-click the **Prefab Asset** in the Project window
- Select a **Prefab Asset** in the Project window and click the **Open Prefab button** in the Inspector window

> Modifying prefab asset also link to its prefab instance. 

Alternatively, you can open a Prefab Asset in **Context** via an **instance of that Prefab**. Ways of doing that include:

- Select a **Prefab instance** in the Hierarchy window and click the **Open** button in the Inspector window
- Select a **Prefab instance** in the Hierarchy window and press **P** on the **keyboard**. This is the default keyboard binding
- Use the arrow button next to the Prefab instance in the Hierarchy window


### Unpacking Prefab instances
To **return the contents of a Prefab instance** into a **regular GameObject**, you **unpack the Prefab instance**. This is exactly the **reverse operation of creating (packing) a Prefab**, except that **it doesn’t destroy the Prefab Asset** but **only affects the Prefab instance**.

You can unpack a Prefab instance by **right-clicking on it in the Hierarchy to select Prefab and selecting Unpack Prefab.** The resulting GameObject in the Scene **no longer has any link to its former Prefab Asset**. The **Prefab Asset itself is not affected by this operation** and there **may still be other instances of it in your Project**.

### Instance overrides
Instance overrides allow you to **create variations between Prefab instances**, while still linking those instances to the same Prefab Asset.

When you **modify a Prefab Asset, the changes are reflected in all of its instances**. However, you can also make modifications directly to an individual instance. Doing this **creates an instance override on that particular instance**.

An example would be if you had a Prefab Asset **"Robot"**, which you placed in multiple levels in your game. However, each instance of the **"Robot"** has a **different speed value**, and a **different** **audio clip** assigned.

There are four different types of **instance override**:
- **Overriding the value** of a property
- **Adding a component**
- **Removing a component**
- **Adding a child GameObject**

> **You can create different prefab instances with different type of overrides**

> **Note**: There are **some limitations** with Prefab instances: you **cannot reparent a GameObject that is part of a Prefab**, and you **cannot remove a GameObject that is part of the Prefab**. You can, however, **deactivate a GameObject**, which is a **good substitute for removing a GameObject** (this counts as a property override).

In the Hierarchy window, Prefab instances with overridden or non-default values have an override indicator to show that they have been edited, which Unity displays with a blue line in the left margin with the same appearance as the lines for instance overrides in the Inspector window.

When you edit a Prefab instance in a Scene, Unity displays a indicator next to the parent GameObject in the hierarchy. \
This indicator highlights any Prefab that has non-default override values in any of its child GameObjects. To open the Overrides dropdown directly from the Hierarchy window click on the override indicator. \
The override indicator appears as a **blue line in the left side of the margin** and is identical to the instance override in the Inspector window. For more information, see Instance overrides.

#### Overrides take precedence
An **overridden property value** on a **Prefab instance** **always takes precedence over the value from the Prefab Asset**. This means that if you **change a property on a Prefab Asset**, it **doesn’t have any effect on instances where that property is overridden**.

> If you make a change to a Prefab Asset, and it does not update all instances as expected, you should check whether that property is overridden on the instance. It is best to only use instance overrides when strictly necessary, because if you have a large number of instance overrides throughout your Project, it can be difficult to tell whether your changes to the Prefab Asset will or won’t have an effect on all of the instances.


![](./img/hierarchy-override-indicator.png)

https://docs.unity3d.com/Manual/PrefabInstanceOverrides.html \
https://docs.unity3d.com/Manual/Hierarchy.html#OverrideIndicator


### Editing a Prefab via its instances(Prefab instance)

The Inspector for the root of a Prefab instance has three more controls than a normal GameObject
: **Open**, **Select** and **Overrides**.

The three Prefab controls in the Inspector window for a Prefab instance.

The **Open** button **opens the Prefab Asset that the instance is from in Prefab Mode**, allowing you to **edit the Prefab Asset** and **thereby change all of its instances**. The **Select** button selects the Prefab Asset that this instance is from in the Project window. The **Overrides** button opens the overrides drop-down window.

![](./img/PrefabsInspectorControls1.png)

#### Overrides dropdown

The **Overrides** drop-down window shows all the overrides on the Prefab instance. It also lets you apply overrides from the instance to the Prefab Asset, or revert overrides on the instance back to the values on the Prefab Asset. The **Overrides** drop-down button only appears for the root Prefab instance, not for Prefabs that are inside other Prefabs.

The **Overrides** drop-down window allows you to apply or revert prefab overrides individually, or apply or revert all the prefab overrides in one go.

-   **Applying** an override modifies the Prefab Asset. This puts the override (which is currently only on your Prefab instance) onto the Asset. This means the Prefab Asset now has that modification, and the Prefab instance no longer has that modification as an override.
    
-   **Reverting** an override modifies the Prefab instance. This essentially discards your override and reverts it back to the state of the Prefab Asset.
    

The drop-down window shows a list of changes on the instance in the form of modified, added and removed components, and added GameObjects (including other Prefabs).


![](./img/PrefabsOverridesDropdown1.png)

The Overrides dropdown in the Inspector window when viewing a Prefab instance.

> For the Overrides dropdown, you can see all the override details on this dropdown. If **adding a new component or adding a new child GameObject** on prefab instance, you can see a **"+"** on the icon. If **Removing a component** from prefab instance, you can see a **"-"** on the icon.

![](./img/remove_component.png)

![](./img/removed_component2.png)

#### Revert & Apply overrides
The Overrides drop-down window also has **Revert All** and **Apply All** buttons for reverting or applying all changes at once.

You can also choose to **revert and apply** some of the overrides, not all. You can do this by clicking the override part in the Overrides dropdown, you can see an extension menu next to the Overrides drop-down window. Then you can click **Revert or Apply button** on that override part.

For components with **modified values**, the view **displays a side-by-side comparison of the component’s values** on the Prefab Asset and the modified component on the Prefab instance. This allows you to compare the original Prefab Asset values with the current overrides, so that you can decide whether you would like to **revert or apply those values**.

![](./img/overide_menu1.png)

![](./img/override_menu2.png)

If you select multiple entries at once, the **Revert All** and **Apply All** buttons are replaced by **Revert Selected** and **Apply Selected** buttons. You can use these to revert or apply multiple overrides at once. Similar to the **Apply All** button, the **Apply Selected** button always applies to the outermost Prefab.

![](./img/override_menu3.png)

> After **reverting or applying the override changes**, the override dropdown don't show the changes anymore. If applying changes, the Prefab Asset now has that modification, and the Prefab instance no longer has that modification as an override, **the changes apply on the Prefab Asset and become part of Prefab Asset**, also **all the prefab instances associate from this Prefab asset apply the changes too**. If reverting changes, this essentially **discards your override** and **reverts it back to the state of the Prefab Asset**.


You can also **revert** and **apply** individual overrides using the context menu in the Inspector, instead of using the Overrides drop-down window.

**Overridden properties** are **shown in bold**. They can be reverted or applied via a context menu:

![](./img/PrefabsContextSingleProperty1.png)

Modified components can be reverted or applied via the cog drop-down button or context menu of the component header:

![](./img/PrefabsContextModifiedComponent1.png)


**Added components have a plus badge** that overlays the icon. They can be reverted or applied via the cog drop-down button or context menu of the component header:

![](./img/PrefabsContextAddedComponent1.png)


**Removed components have a minus badge** that overlays the icon. The removal can be reverted or applied via the cog drop-down button or context menu of the component header. Reverting the removal puts the component back, and applying the removal deletes it from the Prefab Asset as well:

![](./img/PrefabsContextRemovedComponent1.png)

**GameObjects (including other Prefabs) that are added as children to a Prefab instance have a plus badge that overlays the icon in the Hierarchy**. They can be reverted or applied via the context menu on the object in the Hierarchy:

![](./img/PrefabsContextAddedGameObject1.png)


### Unused Overrides
Instance override values are stored as data in the scene or prefab in which they are defined. However, an override becomes **"unused"** if its target object is **either invalid or its Property Path is unknown**. In this case, the **data becomes unused**. It is still stored in the scene file, but is **redundant**.

For example, if you override the values of a **`public` field** on a script attached to a prefab, and then delete the script, the data that contains the override values becomes unused because it targets an object that no longer exists.

Override data also becomes unused if you delete the public field definition, or rename it, because the Property Path no longer matches the stored data, although the component object itself still exists.


To check for and remove unused overrides using the Inspector:

-   Select the GameObject(s) that you want to work on.
-   In the inspector, click the **Overrides** drop-down menu. If there are any unused overrides present, the menu will show an **“Unused overrides” option**, otherwise the **“Unused overrides” option** is not displayed.
-   Click **Unused Overrides** to open the unused overrides panel.
-   The unused overrides panel lists the unused overrides, and has a **Remove** button.
-   Click the **Remove** button to remove the unused overrides.


To check for and remove unused instance overrides from the Hierarchy window:

1.  Select the GameObject(s) that you want to work on.
2.  In the Hierarchy window, right-click on one of the selected objects, and select: **Prefab > Remove Unused Overrides**
3.  Unity displays a dialog box showing the number of unused overrides, if any.
4.  Click the **Remove** button to remove the unused overrides.
 
https://docs.unity3d.com/Manual/UnusedOverrides.html


### Prefab Variants
Prefab Variants are useful when you want to have **a set of predefined variations of a Prefab**.

For example, you might want to have several different types of GermSlimeTargets in your game, which are all based on the same basic GermSlimeTarget Prefab. However you may want some GermSlimeTargets to carry items, some to move at different speeds, or some to emit extra sound effects.


A **Prefab Variant** **inherits the properties of another Prefab**, called the **base**. **Overrides made to the Prefab Variant take precedent over the base Prefab’s values**. A Prefab Variant can have **any other Prefab as its base**, including Model Prefabs or other Prefab Variants.

#### Creating a Prefab Variant

There are multiple ways to create a Prefab Variant based on another Prefab.

- You can **right-click** on a **Prefab in the Project view** and select **Create > Prefab Variant**. This creates a **variant of the selected Prefab**, which **initially doesn’t have any overrides**. You can open the **Prefab Variant** in Prefab Mode to begin adding overrides to it.

- You can also **drag a Prefab instance in the Hierarchy into the Project window**. When you do this, a dialog asks if you want to create a new **Original Prefab** or a **Prefab Variant**. If you choose **Prefab Variant** you get a new **Prefab Variant** based on the **Prefab instance** you dragged. Any **overrides you had on that instance are now inside the new Prefab Variant**. You can open it in Prefab Mode to add additional overrides or edit or remove overrides.


#### Editing a Prefab Variant
When a **Prefab Variant** is opened in **Prefab Mode**, the root appears as a **Prefab instance** with the blue Prefab icon. This **Prefab instance** represents the **base Prefab** that the **Prefab Variant** inherits from; it doesn’t represent the **Prefab Variant** itself. Any edits you make to the **Prefab Variant** become **overrides to this base that exists in the Variant**.

As with any **Prefab instance**, you can **use prefab overrides in a Prefab Variant**, such as **modified property values, added components, removed components, and added child GameObjects**. There are also the same limitations: you cannot reparent GameObjects in the Prefab Variant which come from its base Prefab. You also cannot remove a GameObject from a Prefab Variant which exists in its base Prefab. You can, however, deactivate GameObjects (as a property override) to achieve the same effect as removing a GameObject.

When you open the Overrides drop-down window, you can always see in its header which object the overrides are to, and in which context the overrides exist. For a Prefab Variant, the header will say that the overrides are to the base Prefab and exist in the Prefab Variant. To make it extra clear, the **Apply All** button also says **Apply All to Base**.

![](./img/Prefabs_variant.png)

After making modification with **Prefab Variant**,open **Prefab Variant** in Prefab Mode, you can check & apply those overrides to the **base Prefab**.


> For the **Prefab Variant instances**, you can make overrides & and apply those overrides to the **Prefab Variant**.


### Nested Prefabs
You can include Prefab instances inside other Prefabs. This is called nesting Prefabs. Nested Prefabs retain their links to their own Prefab Assets, while also forming part of another Prefab Asset.


https://docs.unity3d.com/Manual/NestedPrefabs.html

### ref
https://docs.unity3d.com/Manual/Prefabs.html \
https://learn.unity.com/tutorial/prefabs-e# \
https://docs.unity3d.com/Manual/InstantiatingPrefabs.html \
https://docs.unity3d.com/Manual/UnpackingPrefabInstances.html

https://docs.unity3d.com/Manual/EditingPrefabViaInstance.html \
https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html

