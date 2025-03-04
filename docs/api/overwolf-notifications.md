---
id: overwolf-notifications
title: overwolf.notifications API
sidebar_label: overwolf.notifications
---

Use this API to send toast notifications from your OW app.

Toast are interactive OS notifications that lets you create flexible "reminders" with text, images, and buttons/inputs.
Toast notifications are a combination of some data properties like header, visual (hero image, inline image, app logo) and the toast content.

**Note that currently using toast on an unpacked app does NOT work.**

## Methods Reference

* [overwolf.showToastNotification()](#showtoastnotificationargs-callback)

## Events Reference

* [overwolf.onToastInteraction](#ontoastinteraction)

## Types Reference

* [overwolf.ToastNotificationParams](#toastnotificationparams-object) Object
* [overwolf.ShowToastNotificationResult](#showtoastnotificationresult-object) Object
* [overwolf.LogoOverride](#logooverride-object) Object
* [overwolf.ToastNotificationButton](#toastnotificationbutton-object) Object
* [overwolf.ToastNotificationEvent](#toastnotificationevent-object) Object
* [overwolf.enums.AppLogoCrop](#applogocrop-enum) enum
* [overwolf.enums.ToastEventType](#toatseventtype-enum) enum
* [overwolf.enums.ToastEventError](#toasteventerror-enum) enum


## showToastNotification(args, callback)
#### Version added: 0.176

> Show Windows toast notification.

Parameter            | Type                                                                                  | Description                |
-------------------- | ------------------------------------------------------------------------------------- | -------------------------- |
args                 | [ToastNotificationParams](#toastnotificationparams-object) object                     | Toast notification params  |
callback             | [(Result: ShowToastNotificationResult)](#showtoastnotificationresult-object) => void  | Returns with the result    |

#### Usage example

```js
overwolf.notifications.showToastNotification({
  header: "Header",
  texts: ["text1", "text2", "text3"],
  logoOverride: {
    url: "overwolf-extension://cchhcaiapeikjbdbpfplgmpobbcdkdaphclbmkbj/84x84.png",
    cropType: overwolf.notifications.enums.AppLogoCrop.Default
  },
  heroImage: "overwolf-extension://cchhcaiapeikjbdbpfplgmpobbcdkdaphclbmkbj/logo_364x180.png",
  inlineImage: "overwolf-extension://cchhcaiapeikjbdbpfplgmpobbcdkdaphclbmkbj/logo_364x180.png",
  attribution: "sent from an app",
  buttons: [
    {
      id: "1",
      text: "button 1"
    },
    {
      id: "2",
      text: "button 2"
    }
  ]
}, console.log);
```

The above line of code triggers this notification:

![notification-example](../assets/notifications/notification-example.png)

## ToastNotificationParams Object

Parameter          | Type     | Description                                 |
-------------------| ---------| ------------------------------------------- |
header             | string   | Mandatory                                   |   
texts              | string[] | Mandatory. See [notes](#texts-notes).       |   
logoOverride       | [LogoOverride](#logooverride-object) object |  See [notes](#logooverride-notes).  |   
heroImage          | string   | See [notes](#heroimage-notes).              |   
inlineImage        | string   | See [notes](#inlineimage-notes)             |   
attribution        | string   | See [notes](#attribution-notes)             |   
buttons            | [ToastNotificationButton](#toastnotificationbutton-object)[]  |  See [notes](#buttons-notes).  |   

#### texts notes

Texts parameter must exist and include 1-3 texts (lines).

#### logoOverride notes

By default, your toast will display your app's logo. However, you can override this logo with your own image.

#### heroImage notes

Toasts can display a hero image, which is displayed prominently within the toast banner and while inside Action Center. Image dimensions must be 364x180 pixels.

#### inlineImage notes

You can provide a full-width inline-image that appears when you expand the toast.

#### attribution notes

If you need to reference the source of your content, you can use attribution text. This text is always displayed at the bottom of your notification, along with your app's identity or the notification's timestamp.

#### buttons notes

Buttons make your toast interactive, letting the user take quick actions on your toast notification without interrupting their current workflow. Buttons appear in the expanded portion of your notification.

## ToastNotificationButton Object

Parameter          | Type     | Description                                 |
-------------------| ---------| ------------------------------------------- |
id                 | string   |                                             |   
text               | string   |                                             |   

## LogoOverride Object

Note: Must be 364x180.

Parameter          | Type     | Description                                 |
-------------------| ---------| ------------------------------------------- |
url                | string   |                                             |   
cropType           | [AppLogoCrop](#applogocrop-enum) enum |                |   

## ShowToastNotificationResult Object

Parameter          | Type     | Description                                 |
-------------------| ---------| ------------------------------------------- |
*success*          | boolean  | inherited from the "Result" Object          |
*error*            | string   | inherited from the "Result" Object          |
id                 | string   |                                             |   

## AppLogoCrop enum
#### Version added: 0.176

> Specify the desired cropping of the image.

Option    | Value        | Description                                            |
--------- | -------------|------------------------------------------------------- |
Default   |  "Default"   | Cropping uses the default behavior of the renderer.    |
None      |  "None"      | Image is not cropped.                                  |
Circle    |  "Circle"    | Image is cropped to a circle shape.                    |

## onToastInteraction
#### Version added: 0.176

> Fired when a user tapped on the body of a toast notification or performed an action inside a toast notification, with the following structure: [ToastNotificationEvent](#toastnotificationevent-object) Object.

## ToastNotificationEvent  Object

Parameter          | Type     | Description                                 |
-------------------| ---------| ------------------------------------------- |
id                 | string   |                                             |   
eventType          | [ToastEventType](#toasteventtype-enum) enum  |         |   
buttonID           | string   |                                             |   
error              | string   |                                             |   
errorCode          | [ToastEventError](#toasteventerror-enum) enum          |   

## ToatsEventType enum
#### Version added: 0.176

> Describes the toast event type.  

Option       | Value            | Description                                            |
------------ | -----------------|------------------------------------------------------- |
Dismiss      |  "dismiss"       |                                                        |
ButtonClick  |  "buttonClick"   |                                                        |
Error        |  "error"         |                                                        |

## ToastEventError enum
#### Version added: 0.176

> Describes the toast event error.

Option                  | Value            | Description                                            |
---------------------- | -----------------|------------------------------------------------------- |
Unknown                |  "unknown"       |                                                        |
NotificationsDisabled  |  "notificationsDisabled "   |                                                        |
Error                  |  "error"         |                                                        |
