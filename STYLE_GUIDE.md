# Swift style guide

Our overarching goals are conciseness, readability, simplicity, and making as much of this style guide robot-enforcable as possible.

Code is read far more often than it's written. Clarity at the point of reading is always better than saving a few keystrokes.

The items in this style guide are required. For more general good ideas and practical advice, please consult our [Best Practices](BEST_PRACTICES.md).

* [Linting](#linting)
* [Naming](#naming)
    * [Class prefixes](#class-prefixes)
* [Spacing and indentation](#spacing-and-indentation)
* [Protocol conformance](#protocol-conformance)
* [View layout](#view-layout)
* [Lazy loading](#lazy-loading)
* [Computed properties](#computed-properties)
* [Function declarations](#function-declarations)
* [Closure expressions](#closure-expressions)
* [Types](#types)
* [Optionals and force-unwrapping](#optionals-and-force-unwrapping)
* [Struct initializers](#struct-initializers)
* [Type inference](#type-inference)
* [Syntactic sugar](#syntactic-sugar)
* [Semicolons](#semicolons)
* [Resources](#resources)
    * [Image assets](#image-assets)
    * [Localized strings](#localized-strings)
* [Commented code](#commented-code)


## Linting

For code lint we use [SwiftLint](https://github.com/realm/SwiftLint). Our current `.swiftlint.yml` file is available [here](default.swiftlint.yml).

Most rules enabled there are for formatting purposes, but there are a few which warn about common issues like forgetting to call `super` in methods, or simple performance tweaks like using `.first(where:)` instead of `.filter { }.first { }`. 

Do not merge PRs which have SwiftLint warnings. 

To automate this process at PR time, you can use [Danger](http://danger.systems/ruby/) and the `Danger-Swiftlint` plugin in [Swift](https://github.com/ashfurrow/danger-swiftlint) or in [Ruby](https://github.com/ashfurrow/danger-ruby-swiftlint) to have a bot that automatically comments on code with violations. Instructions for setting up Danger and its associated bots are [here](http://danger.systems/guides/getting_started.html).

## Naming

Use descriptive names with `CamelCase` for classes, methods, variables, etc. 

Class names should be capitalized. Method names, variables and constants should start with a lower case letter.

Avoid underscores except where the name of something is used to access other things (such as generated code or `CodableKeys` enums).

When in doubt, look at how Xcode lists the method in the jump bar – our style here matches that.

![Methods in Xcode jump bar](https://raw.githubusercontent.com/raywenderlich/swift-style-guide/master/screens/xcode-jump-bar.png)

### Class prefixes

Do not add prefixes to your Swift types. They are not necessary due to module-level namespacing. 

## Spacing and indentation

* Indent using 4 spaces rather than tabs. This should be configured on the project.

  ![Xcode indent settings](https://raw.githubusercontent.com/bakkenbaeck/iOS-playbook/master/assets/xcode-text-settings-swift.png)

* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.
* There should be exactly one blank line between methods to aid in visual clarity and organization. 

## Protocol conformance

When adding protocol conformance to a class, prefer adding a separate class extension for the protocol methods rather than adding the protocol conformances to the primary class wherever possible. 

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

## Lazy loading

When creating models with properties that require setup or configuration, it's better to use lazy loading instead of adding the logic to the `viewDidLoad` or `init` methods. 

**Preferred:**

```swift
class RecipeCell: UITableViewCell {
    private lazy var label: UILabel = {
        let label = UILabel()
        label.textColor = .red

        return label
    }()

    init() {
        super.init()
        contentView.addSubview(label)
    }
...
```

**Not preferred:**

```swift
class RecipeCell: UITableViewCell {
    private let label: UILabel

    init() {
        label = UILabel()
        label.textColor = .red

        super.init()
        contentView.addSubview(label)
    }
...
```

## View layout

Views should be laid out using Auto Layout. If setting up views programmatically, we recommend using [TinyConstraints](https://github.com/roberthein/TinyConstraints) to make your Auto Layout code easier to both write and read.

If you don't want to or can't use a library, remember to use the highest layer of abstraction available to you, such as `NSLayoutAnchor` or `UIStackView`, in order to reduce boilerplate.

Use either single (e.g., `addSubviewsAndConstraints()`) or clearly named multiple (e.g., `setupTextFields()`, `setupButtons()`) layout methods to keep your `viewDidLoad` or `init` methods from being painfully long. 

**Preferred:**

```swift
override init(frame: CGRect) {
    super.init(frame: frame)
    // do general setup things
    addSubviewsAndConstraints()
}

func addSubviewsAndConstraints() {
    addSubview(label)
    NSLayoutConstraint.activate([
        label.topAnchor.constraint(equalTo: self.topAnchor),
...
```

**Not preferred:**

```swift
override init(frame: CGRect) {
    super.init(frame: frame)
    // do general setup things
    addSubview(label)
    NSLayoutConstraint.activate([
        label.topAnchor.constraint(equalTo: self.topAnchor),
...
```

## Computed properties

If a computed property is read-only, omit the `get` clause. The `get` clause is required only when a `set` clause is provided.

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

## Closure expressions

Use trailing closure syntax wherever possible. Keep the `[weak self] parameterOne, parameterTwo in` on the same line as the opening brace of the closure. 

Give incoming closure parameters descriptive names:

```swift
return SKAction.customActionWithDuration(effect.duration) { node, elapsedTime in
    // more code goes here
}
```

Using indexed parameters is permissible, but try to limit it to one-liners, and have clearly named variables with the result of the operation: 

**Preferred:**

```swift
let adminUsers = users.filter { $0.isAdmin }
let adminUsernames = adminUsers.map { $0.username }
let sortedUsernames = adminUsernames.sort { $0 < $1 }
```

**Not Preferred**

```swift
users.filter { $0.isAdmin }
    .map { $0.username }
    .sort { $0 < $1 } 
```

Be careful getting too happy with `$0`-style parameter usage - make sure that other people (including Future You) can understand exactly what that variable represents when they read your code.

## Types

Always use Swift's native types when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.

**Preferred:**

```swift
let width = 120.0 // Double
let widthString = (width as NSNumber).stringValue // String
```

**Not Preferred:**

```swift
let width: NSNumber = 120.0 // NSNumber
let widthString: NSString = width.stringValue // NSString
```

## Optionals and force-unwrapping

Declare variables and function return types as optional with `?` where a nil value is acceptable.

Avoid Implicitly Unwrapped Optionals (i.e., variables declared with an `!`) unless using `IBOutlets` that should never be `nil` after the nib or storyboard has loaded, or unless you have a very clear reason to do so you can justify when disabling the IUO warning in SwiftLint. 

There's a reason `!` has the informal nickname of "the crash operator": If what you think is going to be there is not there, your entire application will crash.

### Struct initializers

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

## Type inference

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

## Syntactic sugar

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

Do not use semicolons at the end of a line - they are no longer required. 

Do not write multiple statements on a single line separated with semicolons.

## Resources

Use extensions or wrapper classes for accessing elements of asset catalogs, custom colors and fonts. It helps to avoid the error-prone practice of hard-coding strings into your code.

Using generated code for this wherever possible is even better, since if an element you were previously accessing was removed, when the code is regenerated, the build will fail at compile time instead of runtime. 

Use a `Theme.swift` that contains extensions for the following classes which will assist in theming your app: 

- `UIColor` wrappers
- `CGFloat` sizes for fonts and margins
- `UIFont` wrappers
 
Work with the designer on your project to come up with a common naming scheme for colors to make picking colors, fonts, and margins out of a design file easier. 

### Image assets

Images should live in `Images.xcassets`. They don't need to be grouped in any way unless you find that helpful for organization.

Images should be named consistently within an app to preserve organization and developer sanity. It is strongly suggested to append the state name to images for non-default states.

Images should be named in either `camelCase` or  `lower_snake_case` to facilitate code generation based on the image names. Pick one and stick to it per project. 

**Examples:**

- `ic_refresh` and `ic_refresh_selected` for a refresh icon and its selected state used in multiple places throughout an app
- `articleNavigationBar` and `articleNavigationBarSelected` for  something used in one specific place. 

### Localized strings

Any strings visible to the user must be localized using `NSLocalizedString` or something wrapping it. It is way, way easier to set up all strings like this from the beginning than to add this feature later. 

Don't use the developer-language content of the string as the localized string's key, because if you do, you then have to update the copy in multiple places. 

Use `lower_snake_case` for localized string keys to both facilitate code generation and to make it extremely obvious when there is an untranslated user-facing string, since by default if a value does not exist for a key in `Localizable.strings`, the key itself is returned.

```swift
// ProfileViewController.swift
titleLabel.text = NSLocalizedString("profile_title", "Title of the profile page")

// Localizable.strings
/* Title of the profile page */
"profile_title" = "Your Profile";
```

**Not preferred:**

```swift
// ProfileViewController.swift
titleLabel.text = NSLocalizedString("Your Profile", "Your Profile")

// Localizable.strings
/* Your Profile */
"Your Profile" = "Your Profile";
```

Some guidelines: 

* Remember that the description is used to tell translators what the purpose of a key is, not what it contains in the developer's language. 
* Using a wrapper class (especially a code-generated one) allows easy reuse of copy both in multiple places in the app, as well as in UI tests.
* If you are including support for Accessibility, ensure all strings which will be read by VoiceOver or assistive devices are also localized. 

## Commented code

Do not merge commented out code. Any code which is commented out when a PR is otherwise ready to be merged should be deleted. We can always go back and find the deleted code with version control if needed.


