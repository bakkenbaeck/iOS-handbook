# Swift Style Guide

Our overarching goals are conciseness, readability, and simplicity.


## Table of Contents

* [Naming](#naming)
  * [Class Prefixes](#class-prefixes)
* [Spacing and Indentation](#spacing-and-indentation)
* [Classes and Structs](#classes-and-structs)
  * [Use of Self](#use-of-self)
  * [Protocol Conformance](#protocol-conformance)
  * [Computed Properties](#computed-properties)
  * [Example definition](#example-definition)
* [Function Declarations](#function-declarations)
* [Closure Expressions](#closure-expressions)
* [Types](#types)
  * [Constants](#constants)
  * [Optionals](#optionals)
  * [Struct Initializers](#struct-initializers)
  * [Type Inference](#type-inference)
  * [Syntactic Sugar](#syntactic-sugar)
* [Semicolons](#semicolons)
* [Resources](#resources)
* [Attribution](#attribution)

## Naming

Use descriptive names with camel case for classes, methods, variables, etc. Class names should be capitalized, while method names, variables and constants should start with a lower case letter.

**Preferred:**

```swift
class WidgetContainer {
    var widgetButton: UIButton
    let widgetHeightPercentage = 0.85
}
```

**Not Preferred:**

```swift
let MAX_WIDGET_COUNT = 100

class app_widgetContainer {
    var wBut: UIButton
    let wHeightPct = 0.85
}
```

For functions and init methods, prefer named parameters for all arguments.

Declarations:

```swift
class Guideline {
    func combineWithString(incoming incoming: String, options: Dictionary?)
    func upvoteBy(amount amount: Int)
    func date(string string: String) -> NSDate
    func timedAction(delay: NSTimeInterval, action: SKAction) -> SKAction!
    func authenticate(username username: String, password: String, completion: (error: NSError?) -> Void)
}
```

Implementations:

```swift
date(string: "2014-03-14")

timedAction(delay: 1.0, action: someOtherAction) {

}

authenticate(username: "elvis", password: "m8eu201je") { error in

}
```

When in doubt, look at how Xcode lists the method in the jump bar – our style here matches that.

![Methods in Xcode jump bar](https://raw.githubusercontent.com/raywenderlich/swift-style-guide/master/screens/xcode-jump-bar.png)

### Class Prefixes

Swift types are all automatically namespaced by the module that contains them. As a result, prefixes are not required in order to minimize naming collisions. If two names from different modules collide you can disambiguate by prefixing the type name with the module name:

```swift
import MyModule

var myClass = MyModule.MyClass()
```

You **should not** add prefixes to your Swift types.

## Spacing and Indentation

* Indent using 4 spaces rather than tabs. This should be configured on the project.

  ![Xcode indent settings](https://raw.githubusercontent.com/bakkenbaeck/iOS-playbook/master/assets/xcode-text-settings-swift.png)

* Avoid doing method indentation since Swift's indentation is inconsistent, and just keep things in one line as long as is possible. It helps you to not having to think many times on how you would indent your code.

**Preferred:**
```swift
let user = User(name: "Igor", lastName: "Ranieri", country: "Germany")
```

**Not Preferred:**
```swift
let user = User(name: "Igor",
      lastName: "Ranieri",
      country: "Germany")
```

* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.
* Tip: You can re-indent by selecting some code (or ⌘A to select all) and then Control-I (or Editor\Structure\Re-Indent in the menu).

**Preferred:**
```swift
if user.isHappy {
    // Do something
} else {
    // Do something else
}
```

**Not Preferred:**
```swift
if user.isHappy
{
  // Do something
}
else {
  // Do something else
}
```

* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but having too many sections in a method often means you should refactor into several methods.

## Classes and Structs

Unless you require functionality that can only be provided by a class, implement a struct instead.

Additional capabilities of classes:

- Inheritance: Enables one class to inherit the characteristics of another
- Type casting: Enables you to check and interpret the type of a class instance at runtime
- Deinitializers: Enable an instance of a class to free up any resources it has assigned
- Reference counting: Allows more than one reference to a class instance
- Compatibility: Classes are available from Objetive-C

### Use of Self

Always use `self` when referencing properties. It will make your life simpler. Trust us.

### Protocol Conformance

When adding protocol conformance to a class, prefer adding a separate class extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods. Also don't use extensions to split code, but to extend classes or structs with additional functionality. Finally, avoid the `// MARK: -` it makes code too noisy.

**Preferred:**
```swift
class MyViewcontroller: UIViewController {
    // class stuff here
}

extension MyViewcontroller: UITableViewDataSource {
    // table view data source methods
}

extension MyViewcontroller: UIScrollViewDelegate {
    // scroll view delegate methods
}
```

**Not Preferred:**
```swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
    // all methods
}

// MARK: - Data download
extension MyViewController {

}
```

### Computed Properties

For conciseness, if a computed property is read-only, omit the get clause. The get clause is required only when a set clause is provided.

**Preferred:**
```swift
var diameter: Double {
    return radius * 2.0
}
```

**Not Preferred:**
```swift
var diameter: Double {
    get {
      return radius * 2.0
    }
}
```

### Example definition

Here's an example of a well-styled class definition:

```swift
class Circle: Shape {
    var x: Int
    var y: Int
    var radius: Double

    var diameter: Double {
        get {
            return self.radius * 2.0
        }
        set {
            radius = newValue / 2.0
        }
    }

    init(x: Int, y: Int, radius: Double) {
        self.x = x
        self.y = y
        self.radius = radius
    }

    convenience init(x: Int, y: Int, diameter: Double) {
        self.init(x: x, y: y, radius: diameter / 2.0)
    }

    func describe() -> String {
        return "I am a circle at \(centerString()) with an area of \(computeArea())"
    }

    override func computeArea() -> Double {
        return M_PI * self.radius * self.radius
    }

    private func centerString() -> String {
        return "(\(x),\(y))"
    }
}
```

The example above demonstrates the following style guidelines:

 + The correct spacing for variable assignations is with a space after and before the equals mark `=`, e.g. `x = 3`
 + Attributes in method signature have the `:` next to the name, e.g `init(x: Int, y: Int)` same with class inheritance and when using type inference
 + Indent getter and setter definitions and property observers
 + Don't add modifiers such as `internal` when they're already the default. Similarly, don't repeat the access modifier when overriding a method


## Function Declarations

Keep function declarations on one line including the opening brace, doesn't matter how long they are:

```swift
func reticulateSplines(spline: [Double]) -> Bool {
    // reticulate code goes here
}
```

```swift
func reticulateSplines(spline: [Double], adjustmentFactor: Double, translateConstant: Int, comment: String) -> Bool {
    // reticulate code goes here
}
```

## Closure Expressions

Use trailing closure syntax wherever possible. In all cases, give the closure parameters descriptive names:

```swift
return SKAction.customActionWithDuration(effect.duration) { node, elapsedTime in
    // more code goes here
}
```

For single-expression closures where the context is clear, use implicit returns:

```swift
attendeeList.sort { a, b in
    a > b
}
```


## Types

Always use Swift's native types when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.

**Preferred:**
```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```

**Not Preferred:**
```swift
let width: NSNumber = 120.0                          // NSNumber
let widthString: NSString = width.stringValue        // NSString
```

### Constants

Constants are defined using the `let` keyword, and variables with the `var` keyword. Any value that **is** a constant **must** be defined appropriately, using the `let` keyword. As a result, you will likely find yourself using `let` far more than `var`.

**Tip:** One technique that might help meet this standard is to define everything as a constant and only change it to a variable when the compiler complains!

### Optionals

Declare variables and function return types as optional with `?` where a nil value is acceptable.

Use implicitly unwrapped types declared with `!` only for instance variables that you know will be initialized later before use. Correct use of implicitly unwrapped is very rare so be careful with this, since it will crash your app if a nil value is found.

When accessing an optional value, use optional chaining if the value is only accessed once or if there are many optionals in the chain:

```swift
textContainer?.textLabel?.setNeedsDisplay()
```

Use optional binding when it's more convenient to unwrap once and perform multiple operations:

```swift
if let textContainer = self.textContainer {
    // do many things with textContainer
}
```

When naming optional variables and properties, avoid naming them like `optionalString` or `maybeView` since their optional-ness is already in the type declaration.

For optional binding, shadow the original name when appropriate rather than using names like `unwrappedView` or `actualLabel`.

**Preferred:**
```swift
var subview: UIView?

// later on...
if let subview = self.subview {
    // do something with unwrapped subview
}
```

**Not Preferred:**
```swift
var optionalSubview: UIView?

if let unwrappedSubview = optionalSubview {
    // do something with unwrappedSubview
}
```

### Struct Initializers

Use the native Swift struct initializers rather than the legacy CGGeometry constructors.

**Preferred:**
```swift
let bounds = CGRect(x: 40.0, y: 20.0, width: 120.0, height: 80.0)
var centerPoint = CGPoint(x: 96.0, y: 42.0)
```

**Not Preferred:**
```swift
let bounds = CGRectMake(40.0, 20.0, 120.0, 80.0)
var centerPoint = CGPointMake(96.0, 42.0)
```

Prefer the struct-scope constants `CGRect.infiniteRect`, `CGRect.nullRect`, etc. over global constants `CGRectInfinite`, `CGRectNull`, etc. For existing variables, you can use the shorter `.zeroRect`.

### Type Inference

The Swift compiler is able to infer the type of variables and constants. You can provide an explicit type via a type alias (which is indicated by the type after the colon), but in the majority of cases this is not necessary.

Prefer compact code and let the compiler infer the type for a constant or variable.

**Preferred:**
```swift
let message = "Click the button"
var currentBounds = computeViewBounds()
```

**Not Preferred:**
```swift
let message: String = "Click the button"
var currentBounds: CGRect = computeViewBounds()
```

**NOTE**: Following this guideline means picking descriptive names is even more important than before.

### Syntactic Sugar

Prefer the shortcut versions of type declarations over the full generics syntax.

**Preferred:**
```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**Not Preferred:**
```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

## Semicolons

Swift does not require a semicolon after each statement in your code. They are only required if you wish to combine multiple statements on a single line.

Do not write multiple statements on a single line separated with semicolons.

## Resources

In `Swift` it's a good practice to use extensions for accessing elements of asset catalogs, custom colors and fonts. It helps to avoid the error-prone practice of hardcoding strings into your code.

Use a `Theme.swift` that contains extensions for theming your app. When adding color extensions avoid adding the `Color` postfix to your color methods, also add some context of how this color is used, just be careful not to have one method per UI element.

Colors should be semantic and context aware, adding the controller or the flow to the color name helps a lot.

**Preferred:**
```swift
self.color = .onboardingButtonNormal
self.highlightedColor = .onboardingButtonHighlighted
```

**Not Preferred:**
```swift
self.color = .buttonNormal
self.highlightedColor = .buttonHighlighted
```

For example:

```swift
import Hex
import UIKit

extension UIColor {
    class func callToActionNormal() -> UIColor {
        return UIColor(hex: "A87CF2")
    }

    class func callToActionSelected() -> UIColor {
        return UIColor(hex: "A87CF2")
    }

    class func callToActionHighlighted() -> UIColor {
        return UIColor(hex: "A87CF2")
    }
}

extension UIFont {
    class func bold(size: Double) -> UIFont {
        return UIFont(name: "DINNextLTPro-Bold", size: CGFloat(size))!
    }

    class func light(size: Double) -> UIFont {
        return UIFont(name: "DINNextLTPro-Light", size: CGFloat(size))!
    }

    class func medium(size: Double) -> UIFont {
        return UIFont(name: "DINNextLTPro-Medium", size: CGFloat(size))!
    }

    class func regular(size: Double) -> UIFont {
        return UIFont(name: "DINNextLTPro-Regular", size: CGFloat(size))!
    }
}
```
