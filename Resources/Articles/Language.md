* [Language](#language)
  * [KVO](#kvo)
  * [KVC](#kvc)
  * [Difference between Delegate and KVO](#what-is-the-difference-between-delegate-and-kvo)
  * [What happens when you call autorelease on an object?](#what-happens-when-you-call-autorelease-on-an-object)
  * [By calling performSelector:withObject:afterDelay: is the object retained?](#by-calling-performselectorwithobjectafterdelay-is-the-object-retained)
  * [Selectors](#selectors)   
  * [Major differences between Objective-C and Swift](#major-differences-between-objective-c-and-swift)
  
  * [Swift](#swift)
    * [Access Levels](#access-levels)
    * [What is an in-out parameter?](#what-is-an-in-out-parameter)
    * [Lazy Stored Property vs Stored Property](#lazy-stored-property-vs-stored-property)
    * [Open class](#open-class)
    * [Final class](#final-class)
    * [Struct vs Class](#structs-vs-classes)
    * [Swift Standard Library Protocol](#swift-standard-library-protocol)
    * [Downcasting](#downcasting)
    * [Explain [weak self] and [unowned self]?](#weak-self-and-unowned-self)
    * [Lazy in Swift](#lazy-in-swift)
    * [What are failable and throwing initializers?](#what-are-failable-and-throwing-initializers)
    * [What are type aliases?](#what-are-type-aliases)
    * [Raw values and associated values](#in-swift-enumerations-whats-the-difference-between-raw-values-and-associated-values)
    * [Non-Escaping and Escaping Closures](#non-escaping-and-escaping-closures)
    * [Error handling in Swift](#how-should-one-handle-errors-in-swift)
    * [Guard benefits](#what-are-benefits-of-guard)
    * [What’s the complexity of the countElements function of a string and why?](#when-applied-to-strings-whats-the-complexity-of-the-countelements-function-and-why)
    * [What are designated and convenience initializers?](#what-are-designated-and-convenience-initializers)
    * [Swift Transforming Array functions](#swift-transforming-array-functions)
    * [Extensions](#extensions)
    * [What is defer?](#what-is-defer)
    * [What is the difference Any and AnyObject?](#what-is-the-difference-any-and-anyobject)   
    * [How will you make property’s getter available, but the property is settable only from within code in swift?](#how-will-you-make-propertys-getter-available-but-the-property-is-settable-only-from-within-code-in-swift)
    * [Property Wrappers](#property-wrappers)

    
  * [Objective-C](#objective-c)
    * [What is the difference between _ vs self. in Objective-C?](#what-is-the-difference-between-_-vs-self-in-objective-c)
    * [What are blocks in Objective-C?](#what-are-blocks-in-objective-c)
    * [“Strong” and “Weak” references](#strong-and-weak-references)
    * [Strong, Weak, Readonly and Copy](#what-is-the-difference-strong-weak-readonly-and-copy)
    * [@dynamic in Objective-C](#what-is-dynamic-in-objective-c)
    * [NSError object](#what-is-made-up-of-nserror-object)  

# Language

## KVO
KVO stands for `Key-Value Observing` and allows a controller or class to observe changes to a property value. In KVO, an object can ask to be notified of any changes to a specific property; either its own or that of another object.

## KVC 
KVC adds stands for `Key-Value Coding`. It’s a mechanism by which an object’s properties can be accessed using string’s at runtime rather than having to statically know the property names at development time.

## Selectors
In Objective-C, selector has two meanings. It can be used to refer simply to the name of a method when it’s used in a source-code message to an object. It also, though, refers to the unique identifier that replaces the name when the source code is compiled. Compiled selectors are of type SEL. All methods with the same name have the same selector. You can use a selector to invoke a method on an object—this provides the basis for the implementation of the target-action design pattern in Cocoa

```objectivec
[friend performSelector:@selector(gossipAbout:) withObject:aNeighbor]
is equivalent to
 [friend gossipAbout:aNeighbor]
 ```

## What is the difference between Delegate and KVO?
Both are ways to have relationships between objects. Delegation is a one-to-one relationship where one object implements a delegate protocol and another uses it and sends messages to it, assuming that those methods are implemented since the receiver promised to comply to the protocol. KVO is a many-to-many relationship where one object could broadcast a message and one or multiple other objects can listen to it and react. KVO does not rely on protocols. KVO is the first step and the fundamental block of reactive programming (RxSwift, ReactiveCocoa, etc.)

## By calling performSelector:withObject:afterDelay: is the object retained?
Yes, the object is retained. It creates a timer that calls a selector on the current threads run loop. It may not be 100% precise time-wise as it attempts to dequeue the message from
the run loop and perform the selector

## What happens when you call autorelease on an object?
When you send an object a autorelease message, its retain count is decremented by 1 at some stage in the future. The object is added to an autorelease pool on the current thread. The main thread loop creates an autorelease pool at the beginning of the function, and release it at the end. This establishes a pool for the lifetime of the task. However, this also means that any autoreleased objects created during the lifetime of the task are not disposed of until the task completes. This may lead to the taskʼs memory footprint increasing unnecessarily. You can also consider creating pools with a narrower scope or use NSOperationQueue with itʼs own autorelease pool. (Also important – You only release or autorelease objects you own.)

## Major differences between Objective-C and Swift
What Most People Say: “I prefer the look and feel of Swift and I certainly won’t miss Objective-C’s square brackets.”
What You Should Say: “While Objective-C is more flexible than Swift, Swift is designed to catch programming errors much earlier. For instance, many errors that would occur at runtime in Objective-C—like sending a message to an object that doesn’t implement it—are now checked at compile-time. Objective-C can accomplish all of the things Swift can, but Swift can do so more safely. Swift supports more programming language features, like optionals, tuples and generics.”
Why You Should Say It: The second answer demonstrates greater familiarity with the operational differences between Objective-C and Swift. It’s fairly easy to learn another programming language once you understand its strengths, weaknesses and compatibility with other languages.

## Swift

## Access Levels

1. Public

Enables entities to be processed with in any source file from their defining module, a source file from another module that imports the defining module.

2. Internal

Enables entities to be used within any source file from their defining module, but not in any source file outside of that module.

3. Private

Restricts the use of an entity to its own defining source file. Private access plays role to hide the implementation details of a specific code functionality.

## What is an in-out parameter?

In-out parameter lets you change the value of a function parameter from within the body of that function.

## Lazy Stored Property vs Stored Property

1. The closure associated to the lazy property is executed only if you read that property. So if for some reason that property is not used (maybe because of some decision of the user) you avoid unnecessary allocation and computation.
You can populate a lazy property with the value of a stored property.

2. You can use self inside the closure of a lazy property. If you need to use self inside the function. In fact, if you're using a class rather than a structure, you should also declare [unowned self] inside your function so that you don't create a strong reference cycle(check the code below).

```swift

import UIKit
import Foundation

class InterviewTest {
 var name: String
 lazy var greeting : String = { [unowned self] in
   return “Hello \(self.name)”
 }()
 
 init(name: String) {
  self.name = name
 }
}

let testObj = InterviewTest(name:”abhi”)
testObj.greeting

```
Note: Remember, the point of lazy properties is that they are computed only when they are first needed, after which their value is saved. So, if you call the iOSResumeDescription for the second time, the previously saved value is returned.

## Open class 
By adding the keyword open in front of the method name, we allow the method to being overridden

## Final class
By adding the keyword final in front of the method name, we prevent the method from being overridden

## Structs vs Classes
1. Inheritance.

Structures can't inherit in swift.

```swift
class Vehicle {
}

class Car : Vehicle {
}
```

2. Pass By

Swift structures pass by value and class instances pass by reference.

3. Thread Safety

Structs are thread-safe

## Swift Standard Library Protocol
There are a few different protocol. Equatable protocol, that governs how we can distinguish between two instances of the same type. That means we can analyze. If we have a specific value is in our array. The comparable protocol, to compare two instances of the same type and sequence protocol: prefix(while:) and drop(while:) [SE-0045].
Swift 4 introduces a new Codable protocol that lets us serialize and deserialize custom data types without writing any special code.

## What are failable and throwing initializers?  

Very often initialization depends on external data, this data can exist as it can not, for that Swift provides two ways to deal with this.
Failable initializers return nil of there is no data, and let the developer “create” a different path in the application based on that.
In other hand throwing initializers returns an error on initialization instead of returning nil.

## Downcasting
When we’re casting an object to another type in Objective-C, it’s pretty simple since there’s only one way to do it. In Swift, though, there are two ways to cast — one that’s safe and one that’s not .
as used for upcasting and type casting to bridged type
as? used for safe casting, return nil if failed
as! used to force casting, crash if failed. should only be used when we know the downcast will succeed.

## [weak self] and [unowned self]
unowned does the same as weak with one exception: The variable will not become nil and therefore the variable must not be an optional.
But when you try to access the variable after its instance has been deallocated. That means, you should only use unowned when you are sure, that this variable will never be accessed after the corresponding instance has been deallocated.
However, if you don’t want the variable to be weak AND you are sure that it can’t be accessed after the corresponding instance has been deallocated, you can use unowned.
By declaring it [weak self] you get to handle the case that it might be nil inside the closure at some point and therefore the variable must be an optional. A case for using [weak self] in an asynchronous network request, is in a view controller where that request is used to populate the view.

## Lazy in Swift
An initial value of the lazy stored properties is calculated only when the property is called for the first time. There are situations when the lazy properties come very handy to developers.

## Raw and associated values
This question tests the developer’s understanding of enumeration in Swift. Enumeration provides a type-safe method of working with a group of related values. Raw values are compile time-set values directly assigned to every case within an enumeration, as in the example detailed below:
```swift
enum Alphabet: Int {
case A = 1
case B
case C
}
```

In the above example code, case “A” was explicitly assigned a raw value integer of 1, while cases “B” and “C” were implicitly assigned raw value integers of 2 and 3, respectively. Associated values allow you to store values of other types alongside case values, as demonstrated below:
```swift
enum Alphabet: Int {
case A(Int)
case B
case C(String)
}
```

## What are type aliases?  

Swift comes with two type aliases to represent non-specific types “Any” and “AnyObject”.
AnyObject represents an instance of any class type and Any is the most generic representation of a type in Swift, it can represent an instance of absolutely any type including functions.

## Non-Escaping and Escaping Closures
The lifecycle of a non-escaping closure is simple:
Pass a closure into a function
The function runs the closure (or not)
The function returns
Escaping closure means, inside the function, you can still run the closure (or not); the extra bit of the closure is stored some place that will outlive the function. There are several ways to have a closure escape its containing function:
Asynchronous execution: If you execute the closure asynchronously on a dispatch queue, the queue will hold onto the closure for you. You have no idea when the closure will be executed and there’s no guarantee it will complete before the function returns.
Storage: Storing the closure to a global variable, property, or any other bit of storage that lives on past the function call means the closure has also escaped.

## What are designated and convenience initializers?

Designated initializers are:
- The CENTRAL point of initialization of a class.
- Classes MUST have at least one.
- Is the responsible for initializing stored properties.
- Is the responsible for calling super init.

Convenience initializers are:
- SECONDARY supporting initializers for a class.
- It can only call a designated initializer that is defined in the same class
- It can also call another convenience initializers defined in the same class
- They are not required, that sort of implied. they are just initializers that we can write for a CONVENIENCE use case
- In a class, They use the keyword “convenience” before the init keyword.

Finally here are 3 basic rules of class initialization:
1. Every class must have a designated initializer, if this class inherits from another, the designated initializer is responsible for calling the designated initializer of its immediate superclass.
2. Classes can have any number of convenience initializers, a convenience initializer must call another initializer from the same class, whether it is a designated initializer or another conveniences initializer.
3. Convenience initializers must ultimately call a designated initializer.

## How should one handle errors in Swift?
The method for handling errors in Swift differ a bit from Objective-C. In Swift, it's possible to declare that a function throws an error. It is, therefore, the caller's responsibility to handle the error or propagate it. This is similar to how Java handles the situation.

You simply declare that a function can throw an error by appending the throws keyword to the function name. Any function that calls such a method must call it from a try block.

```swift
func canThrowErrors() throws -> String

//How to call a method that throws an error
try canThrowErrors()

//Or specify it as an optional
let maybe = try? canThrowErrors()
```

## What are benefits of Guard?
There are two big benefits to guard. One is avoiding the pyramid of doom, as others have mentioned — lots of annoying if let statements nested inside each other moving further and further to the right. The other benefit is provide an early exit out of the function using break or using return.


## When applied to strings, what’s the complexity of the countElements function and why?
Comments: The String struct doesn’t provide a count or length property or method to count the number of characters it contains. Instead a global countElements<T>() function is available.


Solution: Swift strings support extended grapheme clusters. Each character stored in a string is a sequence of one or more unicode scalars that, when combined, produce a single human readable character. Since different characters can require different amounts of memory, and considering that an extreme grapheme cluster must be accessed sequentially in order to determine which character it represents, it’s not possible to know the number of characters contained in a string upfront, without traversing the entire string. For that reason, the complexity of the countElements function is O(n).

## In Swift enumerations, what’s the difference between raw values and associated values?
Raw values are used to associate constant (literal) values to enum cases. The value type is part of the enum type, and each enum case must specify a unique raw value (duplicate values are not allowed).

The following example shows an enum with raw values of type Int:

```swift
enum IntEnum : Int {
    case ONE = 1
    case TWO = 2
    case THREE = 3
}
```
An enum value can be converted to its raw value by using the rawValue property:

```swift
var enumVar: IntEnum = IntEnum.TWO
var rawValue: Int = enumVar.rawValue
```
A raw value can be converted to an enum instance by using a dedicated initializer:

```swift
var enumVar: IntEnum? = IntEnum(rawValue: 1)
```
Associated values are used to associate arbitrary data to a specific enum case. Each enum case can have zero or more associated values, declared as a tuple in the case definition:

```swift
enum AssociatedEnum {
    case EMPTY
    case WITH_INT(value: Int)
    case WITH_TUPLE(value: Int, text: String, data: [Float])
}
```

Whereas the type(s) associated to a case are part of the enum declaration, the associated value(s) are instance specific, meaning that an enum case can have different associated values for different enum instances.

## Swift Transforming Array functions  
→ map and flatMap— how to transform element.
→ filter— should an element be included?  
→ reduce— how to fold an element into an aggregate value  
→ sort and lexicographicCompare—in what order should two elements come?  
→ indexOf and contains—does this element match?  
→ minElement and maxElement—which is the min/max of two elements?  
→ elementsEqual and startsWith—are two elements equivalent?  
→ split—is this element a separator?  

## What is defer?
defer keyword provides a safe and easy way to declare a block that will be executed only when execution leaves the current scope. 

Defer is usually used to cleanup the code after execution. This might involve deallocating container, releasing memory or close the file or network handle. When put into the routine, code inside defer block is last to execute before routine exits. Routine in this case could refer to a function. Sometimes when function returns there are hanging threads, incomplete operation which needs to cleanup. Instead of having them scattered across function, we can group them together and put in the defer block. When function exits, cleanup code in the defer gets executed.

## Extensions
Extensions add new functionality to an existing class, structure, enumeration, or protocol type. This includes the ability to extend types for which you do not have access to the original source code. Extensions are similar to categories in Objective-C

Extensions in Swift can:  
- Add computed instance properties and computed type properties  
- Define instance methods and type methods  
- Provide new initializers  
- Define subscripts  
- Define and use new nested types  
- Make an existing type conform to a protocol  

## What is the difference Any and AnyObject?
According to Apple’s Swift documentation:
Any can represent an instance of any type at all, including function types and optional types.
AnyObject can represent an instance of any class type.

## How will you make property’s getter available, but the property is settable only from within code in swift?

The example below shows a version of the TrackedString structure in which the structure is defined with an explicit access level of public. The structure’s members (including the numberOfEdits property) therefore have an internal access level by default. You can make the structure’s numberOfEdits property getter public, and its property setter private, by combining the public and private(set) access-level modifiers:

<center><img src = https://cdn-images-1.medium.com/max/1600/1*3P7lXezMt2bFTA9U9UDF6Q.png width="500"></center>

## Property Wrappers

A property wrapper adds a layer of separation between code that manages how a property is stored and the code that defines a property. For example, if you have properties that provide thread-safety checks or store their underlying data in a database, you have to write that code on every property. When you use a property wrapper, you write the management code once when you define the wrapper, and then reuse that management code by applying it to multiple properties.

To define a property wrapper, you make a structure, enumeration, or class that defines a wrappedValue property. In the code below, the TwelveOrLess structure ensures that the value it wraps always contains a number less than or equal to 12. If you ask it to store a larger number, it stores 12 instead.

```swift
@propertyWrapper
struct TwelveOrLess {
    private var number: Int
    init() { self.number = 0 }
    var wrappedValue: Int {
        get { return number }
        set { number = min(newValue, 12) }
    }
}
```
The setter ensures that new values are less than 12, and the getter returns the stored value.

## Objective-C

## What is the difference between _ vs self. in Objective-C?

You typically use either when accessing a property in Objective-C. When you use _, you're referencing the actual instance variable directly. You should avoid this. Instead, you should use self. to ensure that any getter/setter actions are honored. 

In the case that you would write your own setter method, using _ would not call that setter method. Using self. on the property, however, would call the setter method you implemented. 


## What are blocks in Objective-C?

Blocks are a language-level feature of Objective (C and C++ too). They are objects that allow you to create distinct segments of code that can be passed around to methods or functions as if they were values. This means that a block is capable of being added to collections such as NSArray or NSDictionary. Blocks are also able to take arguments and return values similar to methods and functions.

The syntax to define a block literal uses the caret symbol(^):

```

^{
  NSLog(@"This is an example of a block")
}

```

## Please explain Method Swizzling

Method swizzling allows the implementation of an existing selector to be switched at runtime for a different implementation in a classes dispatch table. Swizzling allows you to write code that can be executed before and/or after the original method. For example perhaps to track the time method execution took, or to insert log statements.  

```objectivec
#import "UIViewController+Log.h"
@implementation UIViewController (Log)
    + (void)load {
        static dispatch_once_t once_token;
        dispatch_once(&once_token,  ^{
            SEL viewWillAppearSelector = @selector(viewDidAppear:);
            SEL viewWillAppearLoggerSelector = @selector(log_viewDidAppear:);
            Method originalMethod = class_getInstanceMethod(self, viewWillAppearSelector);
            Method extendedMethod = class_getInstanceMethod(self, viewWillAppearLoggerSelector);
            method_exchangeImplementations(originalMethod, extendedMethod);
        });
    }
    - (void) log_viewDidAppear:(BOOL)animated {
        [self log_viewDidAppear:animated];
        NSLog(@"viewDidAppear executed for %@", [self class]);
    }
@end
```
## "Strong” and “weak” references
By default, any variable that points to another object does so with what is referred to as a “strong” reference. A retain cycle occurs when two or more objects have reciprocal strong references (i.e., strong references to each other). Any such objects will never be destroyed by ARC (iOS’ Automatic Reference Counting). Even if every other object in the application releases ownership of these objects, these objects (and, in turn, any objects that reference them) will continue to exist by virtue of those mutual strong references. This therefore results in a memory leak.

Reciprocal strong references between objects should therefore be avoided to the extent possible. However, when they are necessary, a way to avoid this type of memory leak is to employ weak references. Declaring one of the two references as weak will break the retain cycle and thereby avoid the memory leak.

To decide which of the two references should be weak, think of the objects in the retain cycle as being in a parent-child relationship. In this relationship, the parent should maintain a strong reference (i.e., ownership of) its child, but the child should not maintain maintain a strong reference (i.e., ownership of) its parent.

For example, if you have Employer and Employee objects, which reference one another, you would most likely want to maintain a strong reference from the Employer to the Employee object, but have a weak reference from the Employee to the Employer.

## What is the difference strong, weak, readonly and copy?
- `Strong` means that the reference count will be increased and the reference to it will be maintained through the life of the object
- `Weak` means that we are pointing to an object but not increasing its reference count. It’s often used when creating a parent child relationship. The parent has a strong reference to the child but the child only has a weak reference to the parent.
- `Readonly`, we can set the property initially but then it can’t be changed.
- `Copy` means that we’re copying the value of the object when it’s created. Also prevents its value from changing.

## What is dynamic in Objective-C?
`@dynamic` used to delegate the responsibility of implementing the accessors.
Dynamic for properties means that it setters and getters will be created manually and/or at runtime.

## What is made up of NSError object?
There are three parts of NSError object a domain, an error code, and a user info dictionary. The domain is a string that identifies what categories of errors this error is coming from.
