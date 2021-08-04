# ALL ISSUES NOTICED:


# 1) Double-clicking to edit items not working
## Severity: High
## Description:
When a to-do item is double-clicked, the user should be presented with an input field that allows them to edit the item.  Instead, nothing happens.  
Steps to replicate:
1. Add at least one item to the list
2. Double click (or single click / hover), noting that no editing option appears.

## Proposed Solution:
In `app.jsx`, the state variable has a property `editing` which points to the ID of the to-do item that is being edited, or "null" if no item is being edited.  Everything was already set up to allow editing once this property pointed to the correct ID; the only thing missing was a way for the user to change the `editing` property.  I added the property `onDoubleClick={this.handleEdit}` to the todoItem model, line 84 of `todoItem.jsx`.  With this in place, double-clicking an item allows the user to edit that item.

---

# 2) Filters are not filtering
## Severity: High
## Description:
Clicking the "All", "Active", or "Completed" buttons in the footer should filter the items that the user is viewing.  Clicking these buttons does properly update the route, as expected, but no items are being filtered out.  
Steps to replicate:
1. Add some items to the list
2. Check off some, but not all, of the items
3. Click any of the filters in the footer
4. Note that the route in the address bar does update, but the items do not change

## Proposed Solution:
In the original version, there was a filter function (`app.jsx` lines 90 - 92), but no matter which filter was selected all of the to-dos were being returned.  I have written a filter function that uses the filter specified in the `nowShowing` property to return only the expected items.

---

# 3) "Clear completed" link doesn't work
## Severity: High
## Description:
Clicking the "Clear completed" link in the footer does not do anything.  
Steps to replicate:
1. Add items to list
2. Check off one or more items
3. Click "Clear Completed" and note that completed items remain on the list

## Proposed Solution:
When clicked, this should remove all of the to-do items that are marked as completed.  There is a prototype function in `todoModel.js` line 84 that runs when the link is clicked, but currently there is nothing written in that function.  To make this function work as expected, the easiest way is probably to write a function that is structured like the `destroy` prototype function (lines 67 - 71 of `todoModel.js`), except instead of returning `candidate !== todo` this filter would return `!candidate.completed`.

---

# 4) New items showing up at top
## Severity: Medium
## Description:
Upon adding a new to-do item, the new item appears at the top of the list.  As shown in the Business Requirements Video, however, the newest item should appear at the bottom of the list.  
Steps to replicate:
1. Add at least one item to the list
2. Add a new item, and note where it appears.

## Proposed Solution:
This problem stems from the addTodo prototype method beginning in line 31 of todoModel.js.  It can be solved simply by reversing the two items added to the array, bringing the spread operator `...this.todos` before the new item entry instead of placing it at the end.

---

# 5) "All" button (top left)
## Severity: Medium
## Description:
On clicking this button, every item's "completed" property changes from its previous state.  
Steps to replicate:
1. Add some items to the list
2. Check off some, but not all, of the items
3. Click the button to the left of the "new item" input, and note that every item on the list changes its status.

## Proposed Solution:
I am actually not totally sure what the expected behavior is -- the button is only ever clicked in the Business Requirements Video when either all of the items are checked or all of the items are not checked.  To me, however, toggling the state of every single item does not seem like a particularly useful feature; as a user, I would expect a behavior more along the lines of "set all items to checked, unless all items are already checked in which case set all items to unchecked."  This way, with a mixed list, one click would set all items to checked and two clicks would set all items to unchecked.
To achieve this, I would declare a variable `newCompletedStatus` as false initially, then loop through all the items.  As soon as the loop finds one item that is not completed, `newCompletedStatus` is set to true and the loop breaks (it will finish the loop as "false" only if every item on the list is already completed).  Then, having finished this, loop through the items one more time, setting every item's `completed` property to equal the value of `newCompletedStatus`.

---

# 6) Footer displaying when list is empty
## Severity: Low
## Description:
When there are no items in the to-do list, the footer should not display.  In the app as written, the footer displays all the time, even before any items are added.  
Steps to replicate:
1. Remove all items (if any) from list
2. Note that the footer still appears

## Proposed Solution:
This can be fixed with a ternary.  In the return statement in app.jsx, replace `{footer}` (line 157) with `{todos.length ? footer : null}`

---

# 7) Wrong background color
## Severity: Low
## Description:
The background color of the list is white, rather than the pale yellow color (approximate hex code #FEF3C7) that appears in the Business Requirements Video.

## Proposed Solution:
This can be solved by adjusting the appropriate CSS.  The relevant areas are:
- .todoapp, specifically the background-color property (line 45)
- `.todo-list li.editing .edit`, specifically the background-color property (line 170)
- `.footer:before`, specifically the box-shadow property (originally lines 275 and 277, now lines 279 and 281)

---

# 8) Styling on Remove button
## Severity: Low
## Description:
The Remove button, which displays on the right when hovering over a to-do item, looks different from the remove button in the Business Requirements Video.  
Steps to replicate:
1. Add at least one item to the to-do list
2. Hover over the item, and note the X that appears on the right.

## Proposed Solution:
Currently it displays as a red X; in the video, it displays as a light-colored X over a circular grey background.  This should just be a matter of adjusting the CSS on that button.  Some relevant properties here will be `color` (to set the X itself), `background-color` (to set the background), and `border-radius: 100%` (to display as a circle rather than a square).

---

---


# Fixes:

## 1) Double clicking to edit items
- I implemented this solution as described above.
- To test this solution, enter at least one item onto the to-do list, then double-click the item.  The item will turn from standard text into an input field, and the text can be edited.  To save the edited text, press "enter" or click anywhere outside of the input field.
	
---
    
## 2) Filters
- I added a few lines of code to the filter used to define the variable `shownTodos`, as described above.
- To test that the filters works properly, add items to the to-do list, then check off some but not all of the items.  Then, click any of the filters in the footer.  When any of these filters is clicked, only the items with the requested "completed" status should display.
	
## 3) Footer with empty list
- I added the ternary that I described, which hides the footer when the list is empty
	
## 4) Background color
- I updated the background color of the to-do list to the creamy yellow color shown in the video

