# enable-copy-paste-all-websites
Enable copy and paste functionality on websites that try to block it. 

Helpful if you have other extensions like Google Translate that translates highlighted or copied text and your textbook readers try to block it. 

```javascript
// ==UserScript==
// @name         Enable Copy Function
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  Enable copying on web pages that have disabled it
// @author       peterasorensen
// @match        *://*/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    // Function to replace oncopy attribute
    function enableCopy(element) {
        if(element.getAttribute('oncopy') === 'return false') {
            element.setAttribute('oncopy', 'return true');
        }
    }

    // Get all elements in the document
    var allElements = document.getElementsByTagName('*');

    // Loop through all elements and enable copy
    for (var i = 0; i < allElements.length; i++) {
        enableCopy(allElements[i]);
    }
})();
```

To use this script:

1. Ensure you have Tampermonkey installed in your Chrome browser.
2. Click on the Tampermonkey icon in your browser and choose "Create a new script..."
3. Delete the content that is present in the new script template.
4. Copy and paste the script I provided above into the editor.
5. Save the script by clicking File > Save or by pressing Ctrl + S (Cmd + S on Mac).

The @match *://*/* line in the metadata block tells Tampermonkey to run this script on every website. You can change this to be more specific if you only want it to run on certain sites.

When you visit a website, this script will automatically execute and modify the oncopy attribute of each element, if it is set to return false. This should enable the copy functionality on most sites that use this method to prevent copying.


## Example: Allow Copy Paste on McGraw Hill Connect readers
1. Install the script as stated above. 
2. Right click on the page and Inspect element.
3. Hit Esc to pull up the bottom drawer.
4. Navigate to Network request blocking.
 <img width="537" alt="image" src="https://github.com/peterasorensen/enable-copy-paste-all-websites/assets/23510568/da3165c3-82fa-4735-b67a-e08bbfe8ba4a">
5. Add this URL `prod.reader-ui.prod.mheducation.com/scripts.*.js`.
This blocks McGraw Hill from loading the script that creates that annoying highlight pop-up.
