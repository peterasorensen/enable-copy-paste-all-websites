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
// @match        *://*.mheducation.com/*
// @grant        GM_addStyle
// @run-at       document-end
// ==/UserScript==

(function() {
    window.addEventListener('copy', function() {
        console.log("running tampermonkey script");
        // Function to replace oncopy and oncut attributes in HTML
        function replaceOnCopyAndCut(html) {
            return html.replace(/oncopy="return false"/g, 'oncopy="return true"')
                       .replace(/oncut="return false"/g, 'oncut="return true"')
                       .replace(/oncopy=&quot;return false&quot;/g, 'oncopy=&quot;return true&quot;')
                       .replace(/oncut=&quot;return false&quot;/g, 'oncut=&quot;return true&quot;')
                       .replace(/user-select: none;/g, 'user-select: text;');
        }

        // Get the entire HTML of the document
        var html = document.documentElement.innerHTML;

        // Replace the attributes in the HTML
        document.documentElement.innerHTML = replaceOnCopyAndCut(html);
    });
    GM_addStyle ( "                            \
      * {                                      \
      user-select: text !important;            \
     -moz-user-select: text !important;        \
     -ms-user-select: text !important;         \
     -khtml-user-select: text !important;      \
     -o-user-select: text !important;          \
     -webkit-user-select: text !important;     \
     -webkit-touch-callout: text !important;   \
       }                                       \
 " );
})();
```

To use this script:

1. Ensure you have Tampermonkey installed in your Chrome browser.
2. Click on the Tampermonkey icon in your browser and choose "Create a new script..."
3. Delete the content that is present in the new script template.
4. Copy and paste the script I provided above into the editor.
5. Save the script by clicking File > Save or by pressing Ctrl + S (Cmd + S on Mac).

The above script is set to `*://*.mheducation.com/*`. The @match *://*/* line in the metadata block tells Tampermonkey to run this script on every website. You can change this to be more specific if you only want it to run on certain sites.

When you visit a website, this script will execute when you try to copy (CMD+C on Mac) and modify the oncopy attribute of each element, if it is set to return false. This should enable the copy functionality on most sites that use this method to prevent copying.


## Example: Allow Copy Paste on McGraw Hill Connect readers
1. Install the script as stated above. 
2. Right click on the page and Inspect element.
3. Hit Esc to pull up the bottom drawer.
4. Navigate to Network request blocking.
 <img width="537" alt="image" src="https://github.com/peterasorensen/enable-copy-paste-all-websites/assets/23510568/da3165c3-82fa-4735-b67a-e08bbfe8ba4a">
 
5. Add this URL `prod.reader-ui.prod.mheducation.com/scripts.*.js`.
This blocks McGraw Hill from loading the script that creates that annoying highlight pop-up.
