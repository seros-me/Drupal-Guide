  ***DRUPAL VIEWS: EXCLUDE CURRENT NODE FROM A LIST VIEW***
  https://www.drupal4u.org/tips-and-tricks/drupal-views-exclude-current-node-list-view
  
In some situations, for example a block listing nodes related to the node being viewed, you might wish to exclude the current node from a list view.

Views 3  Comprobado OK en un bloque

1. Click on the "advanced" tab.
2. Click on add under "contextual filters".
3. Choose Content:nid.
4. Under "when the filter variable is not available, choose "provide default value".
5. From the drop down menu select "content id from url".
6. Scroll all the way down to the bottom of the window and click on the "More" link.
7. Click "Exclude"
8. Views 2

Use views arguments, assuming this is a block view

1. Add an argument for Node: nid
2. Set 'Action to take if argument is not present' to 'Provide default argument'
3. Set 'Default argument type' to 'Node ID from URL'
4. Check 'Exclude the argument'


See: https://www.drupal.org/node/131547
  
  
  
  ```php
  ```
  
  
    ```php
  ```
  
  
    ```php
  ```
  
  
    ```php
  ```
