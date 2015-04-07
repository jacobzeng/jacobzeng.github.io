---
layout: post
title:  "IOS: How to design prototype cell in storyboard"
date:   2015-04-07 17:00:00
categories: ios
tags: ios uitableview
---

>IOS SDK 8.2, Xcode Version 6.2

Sometimes, you want to design the UI of prototype cell in storyboard, it's very straightforward.

Define the UI what you want
-------

1. Select `Table View`, show `Attributes inspector`, find the `Prototype Cells`
![find prototype cells attribute]({{site.url}}/assets/How to add prototype cell in storyboard/5.png)

2. Set attribute `Prototype Cells` to 1, sure of that you can set more prototype cells if you want
![set prototype cells attribute to 1]({{site.url}}/assets/How to add prototype cell in storyboard/10.png)

3. Set attribute `Row Height` for more space to hold ui elements in cell
![set row hegiht]({{site.url}}/assets/How to add prototype cell in storyboard/20.png)

4. Select `Table View Cell`, set attribute `Identifler`, will use it latter
![set identifler]({{site.url}}/assets/How to add prototype cell in storyboard/15.png)

Create custom UITableViewCell for hosting the prototype cell
------

5. Create custmom UITableViewCell class, named `ContentTableViewCell` in this example
![create ContentTableViewCell]({{site.url}}/assets/How to add prototype cell in storyboard/25.png)

6. Select `Table View Cell`, show `Identity inspector`, binding attribute `Class` to new created class `ContentTableViewCell`
![set attribute Class]({{site.url}}/assets/How to add prototype cell in storyboard/30.png)

7. Show Assistant Editor
![Show Assistant Editor]({{site.url}}/assets/How to add prototype cell in storyboard/35.png)

8. Select `Content View`, then select `ContentTableViewCell.h` file in `Assistant Editor`
![select ContentTableViewCell.h]({{site.url}}/assets/How to add prototype cell in storyboard/40.png)

9. Outlet the UI elements in the cell
![outlet ui elements in the cell]({{site.url}}/assets/How to add prototype cell in storyboard/45.png)

Implements (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)
------

10. Define the key for identifler of `cellContent`
![define key]({{site.url}}/assets/How to add prototype cell in storyboard/50.png)

11. Import `ContentTableViewCell.h` and `PrefixHeader.pch` in ViewController
![55]({{site.url}}/assets/How to add prototype cell in storyboard/55.png)

12. Implement the `cellForRowIndexPath`

~~~
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    
    ContentTableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:kCellContent];
    if(!cell){
        cell = [tableView dequeueReusableCellWithIdentifier:NSStringFromClass([ContentTableViewCell class])];
    }
    
    [self renderCellContent:cell forRowAtIndexPath:indexPath];
    return cell;
}

- (void)renderCellContent:(ContentTableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath{
    
    NSDictionary *data = contents[indexPath.section];
    
    cell.labelTime.text = [data objectForKey:@"time"];
    cell.labelContent.text = [data objectForKey:@"content"];
}
~~~
{: .language-objective-c}

