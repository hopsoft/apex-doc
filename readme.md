# Oracle Application Express (APEX) Quick Reference

A collection of the meaty technical info from [APEX's official documentation](http://docs.oracle.com/cd/E23903_01/doc/doc.41/e21674.pdf) which allows developers and other techies to ramp up quickly.

## Intro

APEX is a full stack development platform for building web applications on top of the Oracle database.
Similar to offerings such as SAP Portal and Microsoft Sharepoint.
It fits a unique & narrow niche aimed at lowering the barrier to entry for application building.

Before you tuck tail and run, be aware that APEX also competes with the likes of other full stack offerings such as Rails, Django, & Node.

The primary difference is the target audience.
APEX is targeted at business oriented folks with a high technical aptitude... not necessarily developers.
Because of this, the official docs are a mix of technical detail and user manual style descriptions.

This readme serves as a quick reference to the technical detail so you can jump in fast.

[Read the official docs here...](http://docs.oracle.com/cd/E23903_01/doc/doc.41/e21674.pdf)

## Understanding APEX's URL Structure

```
f?p=App:Page:Session:Request:Debug:ClearCache:itemNames:itemValues:PrinterFriendly
```

* `App` Indicates an application ID or alphanumeric alias.
* `Page` Indicates a page number or alphanumeric alias.
* `Session` Identifies a session ID.
* `Request` Sets the value of REQUEST.
* `Debug` [YES|NO] Indicates whether or not to debug the page.
* `ClearCache` [[ITEM_NAME],RP,APP,SESSION] Clears the cache. This sets the value of items to null.
* `itemNames` Comma-delimited list of item names to set session state for.
* `itemValues` Comma-delimited list of item values to assign to `itemNames`.
* `PrinterFriendly` Indicates whether or not to render the page in printer friendly mode.

[More...](http://docs.oracle.com/cd/E23903_01/doc/doc.41/e21674/concept_url.htm#BEIFCDGF)

## Session State

All `Items` in APEX persist their values to session state.

This means a textbox named `MY_TEXTBOX` will persist its value into a session key of `MY_TEXTBOX` whenever the page submits or links to another page. *This is why all item names must be unique.*

### How to use session values

* **SQL** `:MY_TEXTBOX` or `:"MY_TEXTBOX"`
* **PL/SQL** `V('MY_TEXTBOX')` or `NV('MY_TEXTBOX')` for numeric values.
* **Static text** `&MY_TEXTBOX.`

[More...](http://docs.oracle.com/cd/E23903_01/doc/doc.41/e21674/concept_ses_val.htm)

## Pages

Pages are split into 3 logical sections.

* **Page Rendering** - Represents the lifecyle of constructing the page.
* **Page Processing** - Represents what happens when the page is submitted... (the page submission lifecycle).
* **Shared Components** - Represents reusable items that are shared between pages.

![APEX page structure](https://img.skitch.com/20120426-g6634a7wt85nmsfjix79e3cdam.png)

### Page Rendering

The rendering lifecycle is comprised of the following events.

1. Before Header
1. After Header
1. Before Regions
1. Regions
1. After Regions
1. Before Footer
1. After Footer

[More...](http://docs.oracle.com/cd/E23903_01/doc/doc.41/e21674/bldr_pg_def_about.htm#HTMDB04014)

### Page Processing

The processing lifecycle is comprised of the following events.

1. After Submit
1. Validating
1. Processing
1. After Processing

[More...](http://docs.oracle.com/cd/E23903_01/doc/doc.41/e21674/bldr_pg_def_about.htm#HTMDB04015)

### Page Components

These are the tools APEX gives you to manipulate the above lifecycles.
Components can be injected at each stage of the lifecycle.

* **Branches** - A logic component used to fork the user experience based on user activity.
* **Buttons** - A physical component used to submit the page or link to other pages.
* **Computations** - A logic component for assigning values to session state.
* **Dynamic Actions** - A logic component for controlling client side behavior.
* **Items** - A physical component that represents an HTML form element.
* **Processes** - A logic component that supports PL/SQL & DML (Data Manipulation Language).
* **Regions** - A physical component that serves as a container for HTML content.
* **Validations** - A logic component for verifying user input.

### Shared Components

These are common elements that can be shared between pages.

* **Breadcrumbs** - A hierarchical list of links that is rendered using a template.
* **Lists** - A collection of links that is rendered using a template.
* **Lists of Values** - A static or dynamic list of key/value pairs.
* **Navigation Bar** - A navigation component to navigate between pages.
* **Security** - An authorization scheme that controls access to the application.
* **Tabs** - A navigation component to navigate between pages.
* **Templates** - An HTML snippet used for rendering data.
* **Theme** - A named collection of templates.





