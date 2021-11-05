* [UIKit](#uikit)
  * [Live Rendering in Storyboards](#how-could-you-setup-live-rendering)
  * [Ways of specifing the layout of elements](#ways-of-specifing-the-layout-of-elements)
  * [Autolayout formula](#formula-of-autolayout)
  * [Size Classes](#size-classes)
  * [Intrinsic Content Size](#intrinsic-content-size)
  * [Frame and bounds](#whats-the-difference-between-the-frame-and-the-bounds)
  * [When bounds origin will be different from 0,0?](#when-bounds-origin-will-be-different-from-00)
  * [What do layer objects represent?](#what-are-layer-objects-and-what-do-they-represent)
  * [File owner meaning](#file-owner)
  * [Life Cycle of App](#app-life-cycle)

# UIKit

## How could you setup Live Rendering?
With `@IBInspectable` and `@IBDesignable`, it’s possible to build a custom interface for configuring your custom controls and have them rendered in real-time while designing your project.

### @IBInspectable -> 修飾屬性

IBInspectable 之後，此屬性接著便會在屬性檢閱器打開 (expose) 一個選項。

`@IBInspectable` properties provide new access to an old feature: user-defined runtime attributes. Currently accessible from the identity inspector, these attributes have been available since before Interface Builder was integrated into Xcode. They provide a powerful mechanism for configuring any key-value coded property of an instance in a NIB, XIB, or storyboard.

Built-in Cocoa types can also be extended to have inspectable properties beyond the ones already in Interface Builder’s attribute inspector. If you like rounded corners, you’ll love this UIView extension:

```swift
extension UIView {
    @IBInspectable var cornerRadius: CGFloat {
        get {
            return layer.cornerRadius
        }
        set {
            layer.cornerRadius = newValue
            layer.masksToBounds = newValue > 0
        }
    }
}
```

### @IBDesignable -> 修飾 class

另標示一個 UIview 是 IBDesignable，介面建構器會**即時渲染** (render) 整個自訂視圖。

The attribute `@IBDesignable` lets Interface Builder perform live updates on a particular view.
This allows seeing how your custom views will appear without building and running your app after each change.

To **mark a custom view** as `IBDesignable`, **prefix** the class name with `@IBDesignable` (or the `IB_DESIGNABLE` macro in Objective-C). 

Your initializers, layout, and drawing methods will be used to render your custom view right on the canvas:
```swift
@IBDesignable
class MyCustomView: UIView {
    ...
}
```

## Ways of specifing the layout of elements

Here are a few common ways to specify the layout of elements in a UIView:

- Using **Interface Builder**, you can add a `XIB file` to your project, layout elements within it, and then load the XIB in your application code (either automatically, based on naming conventions, or manually). Also, using InterfaceBuilder you can create a storyboard for your application.
- You can your own code to use **NSLayoutConstraints** to have elements in a view arranged by Auto Layout.
- You can create CGRects describing the exact coordinates for each element and pass them to UIView’s  
```objectivec  
  - (id)initWithFrame:(CGRect)frame method.
```
## Formula of Autolayout
<img src = "/Resources/Articles/Autolayout.png" width = 700>

## Size Classes
利用尺寸類別 (Size Classes) 建構自適應佈局　靈活為不同螢幕尺寸做開發

A size class is a new technology used by iOS to allow you to custom your app for a given device class, based on its **orientation** and **screen size**.

There are presently four size classes:  
- Horizontal Regular   
- Horizontal Compact  
- Vertical Regular  
- Vertical Compact  

<img src = "/Resources/Articles/Size%20Classes.png">

* can also **target specific UI elements*
You can also use Size Classes to **target specific UI elements** like **font colors**, **font sizes**, drop shadows, view colors and more to adapt to users’ device screen. These variations give you added control as to how your UI can adapt to different devices and orientations that your user may be using.

![截圖 2021-10-09 下午9 09 06](https://user-images.githubusercontent.com/45844869/136659057-4cfc7555-c93e-4ec5-923b-df417d502e4d.png)

## Intrinsic Content Size

依照 Intrinsic Content Size 去自動調整元件 Layout 的原則， 
view 依照 Intrinsic Content Size 壓縮/撐開 super view 的原則

The Intrinsic Content Size is one of the most powerful features you gain when you opt-in to using Auto Layout to describe your interfaces. When a view has an intrinsic content size, it is promising Auto Layout that it will have a predefined size that the engine can use to calculate and lay out its views

<center><img src = "/Resources/Articles/Intrisic%20Content%20Size.png" width="500"></center>

Auto Layout 將 Intrinsic Content Size 拆解成兩個概念
* **Content Hugging：** 當元件被拉伸時 (內容尺寸**比 super view 小**的時候)，**向內拉動**的力量
* Content Compression Resistance： 當元件被擠壓時 (內容尺寸**比 super view 大**的時候)，**向外推擠**的力量

<img src = "https://user-images.githubusercontent.com/45844869/136660431-8a57e955-f3f4-4fee-bcd2-e8b673153282.png" width = 500> 
<img src = "https://user-images.githubusercontent.com/45844869/136660446-8db6623a-ba02-488a-bd26-a56fbc314499.png" width = 600> 

https://zonble.gitbooks.io/kkbox-ios-dev/content/autolayout/intrinsic_content_size.html

## What’s the difference between the frame and the bounds?
`The bounds` of an UIView is the rectangle, expressed as a location (x,y) and size (width,height) relative to **its own coordinate system (0,0)** (可用來調整 subview 的位置）  
`The frame` of an UIView is the rectangle, expressed as a location (x,y) and size (width,height) relative to the **superview** it is contained within.  

<center><img src = "/Resources/Articles/Frame-Bounds.png"></center>

* **contentOffset:** 就是用來改變 bounds.size
* **Size:** frame 和 bounds 的 size 總是一樣，除非用了 transform
  > 改了 frame.size，bounds.size 也會跟著變
* **Transform:**
  修改 transform 是去動 frame，bounds 不會變。
  > 比如：
  > transform 旋轉後，frame 的大小是其外接矩形的大小。


## When bounds origin will be different from 0,0?

Let's take an example of `UIScrollView`:
UIScrollView's bounds.origin will not be (0, 0) when its contentOffset is not (0, 0).

### Transform
Transform 是調整 view 的 frame，所以 View 本身的 bounds 並沒有變化

> Use this property to scale or rotate the view's **frame** rectangle within its **superview's coordinate system**.

## What are layer objects and what do they represent?
Layer 跟 View 最大的差別：**Layer 不能響應使用者事件**

* `Layer objects` are data objects which represent **visual content**. 
* Layer objects are **used by views** to render their content. 
* Custom layer objects can also be added to the interface to implement complex animations and other types of sophisticated visual effects.

  ```mermaid 
  graph TD;
      
      UIView-->UIResponder;
      UIResponder-->NSObject;
      CALayer-->NSObject;
  ```

## File owner
https://www.jianshu.com/p/fb20d9239bf3

Two points to be remembered:
- The File owner is the object that loads the **nib**
  > i.e. that object which receives the message `loadNibNamed:` or `initWithNibName:`.
- If you want to access any objects in the nib after loading it, you can set an outlet in the file owner.

So you created a fancy view with lots of buttons, subviews etc . If you want to modify any of these views / objects any time after loading the nib FROM the loading object (usually a view or window controller) you set outlets for these objects to the file owner. It's that simple.

This is the reason why by default all View Controllers or Window Controllers act as file owners, and also have an outlet to the main window or view object in the nib file: because duh, if you're controlling something you'll definitely need to have an outlet to it so that you can send messages to it.

The reason it's called file owner and given a special place, is because unlike the other objects in the nib, the file owner is external to the nib and is not part of it. In fact, it only becomes available when the nib is loaded. So the file owner is a stand-in or proxy for the actual object which will later load the nib.

## App Life Cycle

`application:willFinishLaunchingWithOptions:`—This method is your app’s first chance to **execute code** at launch time.

`application:didFinishLaunchingWithOptions:`—This method allows you to perform any final initialization **before** your app is **displayed** to the user.

`applicationDidBecomeActive:`—Lets your app know that it is about to **become the foreground app**. Use this method for any last minute preparation.

`applicationWillResignActive`:—Lets you know that your app is transitioning away from being the foreground app. Use this method to put your app into a quiescent state.

`applicationDidEnterBackground`:—Lets you know that your app is now **running in the background** and **may be suspended** at any time.

`applicationWillEnterForeground`:—Lets you know that your app is moving out of the background and back into the foreground, but that it is not yet active.

`applicationWillTerminate`:—Lets you know that your app is being terminated. This method is not called if your app is suspended.
