* [Tasks](#tasks)
  * [Explain why a compile time error occurs](#explain-why-a-compile-time-error-occurs-how-can-you-fix-it)
  * [Spot the bug that occurs in the code](#spot-the-bug-that-occurs-in-the-code)
  * [Consider the following code](#consider-the-following-code)
  * [Determine the value of “x” in the Swift code](#determine-the-value-of-x-in-the-swift-code-below)
  * [Given two sorted arrays, the task is to merge them in a sorted manner](#given-two-sorted-arrays-the-task-is-to-merge-them-in-a-sorted-manner)  
  * [Write down the custom method which works similar to reduce() api?](#write-down-the-custom-method-which-works-similar-to-reduce-api)
  
# Tasks

## Explain why a compile time error occurs. How can you fix it?
Task:
The following code snippet results in a compile time error:
```objectivec  
struct IntStack {
  var items = [Int]()
  func add(x: Int) {
    items.append(x) // Compile time error here
  }
}
```

Solution:
Structures are value types. By default, the properties of a value type cannot be modified from within its instance methods.
However, you can optionally allow such modification to occur by declaring the instance methods as ‘mutating’:
```objectivec  
struct IntStack {
  var items = [Int]()
  mutating func add(x: Int) {
    items.append(x) // All good!
  }
}
```

## Spot the bug that occurs in the code

```objectivec
@interface MyCustomController : UIViewController  

@property (strong, nonatomic) UILabel *alert;  

@end  

@implementation MyCustomController  

- (void)viewDidLoad {
  CGRect frame = CGRectMake(100, 100, 100, 50);
  self.alert = [[UILabel alloc] initWithFrame:frame];
  self.alert.text = @"Please wait...";
  [self.view addSubview:self.alert];
  dispatch_async(
    dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0),
    ^{
      sleep(10);
      self.alert.text = @"Waiting over";
    }
  );
}  
```

Solution:
All UI updates must be performed on the main thread. Global dispatch queues do not make any guarantees so code should be modified to run the UI update on the main thread. Here is the fix below:

```objectivec
dispatch_async(		
dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0),
^{
sleep(10);
dispatch_async(dispatch_get_main_queue(), ^{
self.alert.text = @"Waiting over";
});
});
```

## Consider the following code:
Task:
```objectivec  
var defaults = UserDefaults.standard()
var userPref = defaults.stringForKey("userPref")!
printString(userPref)

func printString(string: String) {
    print(string)
}
```

Solution:
The second line uses the stringForKey method of UserDefaults, which returns an optional, to account for the key not being found, or for the corresponding value not being convertible to a string.

During its execution, if the key is found and the corresponding value is a string, the above code works correctly. But if the key doesn’t exist, or the corresponding value is not a string, the app crashes with the following error:

fatal error: unexpectedly found nil while unwrapping an Optional value
The reason is that the forced unwrapping operator ! is attempting to force unwrap a value from a nil optional. The forced unwrapping operator should be used only when an optional is known to contain a non-nil value.

The solution consists of making sure that the optional is not nil before force-unwrapping it:
```objectivec  
let userPref = defaults.stringForKey("userPref")
if userPref != nil {
    printString(userPref!)
}
```

## Determine the value of “x” in the Swift code below
Task:
```swift  
var a1 = [1, 2, 3, 4, 5]
var a2 = a1
a2.append(6)
var x = a1.count
```

Solution:
In Swift, arrays are implemented as structs, making them value types rather than reference types (i.e., classes). When a value type is assigned to a variable as an argument to a function or method, a copy is created and assigned or passed. As a result, the value of “x” or the count of array “a1” remains equal to 5 while the count of array “a2” is equal to 6, appending the integer “6” onto a copy of the array “a1.” The arrays appear in the box below.
```swift
a1 = [1, 2, 3, 4, 5]  
a2 = [1, 2, 3, 4, 5, 6]  
```

## Given two sorted arrays, the task is to merge them in a sorted manner

Input  :  arr1 = [1, 3, 4, 5]  
          arr2 = [2, 4, 6, 8]
Output :  arr3 = [1, 2, 3, 4, 5, 6, 7, 8]

1. Method 1 (O(n1 * n2) Time and O(1) Extra Space)

Create an array arr3 of size n1 + n2.
Copy all n1 elements of arr1 to arr3
Traverse arr2 and one by one insert elements (like insertion sort) of arr3 to arr1. This step take O(n1 * n2) time.

2. Method 2 (O(n1 + n2) Time and O(n1 + n2) Extra Space)
The idea is to use Merge function of Merge sort.

Create an array arr3 of size n1 + n2.
Simultaneously traverse arr1 and arr2.
Pick smaller of current elements in arr1 and arr2, copy this smaller element to next position in arr3 and move ahead in arr3 and the array whose element is picked.
If there are are remaining elements in arr1 or arr2, copy them also in arr3.

## Write down the custom method which works similar to reduce() api
```swift
sum = array.reduce(0, +)
//reduce() here is an ((Int, ((Int, Int) -> Bool)) -> Int)
//and the + operator is func +(lhs: Int, rhs: Int) -> Bool,
//... or ((Int, Int) -> Bool), so there's no need to define reduce's closure.
```
