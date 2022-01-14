---
layout: post
title: Coordinator pattern with Clean Swift
author: Milan de Ruiter
tags: ["Architecture", "Coordinator", "Delegate", "Clean Swift", "Swift"]
---


The coordinator pattern is a very common pattern in the iOS industry. As with a lot of architectures, this one is invented to separate responsibilities. This coordinator is responsible for what the name already does suspect, coordinating. 

Considering that we are using the Clean Swift architecture we already have a layer that is responsible for the navigation - router. 

In comparison with the router, the coordinator has a few more benefits. But first let’s see how a coordinator can be implemented.

all the coordinators conforms to a protocol: 

```swift
protocol Coordinator {
  func start()
}
```

This protocol tells that each coordinator should have the start method. The dependencies that are needed to start this flow are injected via the constructor.

Let’s say we have an coordinator that starts when the application is loaded, `applicationCoordinator`.

```swift
final class ApplicationCoordinator: Coordinator {
  
  private let window: UIWindow

  init(window: UIWindow) {
    self.window = window
  }

  // MARK: - Coordinator

  func start() {
    let dashboardViewController = DashboardBuilder().build()
    window.rootViewController = dashboardViewController
    window.makeKeyAndVisible()
  }
}
```

Above we see an example of a coordinator. This coordinator received a window via the constructor that this coordinator uses to presents the dashboard on. In other coordinators we are not using the window but navigationControllers or the presenting viewController.

At this point we have a coordinator that is showing one single view, but how do we know when we have to navigate? This will be clear once I have explained how we communicate with the coordinator. 

Instead of a coordinator protocol that is telling its child’s what entry points we have, we do it the other way around. The ViewController that we are showing tells the coordinator what happened in the viewController.

To make this communication possible, each viewController will get it’s very own delegate. For the dashboardViewController it looks like this:

```swift
protocol DashboardViewControllerDelegate: AnyObject {
  func dashboard(_ viewController: UIViewController, didSelect item: DashboardItem)
}

final class DashboardViewController: UIViewController {

  weak var delegate: DashboardViewControllerDelegate?

  func display(selected item: DashboardItem) {
    delegate?.dashboard(self, didSelect: item)
  }
}
```

And for the coordinator we have to conform to this delegate.

```swift
final class ApplicationCoordinator: Coordinator {
  
  private let window: UIWindow
  private var coordinator: Coordinator?

  init(window: UIWindow) {
    self.window = window
  }

  // MARK: - Coordinator

  func start() {
    let dashboardViewController = DashboardBuilder(delegate: self).build()
    window.rootViewController = dashboardViewController
    window.makeKeyAndVisible()
  }
}

// MARK: - DashboardViewControllerDelegate
extension ApplicationCoordinator: DashboardViewControllerDelegate {

  func dashboard(_ viewController: UIViewController, didSelect item: DashboardItem) {
    let builder = DetailsCoordinatorBuilder(
      presentingViewController: viewController,
      item: item,
      delegate: self
    )
      
    coordinator = builder.build()
    coordinator?.start()
  }
}

// MARK: - DetailsCoordinatorDelegate
extension ApplicationCoordinator: DetailsCoordinatorDelegate {
  // ...
}
```

With using this delegation pattern the viewController is no longer telling what the router needs to do, but it tells what happened. Everyone can be the delegate and act on those events.

Also, you see that we are not starting the details viewController directly, but we are starting a new coordinator. By calling start on this coordinator we let the coordinator know that it can show its viewController.

Another benefit of coordinators is that it can work without having a VIP stack. This sounds weird but maybe the following example shows you how this can be useful.

Let’s use the same scenario as before when the application starts we have a applicationCoordinator. This coordinator is responsible for starting the flow, however we  can have two flows. User is already logged in or the used it not logged in. In this scenario the coordinator can decide based on the provided information which flow it should start. 

Implementing this behavior will look like:

```swift
final class ApplicationCoordinator: Coordinator {
  
  private let authService: AuthService
  private let window: UIWindow
  private var coordinator: Coordinator?

  init(window: UIWindow, authService: AuthService) {
    self.window = window
    self.authService = authService
  }

  // MARK: - Coordinator

  func start() {
    if authService.status == .authenticated {
      let builder = DashboardCoordinatorBuilder(window: window, delegate: self)
      coordinator = builder.build()
    } else {
      let builder = LoginCoordinatorBuilder(window: window, delegate: self)
      coordinator = builder.build()
    }

    coordinator?.start()
  }
}

// MARK: - DashboardCoordinatorDelegate
extension ApplicationCoordinator: DashboardCoordinatorDelegate {
  // ...
}

// MARK: - LoginCoordinatorDelegate
extension ApplicationCoordinator: LoginCoordinatorDelegate {
  // ...
}
```

As you can see, no single viewController is shown via this coordinator. Using those kind of coordinators can help you separate features and start flows without knowing who started the flow.
