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
> 可想像成 **轉接頭**
>
> 通過製作一個轉接頭，就可以在不改造設備內部結構的情況下直接使用該設備了。

An Adapter allows classes with incompatible interfaces to work together. It wraps itself around an object and exposes a standard interface to interact with that object.

[參考]("https://ios.devdon.com/archives/16")
當現有的系統需要繼承一個具有類似功能的新組件，而該組件沒有提供通用介面，並且無法修改改組件時，這時候就可以用上適配器了。

* **優點：** 將無法修改 source code 的組件集成到現有的項目中。
  > 在使用第三方框架、利用另一個項目所輸出的數據時，通常會遇到組件之間的兼容問題。
* **何時應該避免使用：** 如果可以修改所要集成的組件的 SourceCode 或者可以直接將該組件提供的 Data 直接遷移到現有的應用中的話。

## Memento Pattern
In Memento Pattern saves your stuff somewhere. Later on, this externalized state can be restored without violating encapsulation; that is, private data remains private. 

One of Apple’s specialized implementations of the Memento pattern is **Archiving**.

[Example]("https://refactoring.guru/design-patterns/memento/swift/example#example-0")

## Factory Method Pattern
Factory Method is used to replace class constructors, to abstract and hide objects initialization so that the type can be determined at runtime, and to hide and contain switch/if statements that determine the type of object to be instantiated.

## Responder Chain
A ResponderChain is a hierarchy of objects that have the opportunity to respond to events received.

https://refactoring.guru/design-patterns/chain-of-responsibility

## Observer Pattern
In the Observer pattern, one object notifies other objects of any state changes.

## Singleton Pattern
The Singleton design pattern ensures that only one instance exists for a given class and that there’s a global access point to that instance. It usually uses lazy loading to create the single instance when it’s needed the first time.

## Decorator Pattern
The Decorator pattern dynamically adds behaviors and responsibilities to an object without modifying its code. It’s an alternative to subclassing where you modify a class’s behavior by wrapping it with another object.

> **“Wrapper”** is the alternative nickname for the Decorator pattern that clearly expresses the main idea of the pattern.

https://refactoring.guru/design-patterns/decorator
[Example]("https://vinileal.com/design%20patterns/design-patterns-swift-decorator/")

## Facade (外觀）Pattern
The Facade design pattern provides a single interface to a complex subsystem. Instead of exposing the user to a set of classes and their APIs, you only expose one simple unified API.

[Example]("https://refactoring.guru/design-patterns/facade/swift/example")