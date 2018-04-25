# Best Practices

These are things we've found to be helpful in creating readable, maintainable code rather than hard requirements. Hard requirements live in the [Style Guide](STYLE_GUIDE.md)

## Table of Contents

* [Xcode](#xcode)
    * [Version](#version)
    * [Project Structure](#project-structure)
* [Versioning](#versioning)
* [Class structure](#class-structure)
* [Comments](#comments)
* [Extension Method Naming](#extension-method-naming)
* [Blocks/Closures vs. Delegates](#blocks-closures-vs-delegates)
* [View controllers](#view-controllers)
* [Property Observing](#property-observing)
* [Networking](#networking)

## Xcode

### Version

The recommended version of Xcode (and Swift) for all purposes is the current available [in the App Store](https://itunes.apple.com/no/app/xcode/id497799835?mt=12).

Make sure to coordinate with your project team when upgrading to a new version, particularly if a new version of Swift is involved.

### Project Structure

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped by type and feature for greater clarity.

Make sure to use an [app-template](https://github.com/bakkenbaeck/app-template) for consistency.

### Warnings

Enable warnings by adding `-Weverything` to your Build Settings under "Other Compiler Flags". If you need to ignore a specific warning you can use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas) or add `-Wno-warning-to-be-disabled` (for example `-Wno-gnu-conditional-omitted-operand`).

## Versioning

To version our open source projects we use [semantic versioning](http://semver.org/), and it's important that minor releases are backwards compatible otherwise don't feel shy to make it a major release.

When making backwards compatible changes, flag your old APIs as deprecated like this:

```swift
@available(*, deprecated: 4.3.0, message: "Use `objectAt(index index: Int)` instead") public func objectAtIndex(index: Int)
```

When it comes to apps, patch releases are bug fixes, minor releases are small new features and major releases are re-designs or big features.

## Class structure

If you are adding a class and several extensions to that class in one file, reading the class becomes simpler if the extensions are in the file below the class. 

Generally, a class file should look like this: 

```
class {
    vars
    init
    funcs
}

extensions
```

## Comments

 When they are needed, comments should be used to explain **why** a particular piece of code does something instead of **what**. Any comments must be kept up-to-date or deleted. 
 
**Preferred:**

```swift
func scrollToBottom() {
	/*
	  Workaround: So far this is the scroll-to-bottom method that worked best,
	  with proper animation, and no issues so far, regardless of scrollview content size.
	  If this looks and feels like a hack, it's because it kinda is.
	  By creating a rect that's 1x1, pointed at the bottom-right side of the scrollview's content
	  and telling it to scroll there, regardless of insets and offsets, it will scroll to the very bottom.
	*/
    let contentSize = collectionView.contentSize
    let bottomRect = CGRect(x: contentSize.width - 1, y: contentSize.height - 1, width: 1, height: 1)
    let visibleRect = collectionView.layer.visibleRect
    if !visibleRect.intersects(bottomRect) {
        collectionView.scrollRectToVisible(bottomRect, animated: true)
    }
}
```

**Not Preferred:**

```swift
// Scrolls to the bottom of the view
func scrollToBottom() {
    let contentSize = collectionView.contentSize
    let bottomRect = CGRect(x: contentSize.width - 1, y: contentSize.height - 1, width: 1, height: 1)
    let visibleRect = collectionView.layer.visibleRect
    if !visibleRect.intersects(bottomRect) {
        collectionView.scrollRectToVisible(bottomRect, animated: true)
    }
}
```

## Extension Method Naming

Watch out for potential method name collisions with Swift extensions of Objective-C classes. [Peter Steinberger goes into the maddening details of why here](https://pspdfkit.com/blog/2016/surprises-with-swift-extensions/), but when adding an extension to an Objective-C class, it can be helpful to add an `@objc` wrapper with a prefix for the compiler. 

**Preferred:**

```swift
extension UIViewController {
    
    @objc(myapp_foo)
    func foo() {
        // Do something view controllery
    }
}
```

**Not preferred**:

```swift
extension UIViewController {
    
    func foo() {
        // Do something view controllery
        // At least until Apple implements their own foo()
    }
}
```

## Blocks/Closures vs Delegates

Choosing when to use a block/closure vs. a delegate method is usually is a simple decision, but if you're having trouble deciding here are some reminders on what does what.

**When To Use Blocks/Closures:**

- Asynchronous (For example: networking operations)
- User inputs with multiple options (For example: `UIAlertController`'s assorted buttons)
- Data source driven inputs (For example: A table items with action blocks that were defined in the data source)
- If there’s no tracked state, or if the state is defined in the same method

**When To Use Delegates**

- Synchronous (For example: buttons actions in views that should perform on their parents)
- Shouldn't return values
- Provides control over performing an action (For example: UITextField's shouldEndEditing)
- User input with one action (For example: buttons actions in views that should perform on their parents)
- If tracked state is shared (if state is stored in a property or a constant)

## View controllers

### Presenting and dismissing View Controllers

- It's better practice to call `dismissViewControllerAnimated:completion:` in the `UIViewController` that did the presenting, not in the `UIViewController` that was presented.

[More information on this blog post.](https://sandofsky.com/blog/never-reach-up.html)

## Property Observing

When using Property Observers make sure to not over-reload the UI. It's common that when using property observers any change will update some part of the UI.

For example:

```swift
// PhotoViewerController
var photo: Photo? {
    didSet {
        guard let viewerItem = viewerItem else { return }
        // Do something with photo, maybe download and so on
    }
}
```

The problem with this logic is that if for some reason the caller repeatedly sets the same photo, this can have awful performance issues. 

If the `PhotoViewerController` is handling property observation, it should also handle the cases where the new introduced value is the same as the one that currently exists.

There are two ways to solve this issue:

### Version A:

One solution would be that the API user (the abuser) makes sure that they don't set a photo item if it has already been set.

```swift
// ViewerController (A horizontal array of PhotoViewerController)
let photoViewerController = cachedPhotoViewerControllers.objectForKey(photo.id)
if photoViewerController.photo?.id != photo.id {
    photoViewerController.photo = photo
}
```

### Version B (Recommended):

The other solution would be that the `PhotoViewerController` takes care of this since it's the one that decided to do property observing, instead of having a separate method to trigger this side effect (downloading photo).

```swift
// PhotoViewerController
var photo: Photo? {
    didSet {
        guard let photo = photo else { return }

        if photo.id != oldValue?.id  {
            // Do something with photo, maybe download and so on
        } //else, the photo has remained the same.
    }
}
```

## Networking

Completion blocks in networking calls should be returned in the main thread. This helps avoid needing to deal with threading outside of the networking stack. 

Completion blocks should contain the `error` instead of success/failure blocks.

Use a simple NSURLSession wrapper to make things simpler. [Teapot](https://github.com/bakkenbaeck/Teapot) is a good candidate for this.

