* [Patterns](#patterns)
  * [Adapter Pattern](#adapter-pattern)
  * [Memento Pattern](#memento-pattern)
  * [Factory Method Pattern](#factory-method-pattern)
  * [Responder Chain](#responder-chain)
  * [Observer Pattern](#observer-pattern)
  * [Singleton Pattern](#singleton-pattern)
  * [Decorator Pattern](#decorator-pattern)
  * [Facade Pattern](#facade-pattern)  

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
