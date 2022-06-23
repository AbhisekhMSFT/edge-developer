---
title: Overview of WebView2 APIs and their capabilities
description: Overview of WebView2 APIs and their capabilities.
author: MSEdgeTeam
ms.author: msedgedevrel
ms.topic: conceptual
ms.prod: microsoft-edge
ms.technology: webview
ms.date: 06/20/2022
---
# Overview of WebView2 APIs and their capabilities

This article provides:
*  A high-level understanding of the capabilities of the WebView2 technology.
*  Concepts about what you can do with the WebView2 APIs.
*  High-level description of WebView2 technology for app developers.
*  Better fundamentals + understanding of WebView2 technology.

Audience: App developers who are curious about the WebView2 technology and would like to know if WebView2 is for them.


**Contents:**
<!-- page outline as of June 20 EOD: -->

* [Summary of APIs](#summary-of-apis)

* [High-level interfaces spanning multiple areas](#high-level-interfaces-spanning-multiple-areas)
   * [Overview of the top-level classes](#overview-of-the-top-level-classes)
      * [UI framework-specific WebView2 element class such as WPF, WinForms, or WinUI WebView2 classes](#ui-framework-specific-webview2-element-class-such-as-wpf-winforms-or-winui-webview2-classes)
   
* [Web/native interop](#webnative-interop)
   * [Host/Web Object Sharing](#hostweb-object-sharing)
   * [Script Execution](#script-execution)
   * [Web Messaging](#web-messaging)
   * [Script Dialogs](#script-dialogs)
   
* [Browser features](#browser-features)
   * [Printing](#printing)
   * [Cookies](#cookies)
   * [Image capture](#image-capture)
   * [Downloads](#downloads)
   * [Permissions](#permissions)
   * [Context menus](#context-menus)
   * [Status Bar](#status-bar)
   * [User Agent](#user-agent)
   * [Autofill](#autofill)
   * [Audio](#audio)
   * [Swipe gesture navigation](#swipe-gesture-navigation)
   
* [Managing WebView2 processes](#managing-webview2-processes)

* [Load and keep track of content (web/local/remote)](#load-and-keep-track-of-content-weblocalremote)
   * [Manage page navigation](#manage-page-navigation)
   * [DOM/Content loaded](#domcontent-loaded)

* [Web content](#web-content)
   * [Web Resource Request](#web-resource-request)
   * [Web Resource Response](#web-resource-response)
   * [Server/Client Certificates](#serverclient-certificates)

* [Working with Chrome Developer Protocol (CDP)](#working-with-chrome-developer-protocol-cdp)

* [iFrames](#iframes)

* [Authentication](#authentication)

* [Environment setup](#environment-setup)

* [Rendering WebView2 in non-framework apps](#rendering-webview2-in-non-framework-apps)

* [Window Management](#window-management)
   * [Sizing, positioning, and visibility](#sizing-positioning-and-visibility)
   * [Zooming](#zooming)
   * [Rasterization Scale](#rasterization-scale)
   * [Focus & Tabbing](#focus--tabbing)
   * [Parent Window](#parent-window)
   * [Keyboard Accelerators](#keyboard-accelerators)
   * [Default background color](#default-background-color)
   
* [Composition-based rendering](#composition-based-rendering)
   * [Output](#output)
   * [Input](#input)
   * [Accessibility](#accessibility)
   
* [User data](#user-data)

* [Performance optimizations/tools and debugging](#performance-optimizationstools-and-debugging)

* [Misc.](#misc)


## Summary of APIs

| API | Purpose |
|---|---|
| [High-level interfaces spanning multiple areas](#high-level-interfaces-spanning-multiple-areas) | The top-level CoreWebView2, Controller, and Environment classes work together so your app can host a WebView2 browser control. |
| [Web/native interop](#webnative-interop) | Embed web content into native applications.  Communicate between native code and web code using simple messages, JavaScript code, and native objects. |
| [Browser features](#browser-features) | Toggle and change these inherited features that are inherited from the browser and are available in a WebView2 control. |
| [Managing WebView2 processes](#managing-webview2-processes) | TBD |
| [Load and keep track of content (web/local/remote)](#load-and-keep-track-of-content-weblocalremote) | TBD |
| [Web content](#web-content) | TBD |
| [Working with Chrome Developer Protocol (CDP)](#working-with-chrome-developer-protocol-cdp) | Instrument, inspect, debug, and profile Chromium-based browsers.  The Chrome DevTools Protocol is the foundation for the Microsoft Edge DevTools.  Use the Chrome DevTools Protocol for features that aren't implemented in the WebView2 platform. |
| [iFrames](#iframes) | Embed other webpages into your own webpage.  Detect when embedded webpages are created, detect when embedded webpages are navigating, and optionally bypass x-frame options. |
| [Authentication](#authentication) | Handle basic authentication in WebView2 controls. |
| [Environment setup](#environment-setup) | Specify settings before WebView2 controls are created. |
| [Rendering WebView2 in non-framework apps](#rendering-webview2-in-non-framework-apps) | Set up the WebView2 rendering system in non-framework apps, such as how the WebView2 control renders output into your host app, and how WebView2 handles input, focus, and accessibility. |
| [Window management](#window-management) | Tasks related to the management of a window. |
| [Input](#input) | The Input API handles keyboard events, so that your app has keyboard support.  Read mouse input on a WebView2 control.  Support touch input, such as pinch zoom, in a WebView2 control. |
| [User data](#user-data) | Manage the user data folder (UDF), which is a folder stored on the user's machine, that contains data related to the host app and WebView2.  WebView2 apps use user data folders to store browser data, such as cookies, permissions, and cached resources. |
| [Performance optimizations/tools and debugging](#performance-optimizationstools-and-debugging) | Analyze and debug performance and handle performance-related events for WebView2 controls.  Make your app respond to user input actions quickly and prevent sluggish response.  Manage memory usage and responsiveness. |
| [Misc.](#misc) | Read settings and handle deferral.<!--define/hints--> |


<!-- ====================================================================== -->
## High-level interfaces spanning multiple areas

This article categorizes the WebView2 interfaces, at a high level, for WebView2.  This article also lists selected members of interfaces.  These interfaces contain properties, methods, and events that span multiple categories in this article:

#### [C#](#tab/c-sharp)

* [CoreWebView2 Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2)
* [CoreWebView2Controller Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller)
* [CoreWebView2Environment Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment)
* [CoreWebView2Settings Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings)

#### [C++](#tab/cpp)

* [ICoreWebView2](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2)
* [ICoreWebView2Controller](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller)
* [ICoreWebView2Environment](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment)
* [ICoreWebView2Settings](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings)

---

<!-- link prefix: https://docs.microsoft.com -->

Cross-platform API implementation: Most of the WebView2 APIs are initially developed for C++, and then most of the C++ APIs are wrapped as C# APIs.  This way, there is a consistent parallelism and equivalence across platforms and programming languages.


### Overview of the top-level classes
<!-- change heading to more like:
### How the top-level classes work: CoreWebView2, Controller, and Environment
-->

The top-level CoreWebView2, Controller, and Environment classes work together so your app can host a WebView2 browser control.

<!-- nutshell intro/summary block to keep sync'd:
* topmost content in [How the top-level classes work: CoreWebView2, Controller, and Environment](environment-controller-core.md).
* [Overview of the top-level classes](apis-capabilities.md#overview-of-the-top-level-classes) in _Overview of WebView2 APIs and their capabilities_.
-->

<!-- todo - derive / condense in context of APIs Overview .md - all this info is dup'd in environment-controller-core.md, so freely rework/condense here & then copy to top of there (aim to completely transform/reword/condense this block): -->

Overview of:
* `CoreWebView2Environment`
* `CoreWebView2`
* `CoreWebView2Controller` vs. `WebView2` classes (UI framework-specific WebView2 element class like the WPF, WinForms, or WinUI `WebView2` classes).

or, equivalently:
* `ICoreWebView2Environment`
* `ICoreWebView2`
* `ICoreWebView2Controller`

`CoreWebView2Environment` represents a group of WebView2 controls that all share the following:
*  They share the same WebView2 browser process.
*  They share the same user data folder.
*  They potentially share WebView2 renderer and other WebView2 processes.

From the `CoreWebView2Environment`, you create `CoreWebView2Controller` and `CoreWebView2` pairs.  They always come together as a `CoreWebView2Controller` and a corresponding `CoreWebView2`.
*  The `CoreWebView2Controller` is responsible for all hosting-related functionality such as focus, visibility, size, and input.
*  The `CoreWebView2` is for the web-specific parts of the WebView2 control, including networking, navigation, script, and parsing and rendering HTML.


#### UI framework-specific WebView2 element class such as WPF, WinForms, or WinUI WebView2 classes

It's different if you are using a UI framework-specific WebView2 element class like our WPF, WinForms, or WinUI WebView2 classes. 

Then, the WebView2 class can optionally take a `CoreWebView2Environment` to use, and otherwise it will create a default `CoreWebView2Environment`.  Internally, the WebView2 class creates its `CoreWebView2Controller` and `CoreWebView2` from the `CoreWebView2Environment`.  The `WebView2` exposes its `CoreWebView2` as a `CoreWebView2` property, but the `CoreWebView2Controller` is kept private to the `WebView2` class.  This is because the `WebView2` class is responsible for connecting all of the `CoreWebView2Controller` functionality to the UI framework.

<!-- / end of nutshell intro/summary block to keep sync'd -->

See also:

* [How the top-level classes work: CoreWebView2, Controller, and Environment](environment-controller-core.md)


<!-- ====================================================================== -->
## Web/native interop

The Microsoft Edge WebView2 control lets you embed web content into native applications. You can communicate between native code and web code using simple messages, JavaScript code, and native objects. 

Some common use cases include:
*  Update the native host window title after navigating to a different website.
*  Send a native camera object and use its methods from a web app.
*  Run a dedicated JavaScript file on the web side of an application.

For more information, see:
* [Interop of native-side and web-side code](../how-to/communicate-btwn-web-native.md)
* [Call web-side code from native-side code](../how-to/javascript.md)
* [Call native-side code from web-side code](../how-to/hostobject.md)

The following are the main APIs for communicating between web and native code.


<!-- ------------------------------ -->
### Host/Web Object Sharing

<!-- define object sharing.  selected methods on a host platform can be called by web-side by sharing specified objects. -->
WebView2 enables objects defined in native code to be passed to the web. These are called host objects, which can be projected into JavaScript so that you can trigger the native object methods from web side code, or as a result of user interaction on the web-side of your app. By doing so, you can avoid re-implementing your native objects' methods in your web-side code. 

#### [C#](#tab/c-sharp)

* [CoreWebView2.AddHostObjectToScript Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.addhostobjecttoscript)
* [CoreWebView2.RemoveHostObjectFromScript Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.removehostobjectfromscript)
* [CoreWebView2Settings.AreHostObjectsAllowed Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.arehostobjectsallowed)
* [CoreWebView2HostResourceAccessKind Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2hostresourceaccesskind)<!--todo: which of the above methods or properties use this enum? it's used by https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.setvirtualhostnametofoldermapping   Add enum to c++ list?-->


#### [C++](#tab/cpp)

* [ICoreWebView2::AddHostObjectToScript method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#addhostobjecttoscript)
* [ICoreWebView2::RemoveHostObjectFromScript method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#removehostobjectfromscript)
* [ICoreWebView2Settings::AreHostObjectsAllowed property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_arehostobjectsallowed), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_arehostobjectsallowed)

---


<!-- ------------------------------ -->
### Script Execution

<!-- what kind of script, any JavaScript statements/objects/methods?  Where is the script executed?  what's this heading referring to? -->
You can use JavaScript in WebView2 controls to customize your native apps to meet your requriements.
<!--
See also:
* []()
-->

#### [C#](#tab/c-sharp)

* [CoreWebView2.AddScriptToExecuteOnDocumentCreatedAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.addscripttoexecuteondocumentcreatedasync)
* [CoreWebView2.ExecuteScriptAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.executescriptasync)
* [CoreWebView2.RemoveScriptToExecuteOnDocumentCreated Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.removescripttoexecuteondocumentcreated)
* [CoreWebView2Settings.IsScriptEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.isscriptenabled)

#### [C++](#tab/cpp)

* [ICoreWebView2::AddScriptToExecuteOnDocumentCreated method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#addscripttoexecuteondocumentcreated)
* [ICoreWebView2::ExecuteScript method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#executescript)
* [ICoreWebView2::RemoveScriptToExecuteOnDocumentCreated method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#removescripttoexecuteondocumentcreated)
* [ICoreWebView2Settings::IsScriptEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_isscriptenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_isscriptenabled)


---

<!--
Clickable API Ref links:
* [CoreWebView2 Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2)
* [ICoreWebView2](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2)
-->


<!-- ------------------------------ -->
### Web Messaging

<!-- what is "web messaging"?  sending messages from wv2 control to http server, then handling the response from the server.  the host app uses the wv2 ctrl to send msgs to http server.  why is messaging needed for a hybrid/web app?  what's involved, in summary? -->
You can send messages from the host app to WebView2 control and from WebView2 control to host app. 

<!--
See also:
* []()
-->

#### [C#](#tab/c-sharp)

* [CoreWebView2.PostWebMessageAsJson Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.postwebmessageasjson)
* [CoreWebView2.PostWebMessageAsString Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.postwebmessageasstring)
* [CoreWebView2.WebMessageReceived Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.webmessagereceived)
   * [CoreWebView2WebMessageReceivedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2webmessagereceivedeventargs)
* [CoreWebView2Settings.IsWebMessageEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.iswebmessageenabled)

#### [C++](#tab/cpp)

* [ICoreWebView2::PostWebMessageAsJson method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#postwebmessageasjson)
* [ICoreWebView2::PostWebMessageAsString method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#postwebmessageasstring)
* [ICoreWebView2::WebMessageReceived event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_webmessagereceived), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_webmessagereceived)
   * [ICoreWebView2WebMessageReceivedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2webmessagereceivedeventargs)
* [ICoreWebView2Settings::IsWebMessageEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_iswebmessageenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_iswebmessageenabled)


---


<!-- ------------------------------ -->
### Script Dialogs

You can use this to manage different JavaScript dialogs to suppress or replace them with custom dialogs.

For more information, see:
* Script dialogs<!--TODO: link avail?-->

#### [C#](#tab/c-sharp)

* [CoreWebView2.ScriptDialogOpening Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.scriptdialogopening)
   * [CoreWebView2ScriptDialogOpeningEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2scriptdialogopeningeventargs)
* [CoreWebView2ScriptDialogKind Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2scriptdialogkind)

#### [C++](#tab/cpp)

* [ICoreWebView2::ScriptDialogOpening event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_scriptdialogopening), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_scriptdialogopening)
   * [ICoreWebView2ScriptDialogOpeningEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2scriptdialogopeningeventargs)
* [COREWEBVIEW2_SCRIPT_DIALOG_KIND enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_script_dialog_kind)


---


<!-- ====================================================================== -->
## Browser features

This section covers features inherited from the browser and available in a WebView2 control.  These APIs allow developers the ability to toggle and change these inherited features.


<!-- ------------------------------ -->
### Printing

You can configure custom print settings and print to PDF.  

<!--
See also:
* []()
-->

#### [C#](#tab/c-sharp)

* [CoreWebView2.PrintToPdfAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.printtopdfasync)
* [CoreWebView2Environment.CreatePrintSettings Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createprintsettings)
* [CoreWebView2PrintSettings Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2printsettings)
   * [CoreWebView2PrintOrientation Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2printorientation)

#### [C++](#tab/cpp)

* [ICoreWebView2_7::PrintToPdf method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_7#printtopdf)
* [ICoreWebView2Environment6::CreatePrintSettings method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment6#createprintsettings)
* [ICoreWebView2PrintSettings interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2printsettings)
   * [COREWEBVIEW2_PRINT_ORIENTATION enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_print_orientation)

---


<!-- ------------------------------ -->
### Cookies

You can use cookies in WebView2 to manage user sessions, store user personalization preferences, and track user behavior.
<!-- what's cookies, what are they used for, what should your app do with them.  cookies are used for local storage of app state such as login, queries, ... to use cookies, your wv2 host app -->

See also:
* [View, edit, and delete cookies](/microsoft-edge/devtools-guide-chromium/storage/cookies)
<!-- no WebView2 page with "cookie" in title -->

#### [C#](#tab/c-sharp)

* [CoreWebView2Cookie Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2cookie)
* [CoreWebView2CookieList Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2cookielist)
* [CoreWebView2.CookieManager Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.cookiemanager#microsoft-web-webview2-core-corewebview2-cookiemanager)
   * [CoreWebView2CookieManager Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2cookiemanager)
* [CoreWebView2CookieSameSiteKind Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2cookiesamesitekind)

#### [C++](#tab/cpp)

* [ICoreWebView2Cookie interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2cookie)
* [ICoreWebView2CookieList interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2cookielist)
* [ICoreWebView2_2.CookieManager property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_2#get_cookiemanager)
   * [ICoreWebView2CookieManager interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2cookiemanager)
* [COREWEBVIEW2_COOKIE_SAME_SITE_KIND enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_cookie_same_site_kind)

---


<!-- ------------------------------ -->
### Image capture

In WebView2 you can capture screenshots and indicate the format to save the image as.
<!-- benefit?  why do this?  
This way, when your wv2 app is running and the user presses Alt+PrtScn, XYZ will ... -->
<!-- Capturing images using the key press combination `Ctrl`+`Shift`+`S` (TODO: confirm)  state macOS key combination too -->

<!--
See also:
* []()
-->

#### [C#](#tab/c-sharp)

* [CoreWebView2.CapturePreviewAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.capturepreviewasync)
* [CoreWebView2CapturePreviewImageFormat Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2capturepreviewimageformat)

#### [C++](#tab/cpp)

* [ICoreWebView2.CapturePreview method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#capturepreview)
* [COREWEBVIEW2_CAPTURE_PREVIEW_IMAGE_FORMAT enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_capture_preview_image_format)

---


<!-- ------------------------------ -->
### Downloads

You can manage the download experience in WebView2 by allowing/blocking downloads based on different metadata and changing the download location. You can also configure a custom download UI or customize the default UI. 

<!--
See also:
* []()
-->


#### [C#](#tab/c-sharp)

General:
* [CoreWebView2.IsDefaultDownloadDialogOpenChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.isdefaultdownloaddialogopenchanged)
* [CoreWebView2.IsDefaultDownloadDialogOpen Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.isdefaultdownloaddialogopen)
* [CoreWebView2.OpenDefaultDownloadDialog Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.opendefaultdownloaddialog)
* [CoreWebView2.CloseDefaultDownloadDialog Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.closedefaultdownloaddialog)

Modify Default Experience:
* [CoreWebView2.DefaultDownloadDialogCornerAlignment Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.defaultdownloaddialogcorneralignment)
   * [CoreWebView2DefaultDownloadDialogCornerAlignment Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2defaultdownloaddialogcorneralignment)
* [CoreWebView2.DefaultDownloadDialogMargin Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.defaultdownloaddialogmargin)

Custom Download Experience:
* [CoreWebView2.DownloadStarting Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.downloadstarting)
   * [CoreWebView2DownloadStartingEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2downloadstartingeventargs)
* [CoreWebView2DownloadOperation Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2downloadoperation)
* [CoreWebView2DownloadInterruptReason Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2downloadinterruptreason)<!--todo: which of the above methods or properties use this enum?-->
* [CoreWebView2DownloadState Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2downloadstate)
* [CoreWebView2HostResourceAccessKind Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2hostresourceaccesskind)

#### [C++](#tab/cpp)

General:
* [ICoreWebView2_9::IsDefaultDownloadDialogOpen property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#get_isdefaultdownloaddialogopen)<!--no put-->
* [ICoreWebView2_9::OpenDefaultDownloadDialog method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#opendefaultdownloaddialog)
* [ICoreWebView2_9::IsDefaultDownloadDialogOpenChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#add_isdefaultdownloaddialogopenchanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#remove_isdefaultdownloaddialogopenchanged)
* [ICoreWebView2_9::CloseDefaultDownloadDialog method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#closedefaultdownloaddialog)

Modify Default Experience:
* [ICoreWebView2_9::DefaultDownloadDialogCornerAlignment property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#get_defaultdownloaddialogcorneralignment), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#put_defaultdownloaddialogcorneralignment)
   * [COREWEBVIEW2_DEFAULT_DOWNLOAD_DIALOG_CORNER_ALIGNMENT enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_default_download_dialog_corner_alignment)
* [ICoreWebView2_9::DefaultDownloadDialogMargin property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#get_defaultdownloaddialogmargin), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_9#put_defaultdownloaddialogmargin)

Custom Download Experience:
* [ICoreWebView2_4::DownloadStarting event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_4#add_downloadstarting), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_4#remove_downloadstarting)
   * [ICoreWebView2DownloadStartingEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2downloadstartingeventargs)
* [ICoreWebView2DownloadOperation interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2downloadoperation)
* [COREWEBVIEW2_DOWNLOAD_INTERRUPT_REASON enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_download_interrupt_reason)<!--todo: which of the above methods or properties use this enum?-->
* [COREWEBVIEW2_DOWNLOAD_STATE enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_download_state)<!--todo: which of the above methods or properties use this enum?-->
* [COREWEBVIEW2_HOST_RESOURCE_ACCESS_KIND enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_host_resource_access_kind)<!--todo: which of the above methods or properties use this enum?-->

---


<!-- ------------------------------ -->
### Permissions

<!-- TODO: does WV2 ever show the dialogs? What can you do with the dialogs? -->
Different web pages may ask you for permissions to access some privileged resources (e.g. dialog to grant permission to access your location). In WebView2 you can find out what the web pages are asking permission for and what to do with the JavaScript dialogs.  

<!--
See also:
* []()
-->

#### [C#](#tab/c-sharp)

* [CoreWebView2.PermissionRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.permissionrequested)
   * [CoreWebView2PermissionRequestedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2permissionrequestedeventargs)
* [CoreWebView2PermissionKind Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2permissionkind)
* [CoreWebView2PermissionState Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2permissionstate)

#### [C++](#tab/cpp)

* [ICoreWebView2::PermissionRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_permissionrequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_permissionrequested)
   * [ICoreWebView2PermissionRequestedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2permissionrequestedeventargs)
* [COREWEBVIEW2_PERMISSION_KIND enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_permission_kind)
* [COREWEBVIEW2_PERMISSION_STATE enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_permission_state)

---


<!-- ------------------------------ -->
### Context menus

The WebView2 control provides a default context menu, and you can create your own context menu when using a WebView2 control.  Use the **ContextMenuRequested** API to customize the context-menus (right-click menus) of a WebView2 app.  For example, you can do any of the following:

*  Add a custom context menu.

   Instead of using the default context menu, your host app can draw its own context menu by using the information that's sent from the WebView2 context menu.  Your app handles the `ContextMenuRequested` event.  You can use the data provided in the Event arguments of `ContextMenuRequested` to display a custom context menu with entries of your choice.  For this case, you handle the event and request a deferral.

   You can add default menu items and/or custom menu items to a custom context menu.

*  Add default menu items to a custom context menu.
*  Add custom menu items to a default context menu.
*  Remove default or custom menu items from the default context menu.
*  Disable context menus.

<!-- get links from Context Menus article or link to there -->
See also:
* [Customize context menus in WebView2](../how-to/context-menus.md)
   * [API Reference overview](../how-to/context-menus.md#api-reference-overview)

#### [C#](#tab/c-sharp)

* [CoreWebView2.ContextMenuRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.contextmenurequested)
* [CoreWebView2.CallDevToolsProtocolMethodForSessionAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.calldevtoolsprotocolmethodforsessionasync)
* [CoreWebView2Settings.AreDefaultContextMenusEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.aredefaultcontextmenusenabled)

#### [C++](#tab/cpp)

* [ICoreWebView2_11::ContextMenuRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_11#add_contextmenurequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_11#remove_contextmenurequested)
* [ICoreWebView2_11::CallDevToolsProtocolMethodForSession method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_11#calldevtoolsprotocolmethodforsession)
* [ICoreWebView2Settings::AreDefaultContextMenusEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_aredefaultcontextmenusenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_aredefaultcontextmenusenabled)

---


<!-- ------------------------------ -->
### Status Bar
<!-- why is this desirable, what kinds of changes for example, does this mean programmatically monitor? -->
A status bar is located in the bottom left of the page and displays the state of the webpage being displayed. In WebView2 you can enable/disable the status bar, get the text in the status bar and find out when the status bar text has changed. 

<!--
See also:
* []()
-->

#### [C#](#tab/c-sharp)

* [CoreWebView2.StatusBarTextChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.statusbartextchanged)
* [CoreWebView2.StatusBarText Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.statusbartext)
* [CoreWebView2Settings.IsStatusBarEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.isstatusbarenabled)

#### [C++](#tab/cpp)

* [ICoreWebView2_12::StatusBarTextChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_12#add_statusbartextchanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_12#remove_statusbartextchanged)
* [ICoreWebView2_12::StatusBarText property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_12#get_statusbartext)
* [ICoreWebView2Settings::IsStatusBarEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_isstatusbarenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_isstatusbarenabled)

---


<!-- ------------------------------ -->
### User Agent
<!-- alt for "Learn how to" -->
<!-- why would you do this, what is benefit for end user, what's involved?  Your app can detect which XYZ and then ABC so that the X is Y. -->
The user agent is a string that represents the identity of the program on behalf of the user e.g. the browser name. In WebView2 you can set the user agent.

<!--
See also:
* []()
-->

#### [C#](#tab/c-sharp)

* [CoreWebView2Settings.UserAgent Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.useragent)

#### [C++](#tab/cpp)

* [ICoreWebView2Settings2::UserAgent property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings2#get_useragent), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings2#put_useragent)

---


<!-- ------------------------------ -->
### Autofill

In WebView2 you can leverage the browser's autofill functionality. You can enable/disable autofill for general information as well as for passwords. These two settings can be controlled independently of one another and allow applications to control the autofill feature.  
<!-- what does "autofill functionality" mean?  how does this benefit end user? -->

<!--
See also:
* []()
-->

#### [C#](#tab/c-sharp)

* [CoreWebView2Settings.IsGeneralAutofillEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.isgeneralautofillenabled)
* [CoreWebView2Settings.IsPasswordAutosaveEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.ispasswordautosaveenabled)

#### [C++](#tab/cpp)

* [ICoreWebView2Settings4::IsGeneralAutofillEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings4#get_isgeneralautofillenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings4#put_isgeneralautofillenabled)
* [ICoreWebView2Settings4::IsPasswordAutosaveEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings4#get_ispasswordautosaveenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings4#put_ispasswordautosaveenabled)

---


<!-- ------------------------------ -->
### Audio

WebView2 has methods to mute and unmute all audio and events to find out when there is audio playing.
<!-- what kind of audio settings?  how does this enhance end-user UX? -->

<!--
See also:
* []()
-->

#### [C#](#tab/c-sharp)

* [CoreWebView2.IsDocumentPlayingAudioChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.isdocumentplayingaudiochanged)
* [CoreWebView2.IsMutedChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.ismutedchanged)
* [CoreWebView2.IsDocumentPlayingAudio Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.isdocumentplayingaudio)
* [CoreWebView2.IsMuted Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.ismuted)

#### [C++](#tab/cpp)

* [ICoreWebView2_8::IsDocumentPlayingAudioChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_8#add_isdocumentplayingaudiochanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_8#remove_isdocumentplayingaudiochanged)
* [ICoreWebView2_8::IsMutedChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_8#add_ismutedchanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_8#remove_ismutedchanged)
* [ICoreWebView2_8::IsDocumentPlayingAudio property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_8#get_isdocumentplayingaudio)
* [ICoreWebView2_8::IsMuted property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_8#get_ismuted), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_8#put_ismuted)

---


<!-- ------------------------------ -->
### Swipe gesture navigation

In WebView2 you can enable/disable swiping gesture navigation on touch input enabled devices. This allows end users to:
*  Swipe left/right (swipe horizontally) to navigate to previous/next page in navigation history.
*  Pull to refresh (swipe vertically) the current page. (This feature is currently disabled by default in the browser, to enable in WebView2, set AdditionalBrowserArguments property with --pull-to-refresh switch).

<!--
See also:
* []()
-->

#### [C#](#tab/c-sharp)

Browser features:<!-- moved from Rendering section -->
* [CoreWebView2Settings.IsSwipeNavigationEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.isswipenavigationenabled)

#### [C++](#tab/cpp)

Browser features:<!-- moved from Rendering section -->
* [ICoreWebView2Settings6::IsSwipeNavigationEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings6#get_isswipenavigationenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings6#put_isswipenavigationenabled)

---

<!-- ====================================================================== -->
## Managing WebView2 processes

TBD

<!-- why, what's the benefit?  what's involved, what kind of considerations & techniques/strategies? -->

<!--
See also:
* []()
-->


#### [C#](#tab/c-sharp)

Info:
* [CoreWebView2.BrowserProcessId Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.browserprocessid)
* [CoreWebView2Environment.GetProcessInfos Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.getprocessinfos)
* [CoreWebView2Environment.ProcessInfosChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.processinfoschanged)
* [CoreWebView2ProcessInfo Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2processinfo)
   * [CoreWebView2ProcessKind Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2processkind)

Exited:
* [CoreWebView2Environment.BrowserProcessExited Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.browserprocessexited)
   * [CoreWebView2BrowserProcessExitedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2browserprocessexitedeventargs)
   * [CoreWebView2BrowserProcessExitKind Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2browserprocessexitkind)

Failed:
* [CoreWebView2.ProcessFailed Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.processfailed)
   * [CoreWebView2ProcessFailedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2processfailedeventargs)
   * [CoreWebView2ProcessFailedKind Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2processfailedkind)
   * [CoreWebView2ProcessFailedReason Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2processfailedreason)

#### [C++](#tab/cpp)

Info:
* [ICoreWebView2::BrowserProcessId property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#get_browserprocessid)<!--no put-->
* [ICoreWebView2Environment8::GetProcessInfos method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment8#getprocessinfos)
* [ICoreWebView2Environment8::ProcessInfosChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment8#add_processinfoschanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment8#remove_processinfoschanged)
* [ICoreWebView2ProcessInfo interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2processinfo)
   * [COREWEBVIEW2_PROCESS_KIND enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_process_kind)
* [ICoreWebView2ProcessInfoCollection interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2processinfocollection)

Exited:
* [ICoreWebView2Environment5::BrowserProcessExited event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment5#add_browserprocessexited), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment5#remove_browserprocessexited)
   * [ICoreWebView2BrowserProcessExitedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2browserprocessexitedeventargs)
   * [COREWEBVIEW2_BROWSER_PROCESS_EXIT_KIND enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_browser_process_exit_kind)

Failed:
* [ICoreWebView2::ProcessFailed event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_processfailed), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_processfailed)
   * [ICoreWebView2ProcessFailedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2processfailedeventargs)
   * [COREWEBVIEW2_PROCESS_FAILED_KIND enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_process_failed_kind)
   * [COREWEBVIEW2_PROCESS_FAILED_REASON enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_process_failed_reason)

---


<!-- ====================================================================== -->
## Load content in WebView2

Use the following APIs to manage the loaded remote and local content and keep track of the load state and history.



#### [C#](#tab/c-sharp)

### Manage content loaded to WebView2
WebView2 provides fundamental APIs to load, stop loading and reload content to WebView2. The content loaded can be:
 -from a URL.
 -a string of HTML.
 -local content via virtual host name to local folder mapping.
 -from a constructed network request.
* [CoreWebView2.Navigate Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.navigate)
* [CoreWebView2.NavigateToString Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.navigatetostring)
* [CoreWebView2.NavigateWithWebResourceRequest Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.navigatewithwebresourcerequest)
* [CoreWebView2.Stop Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.stop)
* [CoreWebView2.Reload Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.reload)
* [CoreWebView2.SetVirtualHostNameToFolderMapping Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.setvirtualhostnametofoldermapping)
* [CoreWebView2.ClearVirtualHostNameToFolderMapping Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.clearvirtualhostnametofoldermapping)

### History API
The history API methods allows back/forward navigation in WebView2 and the events provide information about the changes in history and WebView2's current source. 
* [CoreWebView2.Source Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.source)
* [CoreWebView2.SourceChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.sourcechanged)
   * [CoreWebView2SourceChangedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2sourcechangedeventargs)
* [CoreWebView2.HistoryChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.historychanged)
* [CoreWebView2.CanGoBack Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.cangoback)
   * [CoreWebView2.GoBack Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.goback)
* [CoreWebView2.CanGoForward Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.cangoforward)
   * [CoreWebView2.GoForward Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.goforward)

### Block unwanted navigations
NavigationStarting event allows the app to cancel navigations in WebView2 including for frames.
* [CoreWebView2.NavigationStarting Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.navigationstarting)
   * [CoreWebView2NavigationStartingEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2navigationstartingeventargs)
* [CoreWebView2.FrameNavigationStarting Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.framenavigationstarting)

### Navigation events
With NavigationStarting and the other navigation events, the app can be informed of the state of navigations in WebView2.
* [CoreWebView2.ContentLoading Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.contentloading)
   * [CoreWebView2ContentLoadingEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2contentloadingeventargs)
* [CoreWebView2.DOMContentLoaded Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.domcontentloaded)
   * [CoreWebView2DOMContentLoadedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2domcontentloadedeventargs)
* [CoreWebView2.NavigationCompleted Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.navigationcompleted)
   * [CoreWebView2NavigationCompletedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2navigationcompletedeventargs)
* [CoreWebView2.FrameNavigationCompleted Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.framenavigationcompleted)

See also:
* [Navigation events for WebView2 apps](navigation-events.md)

#### [C++](#tab/cpp)

### Manage content loaded to WebView2
WebView2 provides fundamental APIs to load, stop loading and reload content to WebView2. The content loaded can be:
 -from a URL.
 -a string of HTML.
 -local content via virtual host name to local folder mapping.
 -from a constructed network request.
* [ICoreWebView2::Navigate method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#navigate)
* [ICoreWebView2::NavigateToString method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#navigatetostring)
* [ICoreWebView2_2::NavigateWithWebResourceRequest method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_2#navigatewithwebresourcerequest)
   * [COREWEBVIEW2_WEB_RESOURCE_CONTEXT enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_web_resource_context)
* [ICoreWebView2::Stop method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#stop)
* [ICoreWebView2::Reload method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#reload)
* [ICoreWebView2_3::SetVirtualHostNameToFolderMapping method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_3#setvirtualhostnametofoldermapping)
* [ICoreWebView2_3::ClearVirtualHostNameToFolderMapping method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_3#clearvirtualhostnametofoldermapping)

### History API
The history API methods allows back/forward navigation in WebView2 and the events provide information about the changes in history and WebView2's current source. 
* [ICoreWebView2::Source property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#get_source)<!--no put-->
* [ICoreWebView2::SourceChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_sourcechanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_sourcechanged)
   * [ICoreWebView2SourceChangedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2sourcechangedeventargs)
* [ICoreWebView2::CanGoBack property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#get_cangoback)<!--no put-->
   * [ICoreWebView2::GoBack method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#goback)
* [ICoreWebView2::CanGoForward property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#get_cangoforward)<!--no put-->
   * [ICoreWebView2::GoForward method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#goforward)

### Block unwanted navigations
NavigationStarting event allows the app to cancel navigations in WebView2 including for frames.
* [ICoreWebView2::NavigationStarting event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_navigationstarting), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_navigationstarting)
   * [ICoreWebView2NavigationStartingEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2navigationstartingeventargs)
* [ICoreWebView2::FrameNavigationStarting event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_framenavigationstarting), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_framenavigationstarting)

### Navigation events
With NavigationStarting and the other navigation events, the app can be informed of the state of navigations in WebView2.
* [ICoreWebView2::NavigationCompleted event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_navigationcompleted), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_navigationcompleted)
   * [ICoreWebView2NavigationCompletedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2navigationcompletedeventargs)
* [ICoreWebView2::FrameNavigationCompleted event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_framenavigationcompleted), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_framenavigationcompleted)
* [ICoreWebView2::HistoryChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_historychanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_historychanged)

See also:
* [Navigation events for WebView2 apps](navigation-events.md)

---

#### [C#](#tab/c-sharp)

Title:
* [CoreWebView2.DocumentTitle Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.documenttitle)
* [CoreWebView2.DocumentTitleChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.documenttitlechanged)

Fullscreen:
* [CoreWebView2.ContainsFullScreenElement Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.containsfullscreenelement)
* [CoreWebView2.ContainsFullScreenElementChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.containsfullscreenelementchanged)

New window: <!-- moved from Rendering section -->
* [CoreWebView2.NewWindowRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.newwindowrequested)
   * [CoreWebView2NewWindowRequestedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2newwindowrequestedeventargs)
   * [CoreWebView2WindowFeatures Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2windowfeatures)
<!-- /moved from Rendering section -->

Close:
* [CoreWebView2Controller.Close Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.close)
* [CoreWebView2.WindowCloseRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.windowcloserequested)

#### [C++](#tab/cpp)

Title:
* [ICoreWebView2::DocumentTitle property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#get_documenttitle)<!--no put-->
* [ICoreWebView2::DocumentTitleChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_documenttitlechanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_documenttitlechanged)

Fullscreen:
* [ICoreWebView2::ContainsFullScreenElement property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#get_containsfullscreenelement)<!--no put-->
* [ICoreWebView2::ContainsFullScreenElementChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_containsfullscreenelementchanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_containsfullscreenelementchanged)

Load:
* [ICoreWebView2::ContentLoading event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_contentloading), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_contentloading)
   * [ICoreWebView2ContentLoadingEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2contentloadingeventargs)
* [ICoreWebView2_2::DOMContentLoaded event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_2#add_domcontentloaded), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_2#remove_domcontentloaded)
   * [ICoreWebView2DOMContentLoadedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2domcontentloadedeventargs)

DOM/ContentLoaded: New window: <!-- moved from Rendering section -->
* [ICoreWebView2::NewWindowRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_newwindowrequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_newwindowrequested)
   * [ICoreWebView2NewWindowRequestedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2newwindowrequestedeventargs)
   * [ICoreWebView2WindowFeatures interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2windowfeatures)
<!-- /moved from Rendering section -->

Close:
* [ICoreWebView2Controller::Close method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#close)
* [ICoreWebView2::WindowCloseRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_windowcloserequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_windowcloserequested)

---

<!-- ------------------------------ -->
### Manage the network requests in WebView2
The WebResourceRequested event allows the app to intercept and override all network requests in WebView2. The WebResourceResponseReceived event allows the app to monitor the request sent to and the response received from the network. 

#### [C#](#tab/c-sharp)
* [CoreWebView2.WebResourceRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.webresourcerequested)
   * [CoreWebView2WebResourceRequestedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2webresourcerequestedeventargs)
* [CoreWebView2.WebResourceResponseReceived Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.webresourceresponsereceived)
   * [CoreWebView2WebResourceResponseReceivedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2webresourceresponsereceivedeventargs)
See also:
* [https://docs.microsoft.com/microsoft-edge/webview2/how-to/webresourcerequested](Custom management of network requests)


#### [C++](#tab/cpp)
### Manage the network requests in WebView2
The WebResourceRequested event allows the app to intercept and override all network requests in WebView2. The WebResourceResponseReceived event allows the app to monitor the request sent to and the response received from the network. 
* [ICoreWebView2::WebResourceRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_webresourcerequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_webresourcerequested)
   * [ICoreWebView2WebResourceRequestedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2webresourcerequestedeventargs)
* [ICoreWebView2_2::WebResourceResponseReceived event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_2#add_webresourceresponsereceived), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_2#remove_webresourceresponsereceived)
   * [ICoreWebView2WebResourceResponseReceivedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2webresourceresponsereceivedeventargs)

See also:
* [https://docs.microsoft.com/microsoft-edge/webview2/how-to/webresourcerequested](Custom management of network requests)
---

For more information, see:
* [Custom management of network requests](/microsoft-edge/webview2/how-to/webresourcerequested?tabs=dotnet)

<!-- ------------------------------ -->
### Server/Client Certificates

Learn how to work with certificates in WebView2 controls (TODO: confirm)
<!-- alt pattern instead of "Learn how to": provide a teaching & tech marketing moment.  Say something about how you (can) use certs, why would you do that?
By handling certs, your app can provide features A/B/C.  Your host app does X, and then the server does Y. -->

<!--
See also:
* []()
-->

#### [C#](#tab/c-sharp)

* [CoreWebView2ClientCertificate Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2clientcertificate)
   * [CoreWebView2ClientCertificateKind Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2clientcertificatekind)
* [CoreWebView2.ClientCertificateRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.clientcertificaterequested)
   * [CoreWebView2ClientCertificateRequestedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2clientcertificaterequestedeventargs)

#### [C++](#tab/cpp)

* [ICoreWebView2ClientCertificate interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2clientcertificate)
   * [COREWEBVIEW2_CLIENT_CERTIFICATE_KIND enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_client_certificate_kind)
   * [ICoreWebView2ClientCertificateCollection interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2clientcertificatecollection)<!--n/a for c#-->
* [ICoreWebView2_5::ClientCertificateRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_5#add_clientcertificaterequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_5#remove_clientcertificaterequested)
   * [ICoreWebView2ClientCertificateRequestedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2clientcertificaterequestedeventargs)

---


<!-- ====================================================================== -->
## Working with Chrome Developer Protocol (CDP)

The [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol) provides APIs to instrument, inspect, debug, and profile Chromium-based browsers. The Chrome DevTools Protocol is the foundation for the Microsoft Edge DevTools. Use the Chrome DevTools Protocol for features that aren't implemented in the WebView2 platform.

See also:
* [https://docs.microsoft.com/microsoft-edge/webview2/how-to/chromium-devtools-protocol](Use the Chrome DevTools Protocol)


#### [C#](#tab/c-sharp)

Open:
* [CoreWebView2Settings.AreDevToolsEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.aredevtoolsenabled)
* [CoreWebView2.OpenDevToolsWindow Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.opendevtoolswindow)

Call:
* [CoreWebView2.CallDevToolsProtocolMethodAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.calldevtoolsprotocolmethodasync)
* [CoreWebView2.CallDevToolsProtocolMethodForSessionAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.calldevtoolsprotocolmethodforsessionasync)

Receiver:
* [CoreWebView2.GetDevToolsProtocolEventReceiver Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.getdevtoolsprotocoleventreceiver)
   * [CoreWebView2DevToolsProtocolEventReceivedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2devtoolsprotocoleventreceivedeventargs)
   * [CoreWebView2DevToolsProtocolEventReceiver Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2devtoolsprotocoleventreceiver)

#### [C++](#tab/cpp)

Open:
* [ICoreWebView2Settings::AreDevToolsEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_aredevtoolsenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_aredevtoolsenabled)
* [ICoreWebView2::OpenDevToolsWindow method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#opendevtoolswindow)

Call:
* [ICoreWebView2::CallDevToolsProtocolMethod method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#calldevtoolsprotocolmethod)
* [ICoreWebView2_11::CallDevToolsProtocolMethodForSession method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_11#calldevtoolspotocolmethodforsession)

Receiver:
* [ICoreWebView2::GetDevToolsProtocolEventReceiver method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#getdevtoolsprotocoleventreceiver)
   * [ICoreWebView2DevToolsProtocolEventReceiver interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2devtoolsprotocoleventreceiver)
   * [ICoreWebView2DevToolsProtocolEventReceivedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2devtoolsprotocoleventreceivedeventargs)

---


<!-- ====================================================================== -->
## iFrames

iFrames allow you to embed other webpages into your own webpage. In WebView2 you can find out when they are created, when they are navigating, and allow bypassing x-frame options.

<!-- wording per overview table:
Embed other webpages into your own webpage.  Detect when embedded webpages are created, detect when embedded webpages are navigating, and optionally bypass x-frame options. -->


#### [C#](#tab/c-sharp)

* [CoreWebView2Frame Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2frame)
* [CoreWebView2FrameInfo Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2frameinfo)
* [CoreWebView2.FrameCreated Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.framecreated)
   * [CoreWebView2FrameCreatedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2framecreatedeventargs)

Navigate:
* [CoreWebView2.FrameNavigationStarting Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.framenavigationstarting)
* [CoreWebView2.FrameNavigationCompleted Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.framenavigationcompleted)
* [CoreWebView2.HistoryChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.historychanged)

#### [C++](#tab/cpp)

* [ICoreWebView2Frame interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frame)
* [ICoreWebView2FrameInfo interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frameinfo)
   * [ICoreWebView2FrameInfoCollection interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frameinfocollection)<!--n/a for c#-->
   * [ICoreWebView2FrameInfoCollectionIterator interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2frameinfocollectioniterator)<!--n/a for c#-->
* [ICoreWebView2_4::FrameCreated event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_4#add_framecreated), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_4#remove_framecreated)
   * [ICoreWebView2FrameCreatedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2framecreatedeventargs)

Navigate:
* [ICoreWebView2::FrameNavigationStarting event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_framenavigationstarting), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_framenavigationstarting)
* [ICoreWebView2::FrameNavigationCompleted event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_framenavigationcompleted), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_framenavigationcompleted)
* [ICoreWebView2::HistoryChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#add_historychanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#remove_historychanged)

---


<!-- ====================================================================== -->
## Authentication

<!-- selling point / value prop: easy configuration of WebView2 apps - support user accounts -->

Learn how to handle basic authentication in WebView2 controls.
<!-- what's the benefit for end users?  how does it essentially work? what's involved? -->

#### [C#](#tab/c-sharp)

* [CoreWebView2HttpRequestHeaders Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2httprequestheaders)
* [CoreWebView2.BasicAuthenticationRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.basicauthenticationrequested)
   * [CoreWebView2BasicAuthenticationRequestedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2basicauthenticationrequestedeventargs)
* [CoreWebView2BasicAuthenticationResponse Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2basicauthenticationresponse)
   * [CoreWebView2HttpResponseHeaders Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2httpresponseheaders)
* [CoreWebView2HttpHeadersCollectionIterator Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2httpheaderscollectioniterator)<!--TODO: why does this collectioniterator class exist in c#, usu not needed-->

#### [C++](#tab/cpp)

* [ICoreWebView2HttpRequestHeaders interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2httprequestheaders)
* [ICoreWebView2_10::BasicAuthenticationRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_10#add_basicauthenticationrequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_10#remove_basicauthenticationrequested)
   * [ICoreWebView2BasicAuthenticationRequestedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2basicauthenticationrequestedeventargs)
* [ICoreWebView2BasicAuthenticationResponse interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2basicauthenticationresponse)
   * [ICoreWebView2HttpResponseHeaders interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2httpresponseheaders)
* [ICoreWebView2HttpHeadersCollectionIterator interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2httpheaderscollectioniterator)

---


<!-- ====================================================================== -->
## Environment setup

Learn how to specify settings before WebView2 controls are created.
<!-- what kind of settings?  why? -->

<!-- selling point / value prop: easy configuration of WebView2 apps

*  What is "envir setup" - of the control?  after I add the WebView2 control to the page?  What do we mean here by "envir"?
*  Why care about it

-->

<!-- link to a doc about this if avail.  eg Auth has a doc to link to. -->
<!--
See also:
* []()
-->

#### [C#](#tab/c-sharp)

* [CoreWebView2.Environment Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.environment)
* [CoreWebView2Environment.BrowserVersionString Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.browserversionstring)
* [CoreWebView2Environment.NewBrowserVersionAvailable Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.newbrowserversionavailable)
* [CoreWebView2EnvironmentOptions Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environmentoptions)

#### [C++](#tab/cpp)

* [ICoreWebView2_2::Environment property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_2#get_environment)<!--no put-->
* [ICoreWebView2Environment::BrowserVersionString property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment#get_browserversionstring)<!--no put-->
* [ICoreWebView2Environment::NewBrowserVersionAvailable event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment#add_newbrowserversionavailable), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment#remove_newbrowserversionavailable)
* [ICoreWebView2EnvironmentOptions interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environmentoptions)

---


<!-- ====================================================================== -->
## Rendering WebView2 in non-framework apps

<!-- keep old intro?:
These APIS are used to set up the WebView2 rendering system in non-framework apps.  For example, how WebView2 renders output into the host app, how WebView2 handles input, focus, and also (for C++ only) accessibility.
-->

If you're using a UI framework for your app, you should use the WebView2 element for that UI framework.  If you're not using a UI framework for your app (for example, if you're using pure Win32 directly), or your UI framework doesn't have a WebView2 element, then you need to create `CoreWebView2Controller` and render it into your app.  If your app UI is build using `DirectComposition` or `Windows.UI.Composition`, then you should use `CoreWebView2CompositionController`; otherwise you should use `CoreWebView2Controller`.

#### [C#](#tab/c-sharp)

* [CoreWebView2Controller.CoreWebView2 Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.corewebview2)
* [CoreWebView2Controller.Close Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.close)
* [CoreWebView2Environment.CreateCoreWebView2ControllerAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createcorewebview2controllerasync)<!--2 overloads-->

#### [C++](#tab/cpp)

* [ICoreWebView2Controller::CoreWebView2 property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#get_corewebview2)<!--no put-->
* [ICoreWebView2Controller::Close method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#close)
* [ICoreWebView2Environment::CreateCoreWebView2Controller method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment#createcorewebview2controller)

---


<!-- ====================================================================== -->
## Window Management

TBD

<!-- ------------------------------ -->
### Sizing, positioning, and visibility

`CoreWebView2Controller` takes a parent `HWND`. The `Bounds` property sizes and positions the WebView2 relative to the parent `HWND`. The visibility of WebView2 can be toggled using `IsVisible`.

#### [C#](#tab/c-sharp)

* [CoreWebView2Controller.Bounds Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.bounds)
* [CoreWebView2Controller.IsVisible Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.isvisible)

#### [C++](#tab/cpp)

* [ICoreWebView2Controller::Bounds property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#get_bounds), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#put_bounds)
* [ICoreWebView2Controller::IsVisible property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#get_isvisible), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#put_isvisible)

---


<!-- ------------------------------ -->
### Zooming

WebView2 `ZoomFactor` is used to scale just the web content.  This is also update when the user zooms the content through `Ctrl`+Mouse Wheel.

#### [C#](#tab/c-sharp)

* [CoreWebView2Controller.ZoomFactor Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.zoomfactor)
* [CoreWebView2Controller.ZoomFactorChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.zoomfactorchanged)
* [CoreWebView2Controller.SetBoundsAndZoomFactor Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.setboundsandzoomfactor)

Browser/gesture/zoom features:<!-- moved from Rendering section - fits best in "gestures", or "zoom" list? -->
* [CoreWebView2Settings.IsPinchZoomEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.ispinchzoomenabled)
* [CoreWebView2Settings.IsZoomControlEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.iszoomcontrolenabled)

#### [C++](#tab/cpp)

* [ICoreWebView2Controller::ZoomFactor property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#get_zoomfactor), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#put_zoomfactor)
* [ICoreWebView2Controller::ZoomFactorChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#add_zoomfactorchanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#remove_zoomfactorchanged)
* [ICoreWebView2Controller::SetBoundsAndZoomFactor method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#setboundsandzoomfactor)

Browser/gesture/zoom features:<!-- moved from Rendering section - fits best in "gestures", or "zoom" list? -->
* [ICoreWebView2Settings5::IsPinchZoomEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings5#get_ispinchzoomenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings5#put_ispinchzoomenabled)
* [ICoreWebView2Settings::IsZoomControlEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_iszoomcontrolenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_iszoomcontrolenabled)

---


<!-- ------------------------------ -->
### Rasterization Scale

The RasterizationScale API scales all WebView2 UI including context menus, tooltip, and popups.  The app can set whether the WebView2 should detect monitor scale changes and automatically update the RasterizationScale.  `BoundsMode` is used to configure whether the Bounds property is interpreted as raw pixels, or DIPs (which need to be scaled by `RasterizationScale`).

#### [C#](#tab/c-sharp)

* [CoreWebView2Controller.BoundsMode Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.boundsmode)
   * [CoreWebView2BoundsMode Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2boundsmode)
* [CoreWebView2Controller.RasterizationScale Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.rasterizationscale)
* [CoreWebview2Controller.RasterizationScaleChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.rasterizationscalechanged)
* [CoreWebview2Controller.ShouldDetectMonitorScaleChanges Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.shoulddetectmonitorscalechanges)

#### [C++](#tab/cpp)

* [ICoreWebView2Controller3::BoundsMode property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#get_boundsmode), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#put_boundsmode)
   * [COREWEBVIEW2_BOUNDS_MODE enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_bounds_mode)
* [ICoreWebView2Controller3::RasterizationScale property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#get_rasterizationscale), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#put_rasterizationscale)
* [ICoreWebView2Controller3::RasterizationScaleChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#add_rasterizationscalechanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#remove_rasterizationscalechanged)
* [ICoreWebView2Controller3::ShouldDetectMonitorScaleChanges property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#get_shoulddetectmonitorscalechanges), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller3#put_shoulddetectmonitorscalechanges)

---


<!-- ------------------------------ -->
### Focus & Tabbing

WebView2 raises events to let the app know when it gains and loses focus. For tabbing, there is an API to move focus into WebView2 and an event for WebView2 to request the app to take focus back.

#### [C#](#tab/c-sharp)

* [CoreWebview2Controller.MoveFocus Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.movefocus)
   * [CoreWebView2MoveFocusReason Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2movefocusreason)
* [CoreWebview2Controller.MoveFocusRequested Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.movefocusrequested)
   * [CoreWebView2MoveFocusRequestedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2movefocusrequestedeventargs)
* [CoreWebview2Controller.GotFocus Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.gotfocus)
* [CoreWebview2Controller.LostFocus Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.lostfocus)

#### [C++](#tab/cpp)

* [ICoreWebview2Controller::MoveFocus method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#movefocus)
   * [COREWEBVIEW2_MOVE_FOCUS_REASON enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_move_focus_reason)
* [ICoreWebview2Controller::MoveFocusRequested event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#add_movefocusrequested), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#remove_movefocusrequested)
   * [ICoreWebView2MoveFocusRequestedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2movefocusrequestedeventargs)
* [ICoreWebview2Controller::GotFocus event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#add_gotfocus), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#remove_gotfocus)
* [ICoreWebview2Controller::LostFocus event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#add_lostfocus), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#remove_lostfocus)

---


<!-- ------------------------------ -->
### Parent Window

WebView2 can be re-parented to a different parent HWND.  WebView2 also needs to be notified when the app's position on the screen has changed.

#### [C#](#tab/c-sharp)

* [CoreWebview2Controller.NotifyParentWindowPositionChanged Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.notifyparentwindowpositionchanged)
   * [CoreWebview2Controller.ParentWindow Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.parentwindow)

#### [C++](#tab/cpp)

* [ICoreWebview2Controller::NotifyParentWindowPositionChanged method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#notifyparentwindowpositionchanged)
   * [ICoreWebview2Controller::ParentWindow property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#get_parentwindow), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#put_parentwindow)

---


<!-- ------------------------------ -->
### Keyboard Accelerators

When WebView2 has focus, it directly receives input from the user. An app may want to intercept and handle certain accelerator key combinations, or disable the normal browser accelerator key behaviors.

#### [C#](#tab/c-sharp)

* [CoreWebView2Settings.AreBrowserAcceleratorKeysEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.arebrowseracceleratorkeysenabled)
* [CoreWebView2Controller.AcceleratorKeyPressed Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.acceleratorkeypressed)
   * [CoreWebView2AcceleratorKeyPressedEventArgs Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2acceleratorkeypressedeventargs)
   * [CoreWebView2KeyEventKind Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2keyeventkind)
   * [CoreWebView2PhysicalKeyStatus Struct](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2physicalkeystatus)

#### [C++](#tab/cpp)

* [ICoreWebView2Settings3::AreBrowserAcceleratorKeysEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings3#get_arebrowseracceleratorkeysenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings3#put_arebrowseracceleratorkeysenabled)
* [ICoreWebView2Controller::AcceleratorKeyPressed event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#add_acceleratorkeypressed), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#remove_acceleratorkeypressed)
   * [ICoreWebView2AcceleratorKeyPressedEventArgs interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2acceleratorkeypressedeventargs)
   * [COREWEBVIEW2_KEY_EVENT_KIND enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_key_event_kind)
   * [COREWEBVIEW2_PHYSICAL_KEY_STATUS struct](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_physical_key_status)

---


<!-- ------------------------------ -->
### Default background color

WebView2 can specify a default background color.  This can be any opaque color or transparent.  This color will be used if the HTML page doesn't set its own background color.

#### [C#](#tab/c-sharp)

* [CoreWebView2Controller.DefaultBackgroundColor Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.defaultbackgroundcolor)
   * [CoreWebView2Color Struct](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2color)

#### [C++](#tab/cpp)

* [ICoreWebView2Controller2::DefaultBackgroundColor property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller2#get_defaultbackgroundcolor), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller2#put_defaultbackgroundcolor)
   * [COREWEBVIEW2_COLOR struct](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_color)

---


<!-- ====================================================================== -->
## Composition-based rendering

For composition-based WebView2 rendering, use the CoreWebView2Environment to create a `CoreWebView2CompositionController`.  The `CoreWebView2CompositionController` also implements all the APIs as `CoreWebView2Controller`, but includes additional APIs specific to composition based rendering.

#### [C#](#tab/c-sharp)

* [CoreWebView2CompositionController Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller)
* [CoreWebView2Environment.CreateCoreWebView2CompositionControllerAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createcorewebview2compositioncontrollerasync)<!--2 overloads-->

#### [C++](#tab/cpp)

* [ICoreWebView2CompositionController interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller)
* [ICoreWebView2Environment3::CreateCoreWebview2CompositionController method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment3#createcorewebview2compositioncontroller)

---


<!-- ------------------------------ -->
### Output

WebView2 can connect its composition tree to an [IDCompositionVisual](https://docs.microsoft.com/windows/win32/api/dcomp/nn-dcomp-idcompositionvisual), [IDCompositionTarget](https://docs.microsoft.com/windows/win32/api/dcomp/nn-dcomp-idcompositiontarget), or `Windows::UI::Composition::ContainerVisual`.<!--link these?  both plats?  not linked in wv2 api ref.  these are c++ not .net links -->

#### [C#](#tab/c-sharp)

* [CoreWebView2CompositionController.RootVisualTarget Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.rootvisualtarget)

#### [C++](#tab/cpp)

* [ICoreWebView2CompositionController::RootVisualTarget property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#get_rootvisualtarget), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#put_rootvisualtarget)

---

<!-- ------------------------------ -->
### Input

<!-- old intro draft:
Work with the mouse for input on a WebView2 control.
Your app can offer rich support UX mouse left click, scroll, rightclick, click & drag.
Your wv2 host app can offer full mouse support by handling XYZ events.  The wv2 control sends xyz events.  Your app listens for these events and handles them.  In response, your app can then XYZ.
-->

Spatial input (mouse, touch, pen) is received by the application and must be sent to WebView2.  WebView2 notifies the app when the cursor should be updated based on the mouse position.

#### [C#](#tab/c-sharp)

* [CoreWebView2CompositionController.Cursor Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.cursor)
* [CoreWebView2CompositionController.CursorChanged Event](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.cursorchanged)
* [CoreWebView2CompositionController.SystemCursorId Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.systemcursorid)
* [CoreWebView2CompositionController.SendMouseInput Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.sendmouseinput)
   * [CoreWebView2MouseEventKind Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2mouseeventkind)
   * [CoreWebView2MouseEventVirtualKeys Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2mouseeventvirtualkeys)
* [CoreWebView2CompositionController.SendPointerInput Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.sendpointerinput)
   * [CoreWebView2PointerEventKind Enum](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2pointereventkind)
* [CoreWebView2Environment.CreateCoreWebView2PointerInfo Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createcorewebview2pointerinfo)
   * [CoreWebView2PointerInfo Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2pointerinfo)

#### [C++](#tab/cpp)

* [ICoreWebView2CompositionController::Cursor property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#get_cursor)<!--no put-->
* [ICoreWebView2CompositionController::CursorChanged event (add](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#add_cursorchanged), [remove)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#remove_cursorchanged)
* [ICoreWebView2CompositionController::SystemCursorId property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#get_systemcursorid)<!--no put-->
* [ICoreWebView2CompositionController::SendMouseInput method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#sendmouseinput)
   * [COREWEBVIEW2_MOUSE_EVENT_KIND enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_mouse_event_kind)
   * [COREWEBVIEW2_MOUSE_EVENT_VIRTUAL_KEYS enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_mouse_event_virtual_keys)
* [ICoreWebView2CompositionController::SendPointerInput method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller#sendpointerinput)
   * [COREWEBVIEW2_POINTER_EVENT_KIND enum](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#corewebview2_pointer_event_kind)
* [ICoreWebView2Environment3::CreateCoreWebView2PointerInfo method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment3#createcorewebview2pointerinfo)
   * [ICoreWebView2PointerInfo interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2pointerinfo)

---

<!-- ------------------------------ -->
### Accessibility

By default, WebView2 will show up in the accessibility tree as a child of the parent HWND, for Win32/C++ apps.  WebView2 provides API to better position the WebView2 content relative to other elements in the application.

#### [C#](#tab/c-sharp)

Not applicable for C#.

<!-- 
* [CoreWebView2CompositionController.AutomationProvider Property] TODO: no C# link, ok?
* [CoreWebView2Environment.GetAutomationProviderForWindow Method] TODO: no C# link, ok?
-->

#### [C++](#tab/cpp)

* [ICoreWebView2CompositionController2::AutomationProvider property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller2#get_automationprovider)<!--no put-->
* [ICoreWebView2Environment4::GetAutomationProviderForWindow method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment4#getautomationproviderforwindow)<!--not in c#-->

---


<!-- ====================================================================== -->
## User data

Manage the user data folder (UDF), which is a folder stored on the user's machine, that contains data related to the host app and WebView2.  WebView2 apps use user data folders to store browser data, such as cookies, permissions, and cached resources.

See also:
* [Manage user data folders](user-data-folder.md)

#### [C#](#tab/c-sharp)

* [CoreWebView2Environment.UserDataFolder Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2environment.userdatafolder)

#### [C++](#tab/cpp)

* [ICoreWebView2Environment7::UserDataFolder property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2environment7#get_userdatafolder)<!--no put-->

---

<!-- ====================================================================== -->
## Performance optimizations/tools and debugging

Learn how to handle performance related events for WebView2 controls.
<!-- alt pattern instead of "Learn how to".  Teach a little about this domain: Here's what to think of by the word "perf", "optimiz", tools, and debugging perf:
Increasing perf for your app includes xyz.  Makes your app respond to user input actions quickly; prevents sluggish response.  Manage memory usage, responsiveness.  Tools help you analyze xyz & debug and test perf. -->
These APIs help you analyze and debug performance and handle performance-related events for WebView2 controls.  Make your app respond to user input actions quickly and prevent sluggish response.  Manage memory usage and responsiveness.

<!--
See also:
* []()
-->

#### [C#](#tab/c-sharp)

<!--TODO: clarify note on IsBuiltInErrorPageEnabled: (probs for navigation and process)-->

* [CoreWebView2Controller.IsVisible Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2controller.isvisible)
* [CoreWebView2.MemoryUsageTargetLevel Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.memoryusagetargetlevel)
* [CoreWebView2.TrySuspendAsync Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.trysuspendasync)
   * [CoreWebView2.IsSuspended Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.issuspended)
   * [CoreWebView2.Resume Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.resume)
* [CoreWebView2.OpenTaskManagerWindow Method](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.opentaskmanagerwindow)
* [CoreWebView2Settings.IsBuiltInErrorPageEnabled Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2settings.isbuiltinerrorpageenabled#microsoft-web-webview2-core-corewebview2settings-isbuiltinerrorpageenabled) 

#### [C++](#tab/cpp)

* [ICoreWebView2Controller::IsVisible property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#get_isvisible), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2controller#put_isvisible)
* [ICoreWebView2Experimental5::MemoryUsageTargetLevel property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2experimental5#get_memoryusagetargetlevel), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2experimental5#put_memoryusagetargetlevel)
* [ICoreWebView2_3::TrySuspend method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_3#trysuspend)
   * [ICoreWebView2_3::IsSuspended property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_3#get_issuspended)<!--no put-->
   * [ICoreWebView2_3::Resume method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_3#resume)
* [ICoreWebView2_6::OpenTaskManagerWindow method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2_6#opentaskmanagerwindow)
* [ICoreWebView2Settings::IsBuiltInErrorPageEnabled property (get](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#get_isbuiltinerrorpageenabled), [put)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2settings#put_isbuiltinerrorpageenabled)

---


<!-- ====================================================================== -->
## Misc.

<!-- description.  Addl opportunities for your app include xyz.  You'll also benefit from considering X. -->

Read settings and handle deferral.<!--define/hints-->

<!--
See also:
* []()
-->

#### [C#](#tab/c-sharp)

* [CoreWebView2.Settings Property](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2.settings)
* [CoreWebView2Deferral Class](https://docs.microsoft.com/dotnet/api/microsoft.web.webview2.core.corewebview2deferral)

#### [C++](#tab/cpp)

* [ICoreWebView2::Settings property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2#get_settings)<!--no put-->
   * [ICoreWebView2StringCollection::Count property (get)](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2stringcollection#get_count)<!--no put--><!--n/a c#-->
   * [ICoreWebView2StringCollection::GetValueAtIndex method](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2stringcollection#getvalueatindex)<!--n/a c#-->
* [ICoreWebView2Deferral interface](https://docs.microsoft.com/microsoft-edge/webview2/reference/win32/icorewebview2deferral)

---


<!-- ====================================================================== -->
<!-- ## See also -->

<!--
* []()
* []()
-->
