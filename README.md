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

# Step 4 

Add the below lines  inside the onInstalled listener function 
```js
    chrome.declarativeContent.onPageChanged.removeRules(undefined, function () {
        chrome.declarativeContent.onPageChanged.addRules([{
            conditions: [new chrome.declarativeContent.PageStateMatcher({
                pageUrl: { hostEquals: 'developer.chrome.com' },
            })
            ],
            actions: [new chrome.declarativeContent.ShowPageAction()]
        }]);
    });
```

Also open permission `declarativeContent` to permissions in the manifest file


# Step 5 

Add `activeTab` to permissions list in the manifest file

create a file called `popup.js` with content below

```js
let changeColor = document.getElementById('changeColor');

chrome.storage.sync.get('color', function (data) {
    changeColor.style.backgroundColor = data.color;
    changeColor.setAttribute('value', data.color);

});

changeColor.onclick = function (element) {
    let color = element.target.value;
    chrome.tabs.query({ active: true, currentWindow: true }, function (tabs) {
        chrome.tabs.executeScript(
            tabs[0].id,
            { code: 'document.body.style.backgroundColor = "' + color + '";' });
    });
};
```

add the created `popup.js` as a script tag in `popup.html`
```html
<script src="popup.js"></script>
```