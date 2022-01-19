---
layout: post
title: Custom Xcode templates
author: Milan de Ruiter
tags: ["Tools", "Xcode Templates", "Tips & Tricks"]
---

An easy way to automate the boring repetitive task to implement for example full VIP cycles is to make use of Xcode templates.

By default Xcode already comes with a lot of templates. For example the template for a Swift file, which you might now use to create a new Swift file is also an Xcode template. Just like we have an template for unit tests.

Both templates are helpful in your daily development. Let’s take the xctest template as an example, as this one adds more code than the simple Swift one. If we look to the template we see that it will create a file like:

```swift
import XCTest

class SomeAwesomeFeatureTests: XCTestCase {

    override func setUpWithError() throws {
        // Put setup code here. This method is called before the invocation of each test method in the class.
    }

    override func tearDownWithError() throws {
        // Put teardown code here. This method is called after the invocation of each test method in the class.
    }

    func testExample() throws {
        // This is an example of a functional test case.
        // Use XCTAssert and related functions to verify your tests produce the correct results.
        // Any test you write for XCTest can be annotated as throws and async.
        // Mark your test throws to produce an unexpected failure when your test encounters an uncaught error.
        // Mark your test async to allow awaiting for asynchronous code to complete. Check the results with assertions afterwards.
    }

    func testPerformanceExample() throws {
        // This is an example of a performance test case.
        self.measure {
            // Put the code you want to measure the time of here.
        }
    }

}

```

By just picking this template you are importing XCTest, Conforming your class to XCTestCase and it even adds some methods to start right away with testing your implementation.

The templates help you to get rid of the boring repetitive tasks (e.g. confirming all test cases to XCTestCase). Another benefit of using templates is that it helps you with aligning the code. If everyone in your team uses the templates, the code will automatically be a bit more aligned. 


# How to make custom Templates?

Now that we know what Templates can do for us, the question is very simple: How can we make our own Xcode templates? 

To be honest, it is really easy to create a custom template. But… before we go and create a template we first need to decide what kind of template we want to make. Yes, there are multiple types of templates.

**File template**:
As the name already does suggest, file templates are templates for one single file. An example of such template is the XCTest template mentioned earlier. 

**Project template**: The project template is a template that creates multiple files. This is useful when implementing the base of an feature. In the MVVM architecture each feature comes with a viewController but also with a viewModel. With a project template you can have those both automatically created for you!

My personal preference when I am working with project templates is to also have the same templates as individual file templates. So when for example I only want a viewModel, I can create that file with a file template. 

What I normally do when I need to create a new template is making a copy of an existing template. You can either use mine (see below) or the one provided by Apple. Apple's default Xcode templates are stored inside this directory: 
```bash
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode
```

In this post I will show you how to make a project template for the VIP (Clean Swift) architecture. This architecture comes with several layers and therefore Xcode templates brings a lot of benefits.

Let’s start making our own templates! The end goal is to have the folder structure just like below.

```xml
ProjectName
    └──VIP.xctemplate
        ├── TemplateIcon.png
        ├── TemplateInfo.plist
        ├── ___FILEBASENAME___Interactor.swift
        ├── ___FILEBASENAME___Presenter.swift
        └── ___FILEBASENAME___ViewController.swift
```

_You can group templates by putting the templates in directories. This is convenient when you are using multiple templates for different projects. In this example we use ProjectName as our project name._

### VIP.xctemplate
A folder directory with the `.xctemplate` file type. 

### TemplateIcon.png
Image in `.png` format that is shown in the Xcode ‘New File’ window. 

### TemplateInfo.plist
This property list file describes how the template is configured and how the wizard which is shown in Xcode can provide the information that is needed. 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Kind</key>
    <string>Xcode.IDEKit.TextSubstitutionFileTemplateKind</string>
    <key>Description</key>
    <string>Template for a full VIP stack. Including a viewController, interactor and presenter</string>
    <key>Platforms</key>
    <array>
        <string>com.apple.platform.iphoneos</string>
    </array>
    <key>Options</key>
    <array>
        <dict>
            <key>Identifier</key>
            <string>productName</string>
            <key>Required</key>
            <true/>
            <key>Name</key>
            <string>Module name:</string>
            <key>Description</key>
            <string>The name of module</string>
            <key>Type</key>
            <string>text</string>
        </dict>
    </array>
