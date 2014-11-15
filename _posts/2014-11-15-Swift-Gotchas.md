---
layout: post
date: 2014-11-15 17:00:00
---

A collection of gotchas encountered while porting NimbusKit to Swift.

<!-- more -->

## Classes can't be cast as objects that conform to protocols

Swift's type-safety generally leads to cleaner code at the expense of hackery that was possible in Objective-C.

NimbusKit encouraged the following pattern when creating a `NITableViewModel`:

~~~
_model = [[NITableViewModel alloc] initWithSectionedArray:contents delegate:(id)[NICellFactory class]];
~~~
{: .language-objectivec}

NICellFactory implemented `NITableViewModelDelegate` both as instance methods **and** as class methods. This treated `NICellFactory.class` like a singleton that implemented `NITableViewModelDelegate`. While convenient, it was not necessarily *safe*.

In Swift you cannot do an `(id)` cast of a class to a protocol. This forced NimbusKit's new [TableCellFactory](https://github.com/NimbusKit/swift/blob/master/TableCellFactory.swift) class to provide a non-mutable singleton instance that could be used in place of the old `(id)[NICellFactory class]`. This had the nice side effect of eliminating duplicate code paths!

The new way to create a model using the global `TableCellFactory` is as follows:

~~~
self.model = TableModel(sections: sections, delegate: TableCellFactory.tableModelDelegate())
~~~
{: .language-swift}

`TableCellFactory` doesn't expose the singleton instance directly. Instead, it returns a `TableCellFactory`-conformant object. This hides the implementation of the singleton and ensures that the global `TableCellFactory` behaves consistenly in every setting.

## Unable to use generic objects as delegates

The compiler allows you to do so but the Objective-C runtime will not know that the protocol methods are implemented. This results in unexpected behavior at best. At worst you'll encounter run-time crashes due to unimplemented required selectors - even though they *are* implemented.

This is likely due to the inability to declare Objective-C selectors for methods on generic classes in Swift.

Example:

~~~
import Foundation
import UIKit

class DataSourceProvider <T: AnyObject> : NSObject, UITableViewDataSource {
  func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return 1
  }

  func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
    let reuseIdentifier = "reuse"
    if let cell = tableView.dequeueReusableCellWithIdentifier(reuseIdentifier) as? UITableViewCell {
      return cell
    }
    return UITableViewCell(style: .Default, reuseIdentifier: reuseIdentifier)
  }
}

class DelegateViewController: UITableViewController {
  let provider = DataSourceProvider<NSObject>()

  override func viewDidLoad() {
    super.viewDidLoad()

    self.tableView.dataSource = self.provider
  }
}
~~~
{: .language-swift}

If you attempt to display this controller the app will crash due to an "unrecognized selector sent to instance".

Making the class non-generic will allow you to use instances as delegates as expected.

This has impacted the way NimbusKit's [TableActions](https://github.com/NimbusKit/swift/blob/master/TableActions.swift) are implemented. Due to the above limitation, TableActions can't take advantage of generics. *sadpanda*
