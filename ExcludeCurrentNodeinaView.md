  ***DRUPAL VIEWS: EXCLUDE CURRENT NODE FROM A LIST VIEW***
  https://www.drupal4u.org/tips-and-tricks/drupal-views-exclude-current-node-list-view
  
In some situations, for example a block listing nodes related to the node being viewed, you might wish to exclude the current node from a list view.

Views 3

Click on the "advanced" tab.
Click on add under "contextual filters".
Choose Content:nid.
Under "when the filter variable is not available, choose "provide default value".
From the drop down menu select "content id from url".
Scroll all the way down to the bottom of the window and click on the "More" link.
Click "Exclude"
Views 2

Use views arguments, assuming this is a block view

Add an argument for Node: nid
Set 'Action to take if argument is not present' to 'Provide default argument'
Set 'Default argument type' to 'Node ID from URL'
Check 'Exclude the argument'
See: https://www.drupal.org/node/131547
  
  
  
  ```php
  ```
  
  
    ```php
  ```
  
  
    ```php
  ```
  
  
    ```php
  ```