</dict>
</plist>
```

Above you can see the full property list we are using in the example. 

**Kind**: The type of template we are going to create. In this example we are setting it to `Xcode.IDEKit.TextSubstitutionFileTemplateKind`. we are saying that we are creating a file template. Another option is `Xcode.IDECoreDataModeler.ManagedObjectTemplateKind`.

**Description**: The description of what this template is doing. In older Xcode versions this was shown, but I haven’t seen the description coming back for a long time. Now I only use it to tell myself (and my colleagues) what this template is doing.

**Platforms**: Platforms is expecting an array with all the supported platforms. In this example we are only creating it for iOS so we add `com.apple.platform.iphoneos`. Other options are:
`com.apple.platform.macos`, `com.apple.platform.watchos`and `com.apple.platform.tvos`.

**Options**: The Options key can be used to customise the template. The value of this key is an array and each of the items is a dictionary with the following keys:

**Identifier**: Alias for Xcode to know which name we are using. Since we can have multiple options, we can differentiate based on the identifier. This identifier is also used to populate the text macros (`___FILEBASENAME___` and `___FILEBASENAMEASIDENTIFIER___`). Most of the templates only have one entry for the productName.

**Name**: Text that is shown in the wizard. As far as I can tell it is basically the label for your input field. 

**Required**: Boolean wether the option is required to be filled in.

**Type**: The type of the input you have to fill in. Since we are entering the productName, we set this value to text.

### ___FILEBASENAME___Interactor.swift

File that behaves as the template for the interactor in the VIP stack. 

```swift
//___FILEHEADER___

import Foundation

protocol ___VARIABLE_productName___Interactable {

}

final class ___FILEBASENAMEASIDENTIFIER___: ___VARIABLE_productName___Interactable {

    // MARK: - Properties

    private let presenter: ___VARIABLE_productName___Presentable

    // MARK: - Initializers

    init(presenter: ___VARIABLE_productName___Presentable) {
        self.presenter = presenter
    }

    // MARK: - ___VARIABLE_productName___Interactable

}
```

In above example you can see that we have something that looks half familiar and half not familiar. 

**___FILEHEADER___**: This is to tell Xcode to insert the default file header here. If you do not want to have the copyright header in your file, you can remove this line. 

**___VARIABLE_productName___**: Here you can see the productName in use that we have set in the `TemplateInfo.plist`. As this is the identifier this will be replaced with the value that you have entered as name for the module. 

E.g. If we enter Overview as the module name, ___VARIABLE_productName___ will be replaced by Overview.

**___FILEBASENAMEASIDENTIFIER___**: This is a populated text macro for Xcode. This is a combination of the identifier you have entered and the name of the file. If we have named the module Overview, this will be replaced with OverviewInteractor.

### ___FILEBASENAME___Presenter.swift
File that behaves as the template for the presenter in the VIP stack. 

```swift
//___FILEHEADER___

import Foundation

protocol ___VARIABLE_productName___Presentable {

}

final class ___FILEBASENAMEASIDENTIFIER___: ___VARIABLE_productName___Presentable {

    // MARK: - Properties

    weak var viewController: ___VARIABLE_productName___Displayable?

    // MARK: - ___VARIABLE_productName___Presentable

}
```

### ___FILEBASENAME___ViewController.swift

File that behaves as the template for the viewController in the VIP stack. 

```swift
//___FILEHEADER___

import UIKit

protocol ___VARIABLE_productName___Displayable: AnyObject {

}

final class ___FILEBASENAMEASIDENTIFIER___: UIViewController {

    // MARK: - Properties

    private let interactor: ___VARIABLE_productName___Interactable
    weak var delegate: ___FILEBASENAMEASIDENTIFIER___Delegate?

    // MARK: - Initializers

    init(interactor: ___VARIABLE_productName___Interactable) {
        self.interactor = interactor
        super.init(nibName: nil, bundle: nil)
    }

    @available(*, unavailable)
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }

    // MARK: - View lifecycle

    override func viewDidLoad() {
        super.viewDidLoad()
    }
}

// MARK: - ___VARIABLE_productName___Displayable
extension ___FILEBASENAMEASIDENTIFIER___: ___VARIABLE_productName___Displayable {

}
```

# Using the template

Now that our template is ready to be used, we only have to make sure it is in the right directory for Xcode to locate. The directory for your custom templates is:

```bash
~/Library/Developer/Xcode/Templates
```

If you have Xcode open, restart Xcode to make sure Xcode properly loads all the templates.
Now, let's create our first feature with the newly created templates:

![Template picker](/assets/custom-xcode-templates/template-picker.png)
![Template picker details](/assets/custom-xcode-templates/template-picker-details.png)

Which results into 3 files being created:

![Template result](/assets/custom-xcode-templates/created-files.png)