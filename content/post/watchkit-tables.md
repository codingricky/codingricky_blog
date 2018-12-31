+++
author = "Ricky Yim"
categories = ["swift", "ios", "watchkit", "apple watch", "table"]
date = 2015-04-25T12:29:19Z
description = ""
draft = false
slug = "watchkit-tables"
tags = ["swift", "ios", "watchkit", "apple watch", "table"]
title = "Using WatchKit tables"
aliases = [
    "/watchkit-tables/"
]
+++

The release of the Apple Watch has meant a whole host of possiblities for application developers. However writing apps for watches is very different to writing apps for phones. One particular area that is different is the User Interface elements. This article will explore how to display data in a table on a watch using the WatchKit components. 

These are the pre-requisites for this article:

* Git
* XCode 6.3

The application will be a simple table showing a list of names and ages of people. 

![](/content/images/2015/04/Bezel-Snapshot-2015-04-25-22-18-24.png)

1.Start by checking out the src code from https://github.com/codingricky/sample-watchkit-table. 

2.If you go to the **Interface.storyboard** of WatchTable WatchKit app, you will see a very basic scene with a table.

![](/content/images/2015/04/storyboard.png)

Important to note that the TableRowController has a custom class the **TableRow**. There are outlets attached from the labels to the **TableRow** class. 

The **TableRow** class looks like:

```swift
import WatchKit

class TableRow: NSObject {
    @IBOutlet weak var nameLabel: WKInterfaceLabel!   
    @IBOutlet weak var ageLabel: WKInterfaceLabel!
}
```

Also the identifier of the Table is set to **tableRow** which will be referenced later on.

3.Open the **InterfaceController** class. 

We will define a map with some sample data that will be displayed in the watch table. Obviously you can source data from anywhere. The map is purely to simplfy things in this article.

```
    let personData = ["Taylor" : 25, "Katy" : 30, "Ellie" : 28]
```

4.In the **willActivate()** fuction we will load the data into the table.

We will set the row count of the table like this.

```swift
        table.setNumberOfRows(personData.count, withRowType: "tableRow")
```
Notice how the row identifier **tableRow** is referenced.
 
5.Lastly we load the data into the table.

```swift
   let keys = personData.keys.array
        for (index, key) in enumerate(keys) {
            let tableRow = table.rowControllerAtIndex(index) as! TableRow
            tableRow.nameLabel.setText(key)
            tableRow.ageLabel.setText(personData[key]!.description)
        }
```

Now if you start the application, you will see the watch with the data loaded. You can source the data from anywhere. I hope that helps you on your way to building watch applications.

