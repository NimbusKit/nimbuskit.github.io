---
layout: post
date: 2014-11-15 17:00:00
---

A collection of gotchas encountered while porting NimbusKit to Swift.

<!-- more -->

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
