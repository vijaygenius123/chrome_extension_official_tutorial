# chrome_extension_official_tutorial

# Step 1 

Create a manifest file the details below
```json
{
    "name": "Name Of Your Extension",
    "version": "X.X",
    "description": "Extension Description!",
    "manifest_version": 2
}
```
The manifest_version has to be  2 since manifest_version 1 is not supported anymore.

# Step 2

Add below configuration so a script can run in the background, we will also add permissions for storage since we will be using the storage to store a value

```json
    "permissions": [
        "storage"
    ],
    "background": {
        "scripts": [
            "background.js"
        ],
        "persistent": false
    },
```

Simple backgroud script, which will set the key storage to value of `#3aa757` when extension is installed.

```js
chrome.runtime.onInstalled.addListener(function () {
    chrome.storage.sync.set({ color: '#3aa757' }, function () {
        console.log("The color is green.");
    });
});
```

# Step 3

Create a simple html for the popup 

```html
<!DOCTYPE html>
<html>

<head>
    <style>
        body {
            min-height: 200px;
            min-width: 200px;
        }

        button {
            outline: none;
        }
    </style>
</head>

<body>
    <button id="changeColor">Change Color</button>
</body>

</html>
```

add below configuration to manifest file so the popup opens when clicked on extension icon

```json
    "browser_action": {
        "default_popup": "popup.html"
    },
```