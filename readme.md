# Oracle Application Express (APEX) Quick Reference

A collection of the meaty technical info from [APEX's official documentation](http://docs.oracle.com/cd/E23903_01/doc/doc.41/e21674.pdf) which allows developers and other tech minded folks to ramp up quickly.

APEX is targeted at business oriented folks with high technical aptitude... not necessarily developers.
Because of this, the official docs are a mix of technical detail and user manual style descriptions.

This readme aspires to be a sane technical APEX reference and should save you from beating your head against the desk.

[Read the official docs here...](http://docs.oracle.com/cd/E23903_01/doc/doc.41/e21674.pdf)

## Intro

APEX is a full stack development platform for building web applications on top of the Oracle database.
Similar to offerings such as SAP Portal and Microsoft Sharepoint.
It fits a unique & narrow niche aimed at lowering the barrier to entry for application building.

#### A note to developers

Before you tuck tail and run, be aware that APEX also competes with the likes of other full stack offerings such as Rails, Django, & Node.
The primary difference is the target audience.
APEX is targeted at business oriented folks with a high technical aptitude... not developers.

Even though it might feel like developing with your hands tied, I would encourage you to stick with it.
APEX is quite powerful and even enjoyable *(...gasp)* once you become more familiar with it.

## Understanding APEX's URL Structure

```
f?p=App:Page:Session:Request:Debug:ClearCache:itemNames:itemValues:PrinterFriendly
```

* `App` Indicates an application ID or alphanumeric alias.
* `Page` Indicates a page number or alphanumeric alias.
* `Session` Identifies a session ID.
* `Request` Sets the value of REQUEST. This can be used to run an PL/SQL [Application Process](http://docs.oracle.com/cd/E23903_01/doc/doc.41/e21674/bldr_app_proc.htm#HTMDB25210).
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

[More...](http://docs.oracle.com/cd/E23903_01/doc/doc.41/e21674/bldr_sc.htm#HTMDB04009)

## APIs

APEX provides several functions and procedures out of the box. This functionality can be leveraged with PL/SQL from within your APEX app.

The available APIs are.

* APEX_UTIL
* APEX_MAIL
* APEX_ITEM
* APEX_APPLICATION
* APEX_CUSTOM_AUTH
* APEX_LDAP

[More...](http://docs.oracle.com/cd/B28359_01/appdev.111/b32258/api.htm)

### APEX_UTIL

APEX_UTIL is a library of functions and procedures that are callable from PL/SQL.

Consider this example which uses values from a multi-select list. The example uses the following session variables.

* **:MY_PARENT** - A single number that represents a parent record's primary key.
* **:MY_VALUES** - A colon delimited list of numbers that represent the primary keys of child records. *(Set via a multi-select list)*

```
DECLARE
  --  declare a variable to hold the selected values
  my_list APEX_APPLICATION_GLOBAL.VC_ARR2;
BEGIN
  -- assign the values to the variable by invoking an APEX_UTIL method
  my_list := APEX_UTIL.STRING_TO_TABLE(:MY_VALUES);

  DELETE FROM my_table WHERE parent_id = :MY_PARENT;

  -- iterate over the list
  FOR i IN 1..my_list.count
  LOOP
    INSERT INTO my_table (parent_id, child_id)
        VALUES (:MY_PARENT, my_list(i));
  END LOOP;
END;
```

Here is the list of everything provided by APEX_UTIL.

* `CACHE_GET_DATE_OF_PAGE_CACHE` Function
* `CACHE_GET_DATE_OF_REGION_CACHE` Function
* `CACHE_PURGE_BY_APPLICATION` Procedure
* `CACHE_PURGE_BY_PAGE` Procedure
* `CACHE_PURGE_STALE` Procedure
* `CHANGE_CURRENT_USER_PW` Procedure
* `CHANGE_PASSWORD_ON_FIRST_USE` Function
* `CLEAR_APP_CACHE` Procedure
* `CLEAR_PAGE_CACHE` Procedure
* `CLEAR_USER_CACHE` Procedure
* `COUNT_CLICK` Procedure
* `CREATE_USER` Procedure
* `CREATE_USER_GROUP` Procedure
* `CURRENT_USER_IN_GROUP` Function
* `DOWNLOAD_PRINT_DOCUMENT` Procedure
* `EDIT_USER` Procedure
* `END_USER_ACCOUNT_DAYS_LEFT` Function
* `EXPIRE_END_USER_ACCOUNT` Procedure
* `EXPIRE_WORKSPACE_ACCOUNT` Procedure
* `EXPORT_USERS` Procedure
* `FETCH_APP_ITEM` Function
* `FETCH_USER` Procedure
* `FIND_SECURITY_GROUP_ID` Function
* `FIND_WORKSPACE` Function
* `GET_ACCOUNT_LOCKED_STATUS` Function
* `GET_ATTRIBUTE` Function
* `GET_AUTHENTICATION_RESULT` Function
* `GET_BLOB_FILE_SRC` Function
* `GET_CURRENT_USER_ID` Function
* `GET_DEFAULT_SCHEMA` Function
* `GET_EMAIL` Function
* `GET_FILE` Procedure
* `GET_FILE_ID` Function
* `GET_FIRST_NAME` Function
* `GET_GROUPS_USER_BELONGS_TO` Function
* `GET_GROUP_ID` Function
* `GET_GROUP_NAME` Function
* `GET_LAST_NAME` Function
* `GET_NUMERIC_SESSION_STATE` Function
* `GET_PREFERENCE` Function
* `GET_PRINT_DOCUMENT` Function
* `GET_SESSION_STATE` Function
* `GET_USER_ID` Function
* `GET_USER_ROLES` Function
* `GET_USERNAME` Function
* `IS_LOGIN_PASSWORD_VALID` Function
* `IS_USERNAME_UNIQUE` Function
* `KEYVAL_NUM` Function
* `KEYVAL_VC2` Function
* `LOCK_ACCOUNT` Procedure
* `PASSWORD_FIRST_USE_OCCURRED` Function
* `PREPARE_URL` Function
* `PUBLIC_CHECK_AUTHORIZATION` Function
* `PURGE_REGIONS_BY_APP` Procedure
* `PURGE_REGIONS_BY_NAME` Procedure
* `PURGE_REGIONS_BY_PAGE` Procedure
* `REMOVE_PREFERENCE` Procedure
* `REMOVE_SORT_PREFERENCES` Procedure
* `REMOVE_USER` Procedure
* `RESET_AUTHORIZATIONS` Procedure
* `RESET_PW` Procedure
* `SAVEKEY_NUM` Function
* `SAVEKEY_VC2` Function
* `SET_ATTRIBUTE` Procedure
* `SET_AUTHENTICATION_RESULT` Procedure
* `SET_CUSTOM_AUTH_STATUS` Procedure
* `SET_EMAIL` Procedure
* `SET_FIRST_NAME` Procedure
* `SET_LAST_NAME` Procedure
* `SET_PREFERENCE` Procedure
* `SET_SESSION_LIFETIME_SECONDS` Procedure
* `SET_SESSION_MAX_IDLE_SECONDS` Procedure
* `SET_SESSION_STATE` Procedure
* `SET_USERNAME` Procedure
* `STRONG_PASSWORD_CHECK` Procedure
* `STRONG_PASSWORD_VALIDATION` Function
* `STRING_TO_TABLE` Function
* `TABLE_TO_STRING` Function
* `UNEXPIRE_END_USER_ACCOUNT` Procedure
* `UNEXPIRE_WORKSPACE_ACCOUNT` Procedure
* `UNLOCK_ACCOUNT` Procedure
* `URL_ENCODE` Function
* `WORKSPACE_ACCOUNT_DAYS_LEFT` Function

[More...](http://docs.oracle.com/cd/B28359_01/appdev.111/b32258/api.htm#CHDBDCIE)

## Misc

* Writing tests for PL/SQL with Ruby. [https://github.com/rsim/ruby-plsql](https://github.com/rsim/ruby-plsql)



