# Bakken & Bæck Objective-C Style Guide

This style guide outlines the coding conventions of the iOS team at Bakken & Bæck.

## Introduction

Here are some of the documents from Apple that informed the style guide. If something isn't mentioned here, it's probably covered in great detail in one of these:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Table of Contents

* [Dot-Notation Syntax](#dot-notation-syntax)
* [Spacing](#spacing)
* [Conditionals](#conditionals)
  * [Ternary Operator](#ternary-operator)
* [Error handling](#error-handling)
* [Methods](#methods)
* [Variables](#variables)
* [Naming](#naming)
* [Init & Dealloc](#init-and-dealloc)
* [Literals](#literals)
* [CGRect Functions](#cgrect-functions)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Private Properties](#private-properties)
* [Booleans](#booleans)
* [Singletons](#singletons)

## Dot-Notation Syntax

Dot-notation should **always** be used for accessing properties. Bracket notation is preferred when using methods and in other instances.

**For example:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Spacing

* Indent using 4 spaces. Never indent with tabs. This should be configured on the project.

  ![Xcode indent settings](https://raw.githubusercontent.com/bakkenbaeck/iOS-playbook/master/assets/xcode-text-settings-objc.png) 

* `if`/`else`/`switch`/`while` and friends should always open on the same line as the statement, conditions like else or do that follow if or while respectively start in the same line where the previous condition ends.

**For example:**
```objc
if (user.isHappy) {
    // Do something
} else {
    // Do something else
}
```
* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
* `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.

## Conditionals

Conditional bodies should have braces, except when initializing and lazy loading.

#### Common code

**For example:**
```objc
if (!error) {
    Field *field = [self.collectionView fieldForIndexPath:field];
    return field.isValid;
}
```

**even if it's one line**

```objc
if (!error) {
    return success;
}
```

**Not:**
```objc
if (!error)
    return success;
```

**Initializer**

```objc
- (instancetype)initWithFrame:(CGRect)frame {
    self = [super initWithFrame:frame];
    if (self) {
        // Initialize
    }
    return self;
}
```

**Lazy loading**

```objc
- (UILabel *)label {
    if (!_label) {
        _label = [UILabel new];
    }
    return _label;
}
```

### Ternary Operator

The Ternary operator, ? , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an if statement, or refactored into instance variables.

**For example:**
```objc
result = (a > b) ? x : y;
```

**Not:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Error handling

When methods return an error parameter by reference, switch on the returned value, not the error variable.

**For example:**
```objc
NSError *error = nil;
BOOL shouldAcceptCalls = [self trySomethingWithError:&error];
if (!shouldAcceptCalls) {
    // Handle Error
}
```

**Not:**
```objc
NSError *error = nil;
[self trySomethingWithError:&error];
if (error) {
    // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).

## Methods

In method signatures, there should be a space after the scope (-/+ symbol). There should be a space between the method segments.

**For Example**:
```objc
- (void)updatePersonWithName:(NSString *)name
                    andImage:(UIImage *)image;
```

In the method implementation opening bracket should **always** be placed in the same line as the last parameter:

**For Example**:
```objc
- (void)updatePersonWithName:(NSString *)name
                    andImage:(UIImage *)image {
    // Implementation
}
```

In method invocations the parameters should be colon aligned, if the method becomes a cascade consider splitting the logic in several methods.

**For Example**:
```objc
[someObject updatePersonWithName:@"Example Text"
                           image:[UIImage imageNamed:@"Image.png"]
                  andDescription:@"Example description"];
```

## Variables

Variables should be named as descriptively as possible. Abbreviated variable names should be avoided.

Asterisks indicating pointers belong with the variable, e.g., `NSString *text` not `NSString* text` or `NSString * text`, except in the case of [constants](https://github.com/bakkenbaeck/iOS-playbook/blob/master/STYLE_GUIDELINES.md#constants).

Property definitions should be used in place of naked instance variables whenever possible. Direct instance variable access should be avoided except in initializer methods (`init`, `initWithCoder:`, etc…), `dealloc` methods and within custom setters and getters. For more information on using Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

When declaring a `strong` property, you can leave out the `strong` keyword as this is the default.

When declaring properties in public headers that contain mutable counterparts (`NSString`, `NSDictionary`, `NSArray`) make sure to include the `copy` keyword.

**For example:**

```objc
@interface BBSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**Not:**

```objc
@interface BBSection : NSObject {
    NSString *headline;
}
```

## Naming

Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Long, descriptive method and variable names are good.

**For example:**

```objc
UIButton *settingsButton;
```

**Not**

```objc
UIButton *setBut;
```

A three letter prefix (e.g. `BB`) should always be used for class names and constants, however may be omitted for Core Data entity names. Constants should be camel-case with all words capitalized and prefixed by the related class name for clarity.

**For example:**

```objc
static const NSTimeInterval BBArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Not:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Properties and local variables should be camel-case with the leading word being lowercase.

Instance variables should be camel-case with the leading word being lowercase, and should be prefixed with an underscore. This is consistent with instance variables synthesized automatically by LLVM. **If LLVM can synthesize the variable automatically, then let it.**

**For example:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Not:**

```objc
id varnm;
```

Prefer `remoteID` or `localID` when naming local or remote foreign keys.

**For example:**

```objc
Note *note;
note.localID = 12; // local
note.remoteID = 10; // from backend
```

**Not:**

```objc```
Note *note;
note.noteID = 1;
```

**or**

```objc```
Note *note;
note.noteId = 1;
```

## Pragma marks

Use pragma marks to structure your code. Sort them in a linear fashion, starting with the initialization, lazy loaded properties and other getters followed by the setters.

Private and custom delegate methods are added at the bottom.

Methods that overwrite their parent methods should be grouped in `#pragma mark - PARENT_CLASS`

```objc
#pragma mark - Initializers

#pragma mark - Getters

#pragma mark - Setters

#pragma mark - View life cycle

#pragma mark - Actions

#pragma mark - Notifications

#pragma mark - BBBaseTableViewController

#pragma mark - UITableViewDelegate

#pragma mark - CustomViewDelegate

#pragma mark - Private methods
```

## init and dealloc

`dealloc` methods should be placed at the top of the implementation, directly after the `@synthesize` and `@dynamic` statements. `init` should be placed directly below the `dealloc` methods of any class.

`init` methods should be structured like this:

```objc
- (instancetype)init {
    self = [super init]; // or call the designated initalizer
    if (!self) return nil;

    // Custom initialization

    return self;
}
```

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

**For example:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Not:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

Literals should be used when accessing `NSDictionary` or `NSArray` instances.

```objc
NSString *name = names[0];
NSString *productManager = productManagers[@"iPhone"];
```

**Not:**

```objc
NSString *name = [names objectAtIndex:0];
NSString *productManager = [productManagers objectForKey:@"iPhone"];
```

## CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**For example:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Not:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants should be declared as `static` constants and not `#define`s unless explicitly being used as a macro.

They should be located in the class that uses them, if they are shared between classes you can have them in the public header.

**For example:**

```objc
static NSString * const BBAboutViewControllerCompanyName = @"Bakken & Bæck";

static const CGFloat BBImageThumbnailHeight = 50.0f;

static const CGSize BBImageDefaultSize = {40.0f, 40.0f};
```

**Not:**

```objc
#define CompanyName @"Bakken & Bæck"

#define thumbnailHeight 2
```

## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types — `NS_ENUM()`

**Example:**

```objc
typedef NS_ENUM(NSInteger, BBAdRequestState) {
    BBAdRequestStateInactive,
    BBAdRequestStateLoading
};
```

## Private Properties

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories (such as `BBPrivate` or `private`) should never be used unless extending another class.

**For example:**

```objc
@interface BBAdvertisement ()

@property (nonatomic) GADBannerView *googleAdView;
@property (nonatomic) ADBannerView *iAdView;
@property (nonatomic) UIWebView *adXWebView;

@end
```

## Booleans

Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

**For example:**

```objc
if (!someObject) {
}
```

**Not:**

```objc
if (someObject == nil) {
}
```

-----

**For a `BOOL`, here are two examples:**

```objc
if (isAwesome)
if (![someObject boolValue])
```

**Not:**

```objc
if ([someObject boolValue] == NO)
if (isAwesome == YES) // Never do this.
```

-----

If the name of a `BOOL` property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor, for example:

```objc
@property (assign, getter=isEditable) BOOL editable;
```
Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.
```objc
+ (instancetype)sharedInstance {
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```
This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Attribution

This document is based on the [NYTimes Objective-C Style Guide](https://github.com/NYTimes/objective-c-style-guide).
