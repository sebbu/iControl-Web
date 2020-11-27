# iControl-Web Manual Pages (Version 1.7)

This github project is a manual for the "iControl Web" iOS app. The app is available for download at the Apple App Store: [https://itunes.apple.com/us/app/icontrol-web/id580659303?mt=8#](https://itunes.apple.com/us/app/icontrol-web/id580659303?mt=8# "iControl Web").

The app iControl Web can be used for any home automation system which supports http. The app can also be used to use IFTTT from Apple watch. You can call any URL with an appropriate sized button on any of your devices (iPhone, iPad or Apple Watch). The screens can be configured according to your needs using a json file via iTunes file sharing.

---

## Motivation

I am an individual developer of the app iControl Web. My intention for the app was my own need of a non-ugly app where I can send http requests to my Raspberry Pi home automation system (I have also implemented a flexible server [Raspberry PI GPIO Web Control Interface](https://bitbucket.org/sbub/raspberry-pi-gpio-web-control)). Over the years I have added more features by user requests. The app is ad free and tracker free as I would not trust any app which has control of my home that integrates these things.

---

# Configuration

In order to configure the app you need to create a configuration JSON file. You can always download a full  example for a starting point using iTunes file sharing or in this repository as [guiSample.json](guiSample.json).

## JSON File via Email or any File based Cloud App

In order to configure the app you can send a configuration JSON file via email attachement and open it on the iPhone. It must have the file ending .json! You have to use the open dialog and choose iControl Web to open it and then follow the instructions **Install New Configuration**. You can also use any other (file based cloud) app, i.e. iCloud, Dropbox, to send/export the file iControl Web. A copy of the previous config is saved under gui.json.[date]. These files can only be accessed via iTunes file sharing.


## iTunes File Sharing (it has moved to the Finder)


I know that it is really a bit of a hassle, but I think it is much easier to fiddle about these cryptic home automation URLs on a computer with a real keyboard instead of an iPhone keyboard. Tool support (JSON editors) is great and you probably do not change your configuration every day. If you can think of any other good possibility to create the UI including the URLs, please let me know.

## Instructions

* Install the app on your device.
* Connect the device to iTunes.
* Find the screen file sharing folder of iControl Web. ![alt iTunes file sharing](images/iTunes-file-sharing.png "iTunes file sharing")
* Drag and drop **guiSample.json** to your computer.
* Copy the file guiSample.json to the name gui.json.
* Edit the file gui.json with a JSON editor to avoid syntax errors. There are plenty in the Mac App Store or available online.
* After you have changed the file to your needs, drag and drop it back (named **gui.json**) to the iTunes file sharing folder in iTunes.

# Install New Configuration

After you have copied a new version of gui.json to your device, please follow these instructions:

* Kill the iPhone app using the app switcher and start it again. You will see the configuration on your iPhone or iPad.
* The configuration should be transferred automatically to the Apple watch.
    * The iPhone shows an alert that it will transfer the configuration to the watch.
    * Enable the watch screen once to finish the transfer.
    * The iPhone app will show an info alert about the successful transfer (alerts might be messed up a little).
* If you do not see an info that the transfer did start, you can swipe to the right most screen. It is an info screen where you can force the transfer of the watch configuration again (if you do not see the info screen on your iPhone you have to enable it with `"showInfoScreen": true`).
* If you still do not see an info that the transfer will start, make sure that the app is installed on your watch and that the watch is linked. Open the iControl watch app and stop it and start it again.
* In rare case this is a little bit of a hassle and might need a restart of both devices and/or a new install.


# JSON Configuration in Detail

It is highly recommended to use a JSON editor. If you do not want to use it for editing, you should use it for a syntax check. The app will silently fail on syntax errrors or might even crash. 

## General Structure

The following example shows the general structure of the JSON file. It consists of two parts:

1. `"pages"` is for the **iPhone/iPad** configuration. Screens are reachable by swiping horizontally. The last screen is an info screen. 
	* The info screen can be hidden by settings `"showInfoScreen": false`. This should only be done after you have done all other configuration and when you are sure that everything is working. 
	* You can disable network response feedback in two ways: use the switch on the info screen to hide error alerts. Set `"coloredNetworkFeedback": false` to not see flashing green/red button color. This is useful if your server does not respond correctly on network requests.
	* If your device supports 3D Touch, you can configure `"3D_Touch"` with up to four commands.
	* You can configure the `pages` in an array:
    	* Each page has a `"pageLabel"` which is optional.
		* You can specify a request `"timeout"` (in seconds). The default value is 2.0 seconds.
		* You can modify the vertical spacing of the controls with `"compactHeight"` flag in order to fit more buttons on one screen.
		* Furthermore a page/screen has an array of `"controls"` (see details about it below).

2. `"pagesWatch"` is for the **Apple watch**. Screens are reachable via table drill down (as it is commonly used in other apps, i.e. in Apple settings app).
	* The screen has a `"pageLabel"` which is displayed in the first line.
	* On the watch you can only specify one request `"timeout"` for all controls.
	* You can configure if you want haptic (and sound) feedback whenever you press a button with the flag `"hapticNetworkResponseFeedback"`.
	* If you want to use the glance for iControl Web, you may specify a request which can be more seen as a sensor with `"glanceTextUrl"`.
	* If the (glance) sensor request fails, the `"glanceErrorText"` is displayed. If you want to use the glance, but you do **not** have an appropriate sensor, simply write something *wrong* to `"glanceTextUrl"`, i.e. "glanceTextUrl": "xxxhttp://www.irtp.de/test.txt". No network request is send out and the `"glanceErrorText"` is displayed directly. The glance can still be used as a shortcut to open the app.
	* Although the `"pagesWatch"` object is an array, there is only one screen on the watch (and the array must consist of one object), but a page can have subpages (see below).


General Structure of the JSON file:
    
    {
      "showInfoScreen": true,
      "coloredNetworkFeedback": true,
      "3D_Touch": {
        "contextMenuLabel1": "All Off",
        "contextMenuIcon1": "Prohibit",
        "contextMenuCmd1": "http://cmd1ContextAllOff",
      },
      "pages": [
        {
          "pageLabel": "General",
          "timeout": 2,
          "compactHeight": false,
          "controls": [
            {
              "_comment": "This is the first screen."
            }
          ]
        }
      ],
      "pagesWatch": [
        {
          "pageLabel": "iControl",
          "timeout": 2,
          "hapticNetworkResponseFeedback": true,
          "glanceTextUrl": "http://www.irtp.de/test.txt",
          "glanceErrorText": "Status not available",
          "controls": [
            {
              "contextMenuLabel1": "All Off",
              "contextMenuIcon1": "Decline",
              "contextMenuCmd1": "http://cmd1MContextAllOff"
            },
            {
              "_comment": "This is the first screen on watch."
            },
            {
              "subPageLabel": "Garage",
              "controls": [
                {
                  "_comment": "This is a sub screen (table drilldown)."
                }
              ]
            }
          ]
        }
      ]
    }




## iPhone/iPad Controls

For the iPhone/iPad there are 5 `"sizeType"` of button sizes to choose from:

* **large:** is one button in a row
* **medium:** are two buttons in a row
* **small:** are two pairs of buttons in a row (in sum: four)
* **verySmall:** are six buttons in a row (i.e. for dimmer)
* **smallVertical:** are two vertical pairs of buttons in a row (in sum: four)

For every `"sizeType"` of buttons you need to specify the exact number of headlines, button labels and commands. Just have a look how the app looks with the guiSample.json file and you will understand it. Use the blocks from guiSample.json as a template to create your controls.

You may specify an id if you need access from other apps (see section *Interaction with other Apps*). 

Here is an example for the medium size button:

    {
      "sizeType": "medium",
      "headline1": "Light",
      "button1": "on",
      "cmd1": "http://cmd1M",
      "cmd1Id": "uniqueCmdId1M",
      "button2": "off",
      "cmd2": "http://cmd2M",
      "cmd2Id": "uniqueCmdId2M"
    }



## Apple Watch Controls


For the Apple watch there are 2 `"sizeType"` of button sizes, a `"label"` and a **sub page** to choose from. The buttons must be of `"sizeType"`:

* **large:** is one button in a row
* **medium:** are two buttons in a row

For every `"sizeType"` of buttons you need to specify the exact number of button labels and commands. As screen size is limited there is no headline by default, but you can specify a `"label"` for instead.  Large buttons, labels and sub pages can have a colored dot in front: `"color"` (yellow, orange, purple, green, cyan, blue, clear, none) where `"none"` is like no color and `"clear"` is also no color but the button is indented.

Here is an example for the medium size button (it does not take a color):

    {
      "sizeType": "medium",
      "button1": "on",
      "cmd1": "http://cmd1Mon",
      "button2": "off ️",
      "cmd2": "http://cmd2Moff"
    }

For a **sub page** you have to specify a `"subPageLabel"` and you can specify a `"color"`. The label and color are also used on the sub page as headline which is underlined with the color. It also has a `"controls"` array for buttons, labels and sub pages.

Here is an example for a sub page:

    {
      "subPageLabel": "Other  ",
      "color": "green",
      "controls": [
        {
          "sizeType": "large",
          "button1": "on",
          "cmd1": "http://cmd1Mon",
          "color": "orange"
        }
      ]
    }


On every page and subpage, it is possible to define up to four context menu items individually. It is reached with a force touch. Possible use cases are

* controls which have an impact (i.e. a garage door should not be opened/closed by accident)
* scene controls which have a meaning for all controls (i.e. turn all lights off for this room)


A context menu entry must have a label, a command and an icon. The names of the icons are predefined by Apple as [WKMenuItemIcon](https://developer.apple.com/documentation/watchkit/wkmenuitemicon). Simply use the short strings (i.e. Accept, Play, Pause, More, ...).

You must specify at most one context menu part for a each page. Here is an example with 2 buttons:

    {
      "contextMenuLabel1": "All Off",
      "contextMenuIcon1": "Decline",
      "contextMenuCmd1": "http://cmd1MContextAllOff",
      "contextMenuLabel2": "All On",
      "contextMenuIcon2": "Accept",
      "contextMenuCmd2": "http://cmd2MContextAllOn"
    }




## Interaction with other Apps

For an iPhone/iPad app it is possible to provide a custom URL scheme. This URL can be used for interactions between apps. The URLs look like normal URLs, i.e. to see your twitter timeline use the following URL: `twitter://timeline`. There even exists list of iPhone URL schemes on the internet. You can search for them to see which of your installed Apps support a custom URL scheme and can be integrated to iControl Web.

The iControl Web app currently supports a custom URL scheme to give external access to a button. You only have to specify an id for the button (on the iPhone). The id must be unique for all buttons on all pages!

    {
      "sizeType": "lage",
      "headline1": "Light",
      "button1": "on",
      "cmd1": "http://cmd1L",
      "cmd1Id": "uniqueCmdId1L",
    }

To reference this button from another app (i.e. Workflow), simply use the following link: 

`icontrol://execute?uniqueCmdId1L`.

Apple has restricted the usage of custom URL schemes with iOS9 (especially the [testing](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIApplication_Class/index.html#//apple_ref/occ/instm/UIApplication/canOpenURL:)). iControl Web does not test if the other app exists and if it can handle the url scheme. This way I do not have to whitelist earch URL scheme and the app should work with any custom url scheme.

If you want to call iControl Web from another app, you probably have to talk to the developer to whitelist this app.

