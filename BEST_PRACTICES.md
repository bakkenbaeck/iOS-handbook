# Best practices

## Table of Contents

* [Xcode](#xcode)
* [Deployment](#deployment)
* [Comments](#comments)
* [Blocks, delegates or data source](#blocks-delegates-or-data-source)
* [View controllers](#view-controllers)
* [Assets](#assets)
* [Property Observing](#property-observing)
* [Networking](#networking)
* [Theming](#theming)

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
- If there’s no tracked state or if the state is defined in the same method

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

let photoViewerController = self.cachedPhotoViewerControllers.objectForKey(photo.remoteID)
if photoViewerController.photo?.remoteID != photo.remoteID {
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
        if self.photo?.remoteID != newValue?.remoteID {
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

## Theming

Use a `Theme.swift` that contains extensions for theming your app. When adding color extensions avoid adding the `Color` postfix to your color methods.

For example:

```swift
import Hex
import UIKit

extension UIColor {
    class func brightPurple() -> UIColor {
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
