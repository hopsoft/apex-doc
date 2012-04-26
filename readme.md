# Oracle Application Express (APEX) Quick Reference

A collection of the meaty technical info from [APEX's official documentation](http://docs.oracle.com/cd/E23903_01/doc/doc.41/e21674.pdf).

Allows developers and other techies to ramp up quickly.

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

[More info...](http://docs.oracle.com/cd/E23903_01/doc/doc.41/e21674/concept_url.htm#BEIFCDGF)
