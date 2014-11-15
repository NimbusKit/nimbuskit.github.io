---
layout: post
---

A collection of gotchas encountered while porting NimbusKit to Swift.

<!-- more -->

## Unable to use generic objects as delegates

The compiler allows you to do so, but the Objective-C runtime will not know that the protocol methods are implemented. This results in unexpected behavior at best, or at worst run-time crashes due to unimplemented selectors - even though they are most certainly implemented.

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
    if let cell: UITableViewCell = tableView.dequeueReusableCellWithIdentifier(reuseIdentifier) as UITableViewCell {
      return cell
    }
    return UITableViewCell(style: .Default, reuseIdentifier: reuseIdentifier) as UITableViewCell
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
