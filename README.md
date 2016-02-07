#enyo-luneos

A selection of Enyo controls and functionality ported from Enyo 1.0 for next-gen LuneOS devices, with backward support for legacy webOS devices.


##AppMenu

Ported from Enyo 1, the AppMenu kind will replicate the behavior of the standard app menu shown in webOS apps. This will have a slightly different style, though, since it uses onyx.Menu as opposed to a custom background image.

**Example:**

	{kind:AppMenu, onSelect: "appMenuItemSelected", components: [{content:"Do something", ontap: "doSomething"}]}

##ApplicationEvents

A convenient subkind of _enyo/Signals_ that outlines all of the webOS-specific events from webos-lib.

**Example:**

     {kind: ApplicationEvents, onbackgesture: "handleBackGesture", onactivate: "handleActivate", ondeactivate: "handleDeactivate", onmenubutton: "handleMenuButton", onwebOSLaunch: "handleLaunch", onwebOSRelaunch: "handleRelaunch", onlowmemory:"handleLowMemory", onvirtualkeyboard: "handleVirtualKeyboard"}
	 
##CoreNavi

An in-app gesture area that can be used for debugging. Emulates the Open webOS back gesture by default, set fingerTracking to true in order to emulate the new Finger Tracking API events. Only shows itself on non-palm platforms, so it's safe to ship with your app.

     //KeyUp-based Gesture
     components: [
     	{kind: Signals, onkeyup: "handleKeyUp"},
     	{kind: CoreNavi, fingerTracking: false}
     ],
     handleKeyUp: function(inSender, inEvent) {
     	if(inEvent.keyIdentifier == "U+1200001") {
     		//Do Stuff
     	}
     }
     
     //Finger Tracking API
     components: [
     	{kind: "Panels",
     	arrangerKind: "CarouselArranger",
     	components:[
     		{content: "Foo"},
     		{content: "Bar"},
     		{content: "DecafIsBad"},
     	]},
     	{kind: "CoreNavi",
               fingerTracking: true,
               onCoreNaviDragStart: "handleCoreNaviDragStart",
               onCoreNaviDrag: "handleCoreNaviDrag",
               onCoreNaviDragFinish: "handleCoreNaviDragFinish"
          }
     ],
     handleCoreNaviDragStart: function(inSender, inEvent) {
     	this.$.CoreNaviPanels.dragstartTransition(inEvent);
     },
     handleCoreNaviDrag: function(inSender, inEvent) {
     	this.$.CoreNaviPanels.dragTransition(inEvent);
     },
     handleCoreNaviDragFinish: function(inSender, inEvent) {
     	this.$.CoreNaviPanels.dragfinishTransition(inEvent);
     },
	

**Example:** 	 
	 
##CrossAppUI

Ported from the non-published set of Enyo 1 APIs to Enyo2, CrossAppUI takes a 'path' parameter (the HTML file to open) and displays it inside your application.
The child application can pass stringified JSON prefixed with 'enyoCrossAppResult=' up to the CrossAppUI via the 'message' event (window scope). CrossAppUI will strip off the prefix, parse it into an object and fire onResult. This is intended to be used as a base class for app-in-app kinds, such as FilePicker (see below).

**message Event Example:**

     "enyoCrossAppResult={\"result\":[{\"fullPath\":\"/path/to/selected/file.foo\",\"iconPath\":\"/var/luna/extractfs//path/to/selected/file.foo:0:0:\",\"attachmentType\":\"image\",\"dbId\":\"++ILuOICkjNDQaUP\"}]}"

**Corresponding onResult Output:**

     {"fullPath":"/path/to/selected/file.foo","iconPath":"/var/luna/extractfs//path/to/selected/file.foo:0:0:","attachmentType":"image","dbId":"++ILuOICkjNDQaUP"}

**Example:**

     {kind:CrossAppUI, style: "width: 100%; height: 100%;", path: "/path/to/app/html.html", onResult: "handleResult"}

##FilePicker

Ported across from Enyo 1, this is a CrossAppUI kind that points to the built-in webOS filepicker. The onPickFile event is called when the file is chosen.

