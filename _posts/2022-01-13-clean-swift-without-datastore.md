---
layout: post
title: Clean Swift without the dataStore
author: Milan de Ruiter
tags: ["Architecture", "Clean Swift", "Swift"]
---

[Clean Swift](https://clean-swift.com) (also known as VIP) is a popular architecture which is focussed on splitting the responsibilities over multiple components. In comparison to the well known [MVVM architecture](https://en.wikipedia.org/wiki/Model–view–viewmodel), VIP has split the viewModel even more.

In Clean Swift we have the following components:

- **ViewController**: Responsible for displaying the view the the user and passing all interactions to the interactor.

- **Interactor**: Responsible for handling all user input and business logic.

- **Presenter**: Responsible for transforming data objects into viewModels which can be passed to the ViewController.

- **Router**: Responsible for the navigation between VIP stacks.

The whole stack as described above is unidirectional. The `ViewController` communicates to the `interactor`, but the `interactor` is never ever communicating back to the `viewController`. The same counts for the `presenter` and the `interactor`.

**The DataStore In Clean Swift**

In Clean Swift the navigation is done via the `router`. To easily get and set the requirements for the next screen, the creators of Clean Swift introduced a `dataStore` protocol. The `router` is aware of this protocol and can use it to get and set properties.

As the whole flow is unidirectional, it feels wrong to have a dataStore protocol which allow us to have a shortcut from `viewController` to `router` and `interactor`.

Another side effect that this dataStore protocol is causing is that we have to deal with optionals. We are not sure when a property is set, since we are working with property injection rather than constructor injection. 

So with using a dataStore protocol we are:

1. Breaking encapsulation; We have to expose properties which can therefore not longer be private.

2. Breaking unidirectional; The router gets and sets values in the dataStore (interactor) and is communicating directly which is in theory against it own guidelines

3. Dealing with optional; The dependency injection in Clean Swift as described above is based on property injection rather than constructor injection. This way we end up with more optionals than needed.

### How To Get Rid Of The DataStore?

Before I show you a way to get rid of this dataStore protocol, I want to describe a situation.

Let's say we are building an application that has two screens. The first screen is a overview screen that is a list of items. The second screen is a detail screen. 

If we look at above example the router that belongs to the first screen is passing the selected item to the second screen. The dataStore will look like:

```swift
protocol SecondScreenDataStore {
    var selectedItem: Item? { get set }
}
```

The selectedItem in the dataStore is an optional as it is set via property injection, however this item is crucial in this screen so it shouldn't be optional.

To fix this issue we can remove the optional from the protocol pass the selectedItem via constructor injection, like:

```swift
protocol SecondScreenDataStore {
    var selectedItem: Item { get set }
}

final class SecondScreenInteractor: SecondScreenDataStore {
  var selectedItem: Item

  init(item: Item) {
    self.selectedItem = item
  }
}
```

In above example we no longer have to deal with the optional, so that issue is solved. 

The other issue we have with the dataStore is that we are breaking encapsulation. The reason for this is only to get and set items. But since we are now setting this via the constructor, we no longer need to know about this item, right?

The only things that is holding us back from removing the dataStore is that the router is also using the dataStore to get properties from the current screen. To cut this dependency we need to make a small change, which also helps us fix the unidirectional issue.

To fix the issue where the router is using the dataStore to get properties, we now have to pass them to the router, via the VIP stack:

```swift
// ViewController.swift

func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
  // Notify interactor that an item is selected
  interactor.didSelectItem(at: indexPath)
}
```

```swift
// Interactor.swift

func didSelectItem(at indexPath: IndexPath) {
    // Handle the tap, and pass the selected item to the presenter
    let selectedItem = items[indexPath.row]
    presenter?.present(selected: selectedItem)
}
```

```swift
// Presenter.swift

func present(selected item: Item) {
  // Pass the item to the viewController
  viewController?.display(selected: item)
}
```


```swift
// ViewController.swift

func display(selected item: Item) {
  // Pass the item to the router
  router?.routeToSelected(item)
}
```

```swift
// Router.swift

func routeToSelected(_ item: Item) {
  // Instantiate the next viewController with the passed dependencies
  let secondScreenViewController = SecondScreenViewController.build(item: item)
  viewController?.navigationController?.push(secondScreenViewController, animated: true)
}
```

Looking at above example you can see that we can pass dependencies to our next VIP stack without breaking the rules what VIP is based on. There is no need to break encapsulation and the unidirectional flow if we just pass the dependencies via a regular VIP cycle and remove the DataStore protocol!