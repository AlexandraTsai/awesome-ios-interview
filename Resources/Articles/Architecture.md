* [Architecture](#architecture)
  * [MVC](#mvc)
  * [MVVM](#mvvm)
  * [MVP](#mvp)
  * [Viper](#viper)  

# Architecture

## MVC
MVC stands for **Model-View-Controller**. It is a software architecture pattern for implementing user interfaces. 

MVC consists of three layers: the model, the view, and the controller.
- The **model layer** is typically where the data resides (persistence, model objects, etc)
- The **view layer** is typically where all the UI interface lies. Things like displaying buttons and numbers belong in the view layer. The view layer does not know anything about the model layer and vice versa.
- The **controller (view controller)** is the layer that integrates the view layer and the model layer together. 

## MVVM

The MVVM defines the following
·      The View , which is generally passive, corresponds to the Presentation Layer
·      The Model , corresponds to the Business Logic Layer and is identical to the MVC pattern
·      The View Model sits between the View and the Model , corresponds to the Application Logic Layer

The key aspect of the MVVM pattern is the binding between the View and the View Model.  In other words, the View is automatically notified of changes to the View Model.

In iOS, this can be accomplished using Key-Value-Observer (KVO) Pattern. In the KVO Pattern (https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html) , one object is automatically notified of changes to the state of another object. In Objective C, this facility is built into the run-time. However, it’s not as straightforward in Swift. One option would be to add the “dynamic” modifiers to properties that need to be dynamically dispatched. However, this is limiting as objects now need to be of type NSObject.  The alternate is to simulate this behavior by defining a generic type that acts as a wrapper around properties that are observable.

## MVP
The MVP defines the following
·      The View , which is generally passive, corresponds to the  Presentation Layer
·      The Model , corresponds to the  Business Logic Layer and is identical to the MVC pattern
·      The Presenter sits between the View and the Model , corresponds to the Application Logic Layer.
A View is typically associated with one Presenter.

In iOS, the interaction between the View and Presenter can be implemented using the Delegation Pattern (https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html). The Delegation Pattern allows one object to delegate tasks to another object. In iOS, the pattern is implemented using a Protocol. The Protocol defines the interface that is to be implemented by the delegate.

In the MVP pattern, the View and Presenter are aware of each other. The Presenter holds a reference to the view it is associated with.

The PresenterProtocol defines the base set of methods that any Presenter must implement. Applications must extend this Protocol to include application specific methods.

```swift
protocol PresenterProtocol: class{
    func attachPresentingView(_view:PresentingViewProtocol)
    func detachPresentingView(_view:PresentingViewProtocol)
}
```

The PresentingViewProtocol  defines the base set of methods that View must implement. By providing default implementation of the methods in this interface, the conformant view does not have to provide its own implementation. This interface can be extended to define application specific methods.

```swift
protocol PresentingViewProtocol: class{
    func dataStartedLoading()
    func dataFinishedLoading()
    func showErrorAlertWithTitle(_title:String?, message:String)
    func showSuccessAlertWithTitle(_title:String?, message:String)
}
```

## Viper

This architecture is based on Single Responsibility Principle which leads to a clean architecture.

- View: The responsibility of the view is to send the user actions to the presenter and shows whatever the presenter tells it.
- Interactor: This is the backbone of an application as it contains the business logic.
- Presenter: Its responsibility is to get the data from the interactor on user actions and after getting data from the interactor, it sends it to the view to show it. It also asks the router/wireframe for navigation.
- Entity: It contains basic model objects used by the Interactor.
- Router: It has all navigation logic for describing which screens are to be shown when. It is normally written as a wireframe.
