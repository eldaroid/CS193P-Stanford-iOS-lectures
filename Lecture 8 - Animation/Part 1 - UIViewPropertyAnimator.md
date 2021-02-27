# Lecture 8, Part 1: UIViewPropertyAnimator

We can animate changes to certain UI properties over time. The properties we can vary are (and this is the complete list):
* `frame` \ `center` - animate the position of the view.
* `bounds` (transient size, which does not conflict with animating center) - will animate the size of view although only in a transient way because it's the frame that determines where you are.

[`Различие Frame и Bounds в iOS.`](https://medium.com/@vladislav.mityuklyaev/%D1%80%D0%B0%D0%B7%D0%BB%D0%B8%D1%87%D0%B8%D0%B5-frame-%D0%B8-bounds-%D0%B2-ios-frame-vs-bounds-in-ios-4e5aee5ed477)

![alt text](https://github.com/eldaroid/pictures/blob/master/Swift/UIViewPropertyAnimator.png)

* `transform` (translation, rotation, scaling)  - We saw that with the card [where we rotated the corners upside down
](https://github.com/eldaroid/PlayingCard-App-in-iOS/wiki/3.-Make-cards#:~:text=lowerRightCornerLabel%20upside%20down)
* `alpha` (opacity)
* `backgroundColor`

We do this using a class called `UIViewPropertyAnimator`. To use it we pass it some animation parameters and a closure which contains the animation code. This code is executed immediately. You can also pass a second closure to be executed as soon as the animation is complete.

The `UIViewPropertyAnimator` class is extremely powerful, and we do not have the time to cover it in great depth. However, we will cover basic usage, which can be accomplished using the `runningPropertyAnimator` function:

class func the sam as static func, but with [one difference](https://stackoverflow.com/questions/25156377/what-is-the-difference-between-static-func-and-class-func-in-swift#:~:text=To%20be%20clearer)

```Swift
class func runningPropertyAnimator (
  withDuration: TimeInterval,
  delay: TimeInterval,
  options: UIViewAnimationOptions,
  animations: () -> Void,
  completion: ((position: UIViewAnimatingPosition) -> Void)? = nil
)
```

An example is shown below, which fades out a view over 3 seconds, beginning 2 seconds after it is called:

```Swift
if myView.alpha == 1.0 {
  UIViewPropertyAnimator.runningPropertyAnimator (
    withDuration: 3.0,
    delay: 2.0,
    options: [.allowUserInteraction],
    animations: { myView.alpha = 0.0 },
    completion: { if $0 == .end { myView.removeFromSuperview() } }
  )
  print("my alpha value is \(myView.alpha)")
}
```

It's important to note that while the change to transparency will not be visible to the user for 5 seconds, in the code, the alpha has immediately been set to 0. i.e. we will see the console output "my alpha value is 0.0" immediately. However, myView will remain a part of the superview until the animation is complete.

There are a wide range of available options you can set when animating UIView properties. The [ documentation for UIViewAnimationOptions](https://developer.apple.com/documentation/uikit/uiviewanimationoptions) lists them all.

[Previous Note](../Lecture%208%20-%20Animation/Part%200%20-%20Intro.md) | [Back To Contents](https://github.com/Firanus/stanford-iOS-lecture-notes) |  [Next Note](../Lecture%208%20-%20Animation/Part%202%20-%20UIView%20Transitions.md)
