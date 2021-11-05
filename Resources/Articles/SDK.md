* [SDK](#sdk)
  * [Application States](#application-states)  

# SDK

## Application States
The iOS application states are as follows:

Not running state: The app has not been launched or was running but was terminated by the system.  

#### Inactive state:
The app is running in the foreground but is currently not receiving events. (It may be executing other code though.) An app usually stays in this state only briefly as it transitions to a different state. The only time it stays inactive for any period of time is when the user locks the screen or the system prompts the user to respond to some event (such as an incoming phone call or SMS message).  

#### Active state: 
The app is running in the foreground and is receiving events. This is the normal mode for foreground apps.  

#### Background state:
The app is in the background and executing code. Most apps enter this state briefly on their way to being suspended. However, an app that requests extra execution time may remain in this state for a period of time. In addition, an app being launched directly into the background enters this state instead of the inactive state.  

#### Suspended state:
While suspended, an app remains in memory but does not execute any code. When a low-memory condition occurs, the system may purge suspended apps without notice to make more space for the foreground app.

# Patterns

## Delegation pattern

The delegation pattern is a powerful pattern used in building iOS applications. The basic idea is that one object will act on another object's behalf or in coordination with another object. The delegating object typically keeps a reference to the other object (delegate) and sends a message to it at the appropriate time. It is important to note that they have a one to one relationship.

## Adapter Pattern
An Adapter allows classes with incompatible interfaces to work together. It wraps itself around an object and exposes a standard interface to interact with that object.

## Memento Pattern
In Memento Pattern saves your stuff somewhere. Later on, this externalized state can be restored without violating encapsulation; that is, private data remains private. One of Apple’s specialized implementations of the Memento pattern is Archiving.

## Factory Method Pattern
Factory Method is used to replace class constructors, to abstract and hide objects initialization so that the type can be determined at runtime, and to hide and contain switch/if statements that determine the type of object to be instantiated.

## Responder Chain
A ResponderChain is a hierarchy of objects that have the opportunity to respond to events received.

## Observer Pattern
In the Observer pattern, one object notifies other objects of any state changes.

## Singleton Pattern
The Singleton design pattern ensures that only one instance exists for a given class and that there’s a global access point to that instance. It usually uses lazy loading to create the single instance when it’s needed the first time.

## Decorator Pattern
The Decorator pattern dynamically adds behaviors and responsibilities to an object without modifying its code. It’s an alternative to subclassing where you modify a class’s behavior by wrapping it with another object.

## Facade Pattern
The Facade design pattern provides a single interface to a complex subsystem. Instead of exposing the user to a set of classes and their APIs, you only expose one simple unified API.
