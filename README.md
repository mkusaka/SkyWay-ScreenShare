English | [日本語](README_ja.md)

# Deprecated!

We have released a new WebRTC platform, [ECLWebRTC](https://webrtc.ecl.ntt.com/en/?origin=skyway), to take the place of SkyWay. We will be shutting down the SkyWay servers in March 2018. Customers who are currently using SkyWay are required to migrate to ECLWebRTC by then or their services will stop working.

If you are looking for the repository of ECLWebRTC, please see the [new ScreenShare Library](https://github.com/skyway/skyway-screenshare).

# SkyWay ScreenShare Library

This is a library for easy implementation of screen sharing function in the WebRTC of Web applications.
This repository includes the source code of Chrome extension and Firefox add-on.

## Installation

### 1. Library

* In case of using CDN


	```html
	<script src="https://skyway.io/dist/screenshare.js"></script>
	```

* In case of self building

	Clone a copy of the SkyWay-ScreenShare git repo by running:
	```
	git clone git@github.com:nttcom/SkyWay-ScreenShare.git
	```

	Enter the SkyWay-ScreenShare directory and run the build script of JavaScript library:
	```
	cd SkyWay-ScreenShare && npm install && npm run build-library
	```

	Use the generated libraries:
	```
	SkyWay-ScreenShare/dist/screenshare.js
	SkyWay-ScreenShare/dist/screenshare.min.js
	```

### 2. Chrome extension

Modify the manifest.json <`SkyWay-ScreenShare/chrome-extension/src/manifest.json`>:
```json
{
  "name": "Your extension name here",
  "short_name": "Your extension short_name here",
  "version": "Your extension version number here",
  "manifest_version": 2,
  "description": "Your extension description here",
  "icons": {
    "16": "icon16.png",
    "48": "icon48.png",
    "128": "icon128.png"
  },
  "permissions": [
    "desktopCapture",
    "tabs"
  ],
  "background": {
    "scripts": ["background.js"],
    "persistent": false
  },
  "content_scripts": [{
    "matches": [""],
    "js": ["content.js"],
    "all_frames": true,
    "run_at": "document_end"
  }]
}
```
Essential modification items are as follows:

|Item|Comment|
|---|---|
|name|Your extension name here.|
|short_name|Your extension short_name here.|
|version|Your extension version number here.|
|description|Your extension description here.|
|icons|Your extension icon files name here.<BR>Icon files should be located here <`SkyWay-ScreenShare/chrome-extension/src/`>.<BR>Icon files of SkyWay is included with in this repository.|
|matches|The site urls that will use this extension. <BR>Wildcards are accepted.<BR>Ex：`"matches": ["https://*.skyway.io/*"]`|


Enter the SkyWay-ScreenShare directory and run the build script for the Chrome extension:
```
cd SkyWay-ScreenShare && npm install && npm run build-chrome
```


Test the extension on Chrome:

1. Access to chrome://extensions/
2. Enable the 'Developer mode'
3. Click the 'Load unpacked extension...' and specify the following directory
```
SkyWay-ScreenShare/chrome-extension/screenshare_chrome_extension/
```


Publish the extension:
If you publish to the Chrome Web Store, please use of the following Zip file.
```
SkyWay-ScreenShare/chrome-extension/screenshare_chrome_extension.zip
```

### 3. Firefox add-on

Modify the main.js <`SkyWay-ScreenShare/firefox-addon/src/lib/main.js`>
```javascript
var pageMod = require('sdk/page-mod');
var self = require('sdk/self');
var __temp = require('chrome');
var Cc = __temp.Cc;
var Ci = __temp.Ci;

var domains_to_add = [''];
var addon_domains = [];
var allowed_domains_pref = 'media.getusermedia.screensharing.allowed_domains';
var enable_screensharing_pref = 'media.getusermedia.screensharing.enabled';

 ....

```
Essential modification items are as follows:

|Item|Comment|
|---|---|
|domains_to_add|Must be specified the domain of the site to enable the ScreenShare function.<BR>Wild card is available in the domain.<BR>Ex：`var domains_to_add = ['*.skyway.io']`|


Modify the package.json `SkyWay-ScreenShare/firefox-addon/src/package.json` を修正します。
```json
{
    "name": "your_add-on_name_here",
    "title": "Your add-on title here",
    "description": "Your add-on description here",
    "author": "Your add-on author here",
    "version": "Your add-on version number here",
    "license": "Your add-on license here",
    "homepage": "Your add-on homepage url here",
    "icon64": "icon64.png",
    "icon": "icon48.png"
}
```
Essential modification items are as follows:

|Item|Comment|
|---|---|
|name|Your add-on name here.|
|license|Your add-on license here.|
|title|Your add-on title here.|
|description|Your add-on description here.|
|author|Your add-on author here.|
|version|Your add-on version number here.|
|homepage|Your add-on homepage url here.|
|icon,icon64|Your add-on icon files name here.<BR>Icon files should be located here <`SkyWay-ScreenShare/firefox-addon/src/`>.|


Installing the cfx command-line tool:

Foe more, see this web site:
https://developer.mozilla.org/en-US/Add-ons/SDK/Tutorials/Installation


Enter the SkyWay-ScreenShare directory and run the pre build script of add-on:
```
cd SkyWay-ScreenShare && npm install && npm run pre-build-firefox
```
【Attention】Pre built will fail because the id is not described in `package.json`, but this is not a problem.
```

 ....

No 'id' in package.json: creating a new ID for you.
package.json modified: please re-run 'cfx xpi'

 ....

```

Enter the SkyWay-ScreenShare directory and run the build script of add-on:
```
cd SkyWay-ScreenShare && npm install && npm run build-firefox
```

Test the generated add-on file on the Firefox:

1. Access to about:addons
2. Drag-and-drop the add-on file `SkyWay-ScreenShare / firefox-addon / screenshare_firefox_addon.xpi` that are generated on the browser


Publish the Addo-On:
Please the following add-on file to download ready in the service site.
```
SkyWay-ScreenShare/firefox-addon/screenshare_firefox_addon.xpi
```

## API reference

```javascript
var screenshare = new SkyWay.ScreenShare([options]);
```

- options
  - debug (boolean)
    - Output the debug log on the browser developer console.

### startScreenShare

- Start the screen share.

```javascript
screenshare.startScreenShare({
	"Width": <number>,
	"Height": <number>,
	"FrameRate": <number>
},function(stream){
 // success callback
 // If successful, you can get a own stream object
},function(error) {
 // error callback
},function() {
 // onEnded callback(optional/Chrome only)
 // You fire when the screen share has been finished.
});
```

### stopScreenShare

- Stop the Screen share.
 - Executing the stop() method of the stream object obtained in startScreenShare
 - Dedicated method implementation plan future

```javascript
stream.stop();
```

### isEnabledExtension

- Check whether the extension is installed or not. `<true or false>`

```javascript
var result = screenshare.isEnabledExtension();
```

### Event

#### type=ScreenShareInjected

- In case of [inline installation](https://developer.chrome.com/webstore/inline_installation) of Chrome extension, this event is fired when loading script is completed. With handling this event, you can start ScreenShare feature automatically. Please note that you have to change config.hostname in chrome-extension/src/background.js.

```javascript
window.addEventListner('message', function(ev) {
  if(ev.data.type === "ScreenShareInjected") {
    console.log('screen share extension is injected, get ready to start');
    startScreenShare();
  }
}, false);
```

## Sample

### SkyWay ScreenShare Sample App

https://skyway.io/examples/screenshare/

  - [Install the Chrome extension](https://chrome.google.com/webstore/detail/skyway-screenshare-sample/lhekboeoffbecdmcgmgeomcpgehiogfj?hl=ja&gl=JP&authuser=2)
  - [Install the Firefox add-on](https://skyway.io/examples/screenshare/screenshare_firefox_addon.xpi)

## Contributing

1. [Fork it](https://github.com/nttcom/SkyWay-ScreenShare/fork)
1. Create your feature branch (git checkout -b my-new-feature)
1. Commit your changes (git commit -am 'Add some feature')
1. Push to the branch (git push origin my-new-feature)
1. Create a new Pull Request

## LICENSE & Copyright

[LICENSE](./LICENSE)