**onPickFile Output:**

     {"fullPath":"/path/to/selected/file.foo","iconPath":"/var/luna/extractfs//path/to/selected/file.foo:0:0:","attachmentType":"image","dbId":"++ILuOICkjNDQaUP"}

**Example:**

     {name: "ImagePicker", kind: FilePicker, fileType:["image"], onPickFile: "selectedImageFile"}

##HtmlContent

Syntactical sugar for a Control with `allowHtml:true`.
Like the Enyo 1 Control of the same name.

**Example:**

	{kind:HtmlContent, content:"This content is<br />separated by an HTML line break (&lt;br /&gt;) tag"}

##ModalDialog

Another kind ported from Enyo 1, this is an onyx.Popup that has `modal:true` and `autoDismiss:false` set to act like a modal dialog

**Example:**

	{name: "myDialog", kind:ModalDialog, components[/* your components */, { kind: onyx.Button, content: "Close popup", ontap: "closePopup"}]}
	closePopup: function() {this.$.myDialog.hide()};
	
##PortsHeader

An onyx.Toolbar that displays the app icon, a custom title and an optional random tagline.

**Example:**

     {kind: PortsHeader,
     title: "FooApp",
     taglines: [
          "My foo-st app",
          "Banana boat.",
          "Fweeeeeeep. F'tang."
     ]}

##PortsSearch

A variant of the PortsHeader that contains an animated, expandable search bar. onSearch is fired based on the 'instant' member variable. If true, it will fire every time the text is changed, otherwise it will fire when the user presses enter or the field loses focus after modification. Setting the 'submitCloses' variable to true will close the search box in this situation.

**Example:**

     {kind: PortsSearch,
     title: "SearchyFooApp",
     instant: false,
     submitCloses: true,
     taglines: [
          "My foo-st app",
          "Banana boat.",
          "Fweeeeeeep. F'tang."
     ],
     onSearch: "searchFieldChanged"}	

	 
##ProgressOrb

A circular variant of the onyx progress bar with an button in the center. Uses an enyo.Animator for smooth transitions between values.
Published properties are value, min and max.
This is an enyo2 reimagining of the progress indicator from the webOS 2.x Browser.

**Example:**

     {name: "FooOrb",
     kind: ProgressOrb,
     style: "position: absolute; right: 8px; bottom: 8px;",
     content: "O",
     onButtonTap: "buttonTapped"},
     buttonTapped: function(inSender, inEvent) {
	this.$.FooOrb.setValue(this.$.FooOrb.value + 100);
     },

##SymKey

Static symkey functionality for webOS 1.x and 2.x.
	
When the symkey on the physical keyboard is pressed, this properly opens the
symtable within webOS.  Automatically opens on the symkey, but can also be
manually activated from `showSymTable()`.

##WebView

This is a port of code from Enyo 1 to Enyo 2 to enable the use of a WebView widget
(think of it like an iframe) inside an Enyo app on webOS. This uses Enyo 1 code and
was slightly modified to enable it to work with Enyo 2. For complete documentation,
refer to [this document](https://developer.palm.com/content/api/reference/enyo/enyo-api-reference.html#enyo.WebView)
(ignore the Inheritance section and all other kinds)

------------

##LuneOS.js

An extension of the [webOS.js](https://github.com/webOS-DevRel/webOS.js) script, providing 
additional functionality and webOS API standardization. Like webOS.js, it does not strictly 
require Enyo and can be used separate if desired. Feature focuses include:

* Device orientation APIs
* Utility APIs (clipboard, LED notification, textindexer, etc.)
* Virtual keyboarc APIs
* Window card APIs
* WebOS event APIs (original events like backgesture, menubutton, etc.; polyfills formodern standard webOS events like webOSLaunch, webOSRelaunch, visibilitychange)

-----------

Unified from webOS-Ext, webos-ports-lib & enyo1-to-enyo2 kinds by Jason Robitaille, ShiftyAxel, and Arthur Thornton.
Restructed and updated for optimum compatibility with modular Enyo 2.7 by Jason Robitaille.