# Bakken & Bæck's iOS Playbook

* [Swift Style Guide](#swift-style-guide)
* [Best Practices](#best-practices)
* [Components](#components)
* [Project Structure](#project-structure)
* [Releases](#releases)

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
* [Control Flow](#control-flow)
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

For functions and init methods, prefer named parameters for all arguments unless the context is very clear. Include external parameter names if it makes function calls more readable.

```swift
func dateFromString(dateString: String) -> NSDate { ... }
func convertPointAt(column: Int, row: Int) -> CGPoint { ... }
func timedAction(delay: NSTimeInterval, action: SKAction) -> SKAction! { ... }

// would be called like this:
dateFromString("2014-03-14") { ... }
convertPointAt(column: 42, row: 13) { ... }
timedAction(delay: 1.0, action: someOtherAction) { ... }
```

For methods, follow the standard Apple convention of referring to the first parameter in the method name:

```swift
class Guideline {
    func combineWithString(incoming: String, options: Dictionary?) { ... }
    func upvoteBy(amount: Int) { ... }
}
```

When referring to functions in prose include the required parameter names from the caller's perspective. If the context is clear and the exact signature is not important, you can use just the method name.

> Call `convertPointAt(column:row:)` from your own `init` implementation.
>
> If you implement `timedAction`, remember to provide an appropriate delay value.
>
> You shouldn't call the data source method `tableView(_:cellForRowAtIndexPath:)` directly.

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

* Indent using 4 spaces rather than tabs to conserve space and help prevent line wrapping. This should be configured on the project.

  ![Xcode indent settings](https://raw.githubusercontent.com/bakkenbaeck/iOS-playbook/master/assets/xcode-text-settings-swift.png)

* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.
* Tip: You can re-indent by selecting some code (or ⌘A to select all) and then Control-I (or Editor\Structure\Re-Indent in the menu). Some of the Xcode template code will have 4-space tabs hard coded, so this is a good way to fix that.

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

Use `self` when required, for example:

- When using optional binding with optional properties

**Preferred:**

```swift
if let textContainer = self.textContainer {
    // do many things with textContainer
}
```

**Not Preferred:**

```swift
if let maybeThisCouldBeTextContainer = textContainer {
    // do many things with maybeThisCouldBeTextContainer
}
```

- To differentiate between property names and arguments in initializers
- When referencing properties in closure expressions

```swift
init(row: Int, column: Int) {
    self.row = row
    self.column = column

    let closure = {
      println(self.row)
    }
}
```

### Protocol Conformance

When adding protocol conformance to a class, prefer adding a separate class extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

Also, don't forget the `// MARK: -` comment to keep things well-organized!

**Preferred:**
```swift
class MyViewcontroller: UIViewController {
    // class stuff here
}

// MARK: - UITableViewDataSource

extension MyViewcontroller: UITableViewDataSource {
    // table view data source methods
}

// MARK: - UIScrollViewDelegate

extension MyViewcontroller: UIScrollViewDelegate {
    // scroll view delegate methods
}
```

**Not Preferred:**
```swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
    // all methods
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
            return radius * 2.0
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
        return M_PI * radius * radius
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
let width: NSNumber = 120.0                                 // NSNumber
let widthString: NSString = width.stringValue               // NSString
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
if let subview = subview {
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


## Control Flow

Prefer the `for-in` style of `for` loop over the `for-condition-increment` style.

**Preferred:**
```swift
for _ in 0..<3 {
    println("Hello three times")
}

for (index, person) in enumerate(attendeeList) {
    println("\(person) is at position #\(index)")
}
```

**Not Preferred:**
```swift
for var i = 0; i < 3; i++ {
    println("Hello three times")
}

for var i = 0; i < attendeeList.count; i++ {
    let person = attendeeList[i]
    println("\(person) is at position #\(i)")
}
```


## Semicolons

Swift does not require a semicolon after each statement in your code. They are only required if you wish to combine multiple statements on a single line.

Do not write multiple statements on a single line separated with semicolons.

The only exception to this rule is the `for-conditional-increment` construct, which requires semicolons. However, alternative `for-in` constructs should be used where possible.


## Resources

In `Swift` it's a good practice to use extensions for accessing elements of asset catalogs, custom colors and fonts. It helps to avoid the error-prone practice of hardcoding strings into your code.

### Colors

```swift
extension UIColor {
    class func applicationColor() -> UIColor {
        return UIColor(hex: "AB76F5")
    }
}
```

### Fonts

```swift
extension UIFont {
    class func bold(size: Double) -> UIFont {
        return UIFont(name: "DINNextLTPro-Bold", size: CGFloat(size))!
    }
}
```


# Best practices

## Table of Contents

* [Xcode](#xcode)
* [Deployment](#deployment)
* [Comments](#comments)
* [Blocks, delegates or data source](#blocks-delegates-or-data-source)
* [View controllers](#view-controllers)
* [Assets](#assets)

## Xcode

### Version

The recommended version of Xcode (and Swift) for all purposes is the current available [in the App Store](https://itunes.apple.com/no/app/xcode/id497799835?mt=12).

### Warnings

Enable warnings by adding `-Weverything` to your Build Settings under "Other Compiler Flags". If you need to ignore a specific warning you can use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas) or add `-Wno-warning-to-be-disabled` (for example `-Wno-gnu-conditional-omitted-operand`).

### Plugin compatibility

After upgrading to a new Xcode version plugins will become disabled until their list of compatible Xcode versions gets updated. In case you can't wait for an official update by the plugins' authors you can try to run [this script](https://gist.github.com/neonichu/9487584/download#). Be sure to find out your Xcode's version UUID first and update it in the script.

You can get your Xcode version UUID by running
```shell
/usr/libexec/PlistBuddy -c 'Print DVTPlugInCompatibilityUUID' "$(xcode-select -p)/../Info.plist"
```


## Deployment

### Semantic Versioning

We support [semantic versioning](http://semver.org/), and it's important that minor releases are backwards compatible otherwise don't feel shy to make it a major release.

When making backwards compatible changes, flag your old APIs as deprecated like this:

```objc
- (NSInteger)foo:(NSInteger)bar __attribute__((deprecated("Use fooWithBar: instead")));
```


## Comments

When they are needed, comments should be used to explain **why** a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. This does not apply to those comments used to generate documentation.

**Comments style**

One-line:
```swift
// Workaround: This is the comment
```

Multiple-lines:

```swift
/*
   Workaround: This is a comment that spans multiple lines.
   Comments should be added when they are not only needed but critical
   to understand the underlying block of code.
 */
```

**For example**

```objc
- (void)viewWillAppear:(BOOL)animated
{
    [super viewWillAppear:animated];

    /*
     Workaround: Selected cell only gets deselected when pressing the back button
     dragging the screen to go back doesn't deselect the selected cell.
     So, `self.clearsSelectionOnViewWillAppear = YES;` only works sometimes.
     */
    NSIndexPath *selectedIndexPath = [self.tableView indexPathForSelectedRow];
    if (selectedIndexPath) {
        [self.tableView deselectRowAtIndexPath:selectedIndexPath animated:YES];
    }
}
```


## Blocks, delegates or data source

### Block
- Asynchronous (For example: networking operations)
- User inputs with multiple options (For example: UIAlertView's YES and NO)
- Data source driven inputs (For example: A table items with action blocks that were defined in the data source)
- Returns many values (For example looking for a field in a collection and returning the field and the indexPath)
- If there’s no tracked state or if state it’s defined in the same method

### Delegate
- Synchronous (For example: buttons actions in views that should perform on their parents)
- Shouldn't return values
- Provides control over performing an action (For example: UITextField's shouldEndEditing)
- User input with one action (For example: buttons actions in views that should perform on their parents)
- If tracked state is shared (if state is stored in a property or a constant)

### Data source
- Returns ONE value

## View controllers

### Naming

When naming subclasses of `UIViewController` or friends `UIPageViewController`, `UICollectionViewController`, `UITableViewController`, you don't have to use the `ViewController` suffix.

For example instead of `BBRecipesTableViewController` you would do `BBRecipesController`, this applies for both Objective-C and Swift.

### Presenting and dismissing View Controllers

- It's better practice to call `dismissViewControllerAnimated:completion:` in the `UIViewController` that did the presenting, not in the `UIViewController` that was presented.

## Assets

### Images

Image names should be named consistently to preserve organization and developer sanity. They should be named as one [lower camel case](http://c2.com/cgi/wiki?LowerCamelCase) string with a description of their purpose, followed by the un-prefixed name of the class or property they are customizing (if there is one), followed by a further description of color and/or placement, and finally their state.

**For example:**

* `refreshBarButtonItem` / `refreshBarButtonItem@2x` and `refreshBarButtonItemSelected` / `refreshBarButtonItemSelected@2x`
* `articleNavigationBarWhite` / `articleNavigationBarWhite@2x` and `articleNavigationBarBlackSelected` / `articleNavigationBarBlackSelected@2x`.

Images should live in `Images.xcassets`.

## Networking

Completion blocks in networking calls should be returned in the main thread.

Completion blocks should contain the `error` instead of success/failure blocks.

# Components

Every agnostic component (UI control, category, helper, etc) should be created in a separate repository and in Swift.

Your component should be unit tested and documented.

# Steps to create a component

- [ ] Create repo on GitHub
- [ ] Go to your Projects folder
- [ ] Run `git clone https://github.com/bakkenbaeck/pod-template <PODNAME>`
- [ ] Run `cd <PODNAME>`
- [ ] Run `./init.rb <PODNAME>`
- [ ] [Enable Travis for your repository](https://travis-ci.org/profile/bakkenbaeck). If your component doesn't show up press "Sync Now"
- [ ] To test your pod in another project while developing it you have to make sure all changes of your pod are commited and synced, then make sure `pod '<YOUR_PODS_NAME>', :git => 'https://github.com/bakkenbaeck/<YOUR_PODS_NAME>.git` is in the other project's Podfile. Running `pod install <YOUR_PODS_NAME>` (first time) or `pod update <YOUR_PODS_NAME>` (after each change to your pod) in the other project is required
- [ ] Add your beautiful code and make a `Initial implementation` pull request
- [ ] Publish your pod by running `pod trunk push`(*)
- [ ] Add Bakken & Bæck as co-owner by running `pod trunk add-owner <PODNAME> post@bakkenbaeck.com`
- [ ] :cake:

(*) If you don't have a CocoaPods account you can create one by following [this steps](http://guides.cocoapods.org/making/getting-setup-with-trunk.html#getting-started).

# Steps to ship your component

Before shipping your component make sure that the README states what makes your Pod different than the other ones, been made by you might not be enough. It's very important that your README is :star2: fabulous :star2:.

Make sure to provide a super simple example on how to get your Pod up and running, if it is dependent on other Pods you should explain why you need them and what they do. Don't assume that people know everything.

If it's a visual component include a `.gif` showing how does it work or what does it do. It helps a lot for people to understand your Pod without having to clone, build and run your project. A good tool to make gifs is [Licecap](http://www.cockos.com/licecap/).

- [ ] Make sure to have a cool logo
- [ ] Submit it to [Cocoa Controls](https://www.cocoacontrols.com/)
- [ ] Make a PR to [iOS Goodies](https://github.com/iOS-Goodies/iOS-Goodies)
- [ ] Submit your component to [Hacker News](https://news.ycombinator.com/), your post should start with **Show HN**

**Send a few tweets from the @bakkenbaeck account**

Always express yourself like you are part of a team, use **"We did this"** and never **"I did this"**.
Take the time to compose personal tweets to all your recipients, copy pasting one message to everyone makes you sound like a robot and feels spammy, don't do this.

- [ ] Send a tweet from the [@bakkenbaeck](https://twitter.com/bakkenbaeck) account with a tiny summary and attach the logo of the Pod
- [ ] Send a tweet to [@maniacdev](https://twitter.com/maniacdev)
- [ ] Send a tweet to [@daveverwer](https://twitter.com/daveverwer) and [@iOSDevWeekly](https://twitter.com/iOSDevWeekly)
- [ ] Send a tweet to [@iOSGoodies](https://twitter.com/iOSGoodies) with the link to your PR
- [ ] Send a tweet to [@NatashaTheRobot](https://twitter.com/NatashaTheRobot)

# Project Structure

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped by type and feature for greater clarity.

```
ProjectName
|
|— Vendor # 3rd party code that doesn't use CocoaPods
|—— METCalendarViewer
|—— LZOPhotoViewer
|
|— Library # Anything that falls outside of the MVC pattern
|—— Views
|—— Networking
|—— Categories
|—— Containers
|—— Base
|—— Managers
|—— Utils (better if can be a category)
|
|— Source
|
|—— AppDelegate
|
|—— Recipes
|——— Views
|——— Controllers
|——— Networking
|
|—— Ingredients
|——— Views
|——— Controllers
|——— Networking
|
|—— Comments
|——— Views
|——— Controllers
|——— Networking
|
|—— Models # Human files. Usually generated by Mogenerator
|——— Autogenerated # machine files, generated by Mogenerator
|
|— Tests
|—— Helpers
|—— Models
|—— Controllers
|—— Networking
|—— Resources // Mocked JSON responses
|
|— Resources
|—— Storyboards
|—— Images.xcassets
|—— JSONs
|—— Sounds
|—— Others
```

# Releases

Steps for releasing a new version of your app:

1. Switch to the `master` branch
2. Bump the projects version number (make sure to use [semantic versioning](http://semver.org/))
3. Create and upload the archive to HockeyApp
4. Commit and push your changes to the `master` branch
5. Create a [release on GitHub](https://help.github.com/articles/creating-releases/). If the release uses the `staging` server, mark it as `pre-release`
