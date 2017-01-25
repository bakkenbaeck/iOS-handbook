# Best Practices & Style Guide

## Table of Contents

* [Xcode](#xcode)
* [Versioning](#versioning)
* [Comments](#comments)
* [Blocks, delegates and data source](#blocks-delegates-and-data-source)
* [View controllers](#view-controllers)
* [Assets](#assets)
* [Property Observing](#property-observing)
* [Networking](#networking)
* [View layout](#view-layout)
* [Optional Force Unwrapping](#optional-force-unwrapping)
* [Lazy loading](#lazy-loading)
* [Swift style guide](#swift-style-guide)
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

## Xcode

### Version

The recommended version of Xcode (and Swift) for all purposes is the current available [in the App Store](https://itunes.apple.com/no/app/xcode/id497799835?mt=12).

### Warnings

Enable warnings by adding `-Weverything` to your Build Settings under "Other Compiler Flags". If you need to ignore a specific warning you can use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas) or add `-Wno-warning-to-be-disabled` (for example `-Wno-gnu-conditional-omitted-operand`).

## Versioning

To version our open source projects we use [semantic versioning](http://semver.org/), and it's important that minor releases are backwards compatible otherwise don't feel shy to make it a major release.

When making backwards compatible changes, flag your old APIs as deprecated like this:

```swift
@available(*, deprecated: 4.3.0, message: "Use `objectAt(index index: Int)` instead") public func objectAtIndex(index: Int)
```

When it comes to apps, patch releases are bug fixes, minor releases are small new features and major releases are re-designs or big features.

## Comments

 When they are needed, comments should be used to explain **why** a particular piece of code does something instead of **what**. Any comments that are used must be kept up-to-date or deleted. This does not apply to those comments used to generate documentation.

**Preferred:**

```swift
/*
  Workaround: So far this is the scroll-to-bottom method that worked best,
  with proper animation, and no issues so far, regardless of scrollview content size.
  If this looks and feels like a hack, it's because it kinda is.
  By creating a rect that's 1x1, pointed at the bottom-right side of the scrollview's content
  and telling it to scroll there, regardless of insets and offsets, it will scroll to the very bottom.
*/
func scrollToBottom() {
    let contentSize = self.collectionView.contentSize
    let bottomRect = CGRect(x: contentSize.width - 1, y: contentSize.height - 1, width: 1, height: 1)
    let visibleRect = self.collectionView.layer.visibleRect
    if !visibleRect.intersects(bottomRect) {
        self.collectionView.scrollRectToVisible(bottomRect, animated: true)
    }
}
```

**Not Preferred:**

```swift
// Scrolls to the bottom of the view
func scrollToBottom() {
    let contentSize = self.collectionView.contentSize
    let bottomRect = CGRect(x: contentSize.width - 1, y: contentSize.height - 1, width: 1, height: 1)
    let visibleRect = self.collectionView.layer.visibleRect
    if !visibleRect.intersects(bottomRect) {
        self.collectionView.scrollRectToVisible(bottomRect, animated: true)
    }
}
```

**Workaround comment:**

```swift
func override viewWillAppear(animated: Bool) {
    super.viewWillAppear(animated)

    /*
     Workaround: Selected cell only gets deselected when pressing the back button
     dragging the screen to go back doesn't deselect the selected cell.
     So, `self.clearsSelectionOnViewWillAppear = true` only works sometimes.
     */
    let selectedIndexPath = self.tableView.indexPathForSelectedRow()
    if selectedIndexPath {
        self.tableView.deselectRowAtIndexPath(selectedIndexPath, animated: true)
    }
}
```

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

## Blocks, delegates and data source

## Naming

Declaring delegates for some UIViewControllers, for example UITableViewControllers or UICollectionViewControllers, can be annoying, since they have their own delegates that use the variable `delegate`, and can't be overwritten.

Usually this leads to the developer naming the delegate things like `recipesDelegate`, `recipesControllerDelegate` and so on. This causes to confusion for the API user, since now there are two delegates. To avoid this, we do not subclass `UITableViewController`and `UICollectionViewController` directly. Instead we use our own classes here: `SweetTableController` and `SweetCollectionController` instead. This frees up the `delegate` variable for our own use. If you are using something else than UITableViewController and UICollectionViewController, use a meaningful name for your delegate, such as the name of the protocol or the class.

**Preferred:**

This is an example of using SweetTableController/SweetCollectionController, where we can use the `delegate` name.

```swift
protocol RecipesControllerDelegate: class {
}

class RecipesController: SweetTableController/SweetCollectionController {
    weak var delegate: RecipesControllerDelegate?
}
```

This is an example of subclassing `UINavigationController` where we can't use the `delegate` name, so make name the delegate based on the use case. For this specific scenario, I want my custom navigation controller to inform me that the navigation controller returned to the root controller.

```swift
protocol BackToRootRecipesNavigationControllerDelegate: class {
}

class RecipesNavigationController: UINavigationController {
    weak var backToRootDelegate: BackToRootRecipesNavigationControllerDelegate?
}
```

This is an example of a controller that will edit or create a recipe if it's a new recipe I want a different delegate than if it's an existing (edited) recipe. That's why I have `newRecipesDelegate` and `existingRecipesDelegate`.

```swift
protocol NewRecipesControllerDelegate: class {
}

protocol ExistingRecipesControllerDelegate: class {
}

class RecipesController: UIPresentationViewController {
    weak var newRecipesDelegate: NewRecipesControllerDelegate?
    weak var existingRecipesDelegate: ExistingRecipesControllerDelegate?
}
```

**Not Preferred:**

```swift
protocol RecipesNavigationControllerDelegate: class {
}

class RecipesNavigationController: UINavigationController {
    weak var recipesNavigationControllerDelegate: RecipesNavigationControllerDelegate?
}
```

```swift
protocol RecipesControllerDelegate: class {
}

class RecipesNavigationController: UITableViewController {
    weak var recipesControllerDelegate: RecipesControllerDelegate?
}
```

### Which one to use and when?

Choosing when to use a block or closure, a delegate or a dataSource most of the time is a simple decision, but if you're having trouble deciding here are some reminders on what does what.

### Block
- Asynchronous (For example: networking operations)
- User inputs with multiple options (For example: UIAlertView's YES and NO)
- Data source driven inputs (For example: A table items with action blocks that were defined in the data source)
- If there’s no tracked state or if the state is defined in the same method

### Delegate
- Synchronous (For example: buttons actions in views that should perform on their parents)
- Shouldn't return values
- Provides control over performing an action (For example: UITextField's shouldEndEditing)
- User input with one action (For example: buttons actions in views that should perform on their parents)
- If tracked state is shared (if state is stored in a property or a constant)

### Data source
- Returns a value

## View controllers

### Naming

When naming subclasses of `UIViewController` or friends `UIPageViewController`, `UICollectionViewController`, `UITableViewController`, you don't have to use the `ViewController` suffix.

For example instead of `BBRecipesTableViewController` you would do `RecipesController`.

*Why?*

Because naming anything else a `Controller` in general is a bad idea and should have another name instead.

### Presenting and dismissing View Controllers

- It's better practice to call `dismissViewControllerAnimated:completion:` in the `UIViewController` that did the presenting, not in the `UIViewController` that was presented.

[More information on this blog post.](https://sandofsky.com/blog/never-reach-up.html)

## Assets

### Images

Image names should be named consistently to preserve organization and developer sanity. They should be named in lower case and separated by dashes, followed by the un-prefixed name of the class or property they are customizing (if there is one), followed by a further description of color and/or placement, and finally their state.

**For example:**

- `refresh-bar-button-item` and `refresh-bar-button-item-selected`
- `article-navigation-bar-white` and `article-navigation-bar-black-selected`

Images should live in `Images.xcassets`, they don't need to be grouped in any way.

## Property Observing

### When using Property Observers make sure to not over-reload the UI

It's common that when using property observers any change will update some part of the UI.

For example:

```swift
// PhotoViewerController

var photo: Photo? {
    didSet {
        guard let viewerItem = self.viewerItem else { return }
        // Do something with photo, maybe download and so on
    }
}
```

The problem with this logic is that if for some reason the user of the `PhotoViewerController` is required to update multiple times his `PhotoViewerController` instance with the same photo, this will have awful performance issues. And since this user doesn't know the kind of logic processed inside this controller, bugs can occur. So it's better that if the `PhotoViewerController` is handling property observation, it should also handle the cases where the new introduced value is the same as the one that currently exists:

There are two ways to solve this issue:

### Version A:

One solution would be that the API user (the abuser) makes sure that they don't set a photo item if it has already been set.

```swift
// ViewerController (A horizontal array of PhotoViewerController)

let photoViewerController = self.cachedPhotoViewerControllers.objectForKey(photo.id)
if photoViewerController.photo?.id != photo.id {
    photoViewerController.photo = photo
}
```

### Version B (Recommended):

The other solution would be that the `PhotoViewerController` takes care of this since it's the one that decided to do property observing, instead of having a separate method to trigger this side effect (downloading photo).

```swift
// PhotoViewerController

var changed = false
var photo: Photo? {
    willSet {
        if self.photo?.id != newValue?.id {
            self.changed = true
        }
    }

    didSet {
        guard let photo = self.photo else { return }

        if self.changed {
            // Do something with photo, maybe download and so on
            self.changed = false
        }
    }
}
```

## Networking

Completion blocks in networking calls should be returned in the main thread.

Completion blocks should contain the `error` instead of success/failure blocks.

Use a simple NSURLSession wrapper to make things simpler, [Networking](https://github.com/3lvis/Networking) is a good candidate for this.

## View layout

Views should be layout using Apple's Auto Layout. No third-party frameworks are recommended at the moment but this is open for change. Just try to use the highest abstraction that's available to you, whether this is `UIStackView` or  `NSLayoutAnchor`.

Old style layout is still an option for when Auto Layout is not available.

Use a method called `addSubViewsAndConstraints()` where you first add the sub views and set your constraints. Call this method from `viewDidLoad()` in a ViewController or the init in a UIView. We do this to keep the layout code from cluttering your init methods. We move adding the sub views in this method because there inseparably related (a.k.a: your app chrashes when you try to set constraints on a view that has not been added to the sub view.) It also avoids conflicts when you use subclassing and want to override the method where you set the constraints. 

**Preferred:** 

```swift
func addSubviewsAndConstraints {
self.addSubview(self.label)

// add constraints to self.label
}
```

## Optional Force Unwrapping

When something that shouldn't return an optional, returns an optional you have two options:

**A)** Pass the error to the caller (throw): This is the ideal way of handling it, specially if there's the possibility of the app to recover. It's pretty straight forward, you create an `Error` or `NSError` with some information about the error, and throw it. [More information about throwing here.](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)

For example, lets say you have a method that returns a filename for a photo, in theory all the photos should have a filename, so, the return value shouldn't be an optional. Throw an error in this case and let the method caller decide if they either go for crashing the app, or for just excluding the photo with the missing filename.

**B)** Crash: is a bit more tricky, here you could use `fatalError()` to provide more information about the crash, or just force unwrap it.

In cases where the crash is obvious such as when dequeuing cells and casting them to your specific cell class, force unwrapping is preferred, because if you can't dequeue the cell, or can't a cell for the specific class, there's no other way to recover than to show an empty screen, which is not preferable.

## Lazy loading

When creating models with properties that require setup or configuration, it's better to use lazy loading instead of adding the logic to the viewDidLoad or init methods. This creates better code separation, it's more readable and avoids cluttering a single massive method that does more than it should. It's also easier to navigate the code, since `⌘+clicking` on a property will lead you right to where it's setup, and not just where it's declared.

**Preferred:** 

```swift
class RecipeCell: UITableViewCell {

lazy var label: UILabel = {
    let label = UILabel(....)
    label.textColor = .red  

    return label
}()

init() {
    self.contentView.addSubview(self.label)

    super.init()
}
```

**Not preferred:**

```swift
class RecipeCell: UITableViewCell {

init() {
    self.label = UILabel(....)
    self.label.textColor = .red  

    super.init()
}
```

# Swift Style Guide

Our overarching goals are conciseness, readability, and simplicity.

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

For functions and init methods, prefer named parameters for all arguments. Also avoid using underscores
in your method names.

Declarations:

```swift
class Guideline {
    func combineWithString(incoming incoming: String, options: Dictionary?)
    func upvoteBy(by amount: Int)
    func date(from string: String) -> NSDate
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

* Avoid doing method indentation since Swift's indentation is inconsistent, and just keep things in one line as long as is possible. It helps you by not letting you think about manually indenting your code.

**Preferred:**
```swift
let user = User(name: "Igor Ranieri", nickname: "Elland", country: "Germany")
```

**Not Preferred:**
```swift
let user = User(name: "Igor Ranieri",
      nickname: "elland",
      country: "Germany")
```

* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.
* Don't use parentheses.
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
        return "I am a circle at \(self.centerString()) with an area of \(self.computeArea())"
    }

    override func computeArea() -> Double {
        return M_PI * self.radius * self.radius
    }

    private func centerString() -> String {
        return "(\(self.x),\(self.y))"
    }
}
```

The example above demonstrates the following style guidelines:

 + The correct spacing for variable assignations is with a single space after and before the equals mark `=`, e.g. `x = 3`
 + Attributes in method signature have the `:` next to the name, e.g `init(x: Int, y: Int)` same with class inheritance and when using type inference
 + Indent getter and setter definitions and property observers
 + Don't add modifiers such as `internal` when they're already the default


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

For single-expression closures don't use implicit returns, always use return.

**Preferred:**

```swift
attendeeList.sort { a, b in
    return a > b
}
```

**Not Preferred:**
```swift
attendeeList.sort { a, b in
    a > b
}
```

Always used named parameters, and never unamed ordered parameters (`$0`). It's harder to write, harder to read and takes much longer to compile.

**Preferred:**
```swift
let values = [2.0, 4.0, 5.0, 7.0]
let squares = values.map { element in element * element}
// [4.0, 16.0, 25.0, 49.0]
```

**Not Preferred:**
```swift
let values = [2.0, 4.0, 5.0, 7.0]
let squares = values.map { $0 * $0 }
// [4.0, 16.0, 25.0, 49.0]
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
    class var callToActionNormal: UIColor {
        return UIColor(hex: "A87CF2")
    }

    class var callToActionSelected: UIColor {
        return UIColor(hex: "A87CF2")
    }

    class var callToActionHighlighted: UIColor {
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
