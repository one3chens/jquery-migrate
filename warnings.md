# jQuery Migrate Plugin - Warning Messages

To allow developers to identify and fix compatibility issues when migrating older jQuery code, the development (uncompressed) version of the plugin generates console warning messages whenever any of its functionality is called. The messages only appear once on the console for each unique message. 

**In most cases these messages are simply _warnings_; code should continue to work properly with later versions of jQuery as long as the jQuery Migrate plugin is used, but we recommmend changing the code where possible to eliminate warnings so that the plugin does not need to be used.**

The production (compressed) version of the plugin does not generate these warnings. To continue using jQuery code that has compatibility issues without making any changes and without console messages, simply include the production version in the file rather than the development version. See the [README](README.md) for download instructions.

All warnings generated by this plugin start with the text "JQMIGRATE" for easy identification. The warning messages, causes, and remediation instructions are listed below.

### JQMIGRATE: jQuery.attrFn is deprecated

**Cause:** Prior to jQuery 1.8, the undocumented `jQuery.attrFn` object provided a list of properties supported by the `$(html, props)` method. It is no longer required as of jQuery 1.8, but some developers "discovered" it by reading the source and began to use it. Their code still expects `jQuery.attrFn` to be present, attempts to assign values to it, and will throw errors if it is not present.

**Solution:** Ensure that you are using the latest version of jQuery UI (1.8.21 or later) and jQuery Mobile (1.2.1 or later); they no longer use `jQuery.attrFn`. Examine any third-party plugins for the string `attrFn` and report its use to the plugin authors (not to jQuery team).

### JQMIGRATE: Can't change the 'type' of an input or button in IE 6/7/8

**Cause:** IE 6, 7, and 8 throw an error if you attempt to change the type attribute of an input or button element, for example to change a radio button to a checkbox. Prior to 1.9, jQuery threw an error for every browser to create consistent behavior. As of jQuery 1.9 setting the type is allowed, but will still throw an error in oldIE. 

**Solution:** For compatibility with oldIE, do not attempt to change the type of an input element. Instead, create a new element in your code and replace the old one.

### JQMIGRATE: jQuery is not compatible with Quirks Mode

**Cause:** A browser runs in "quirks mode" when the HTML document does not have a `<!doctype ...>` as its first non-blank line, or when the doctype in the file is invalid. This mode causes the browser to emulate 1990s-era (HTML3) behavior. In Internet Explorer, it also causes many high-performance APIs to be hidden in order to better emulate ancient browsers. jQuery has never been compatible with, or tested in, quirks mode.

**Solution:** Put a [valid doctype](http://www.w3.org/QA/2002/04/valid-dtd-list.html) in the document and ensure that the document is rendering in standards mode. The simplest valid doctype is the HTML5 one, which we highly recommend: `<!doctype html>` . The jQuery Migrate plugin does not attempt to fix issues related to quirks mode.

### JQMIGRATE: jQuery.browser is deprecated

**Cause:** `jQuery.browser` was deprecated in version 1.3, and finally removed in 1.9. Browser sniffing is notoriously unreliable as means of detecting whether to implement particular features. 

**Solution:** Where possible, use feature detection to make code decisions rather than trying to detect a specific browser. The [Modernizr](http://modernizr.com) library provides a wide variety of feature detections. As a last resort, you can directly look at the `navigator.userAgent` string to detect specific strings returned by the browser.

### JQMIGRATE: jQuery.sub() is deprecated

**Cause:** The `jQuery.sub()` method provided an imperfect way for a plugin to isolate itself from changes to the `jQuery` object. Due to its shortcomings, it was deprecated in version 1.8 and removed in 1.9.

**Solution:** Rewrite the code that depends on `jQuery.sub()`, use the minified production version of the jQuery Migrate plugin to provide the functionality, or extract the `jQuery.sub()` method from the plugin's source and use it in the application.

### JQMIGRATE: 'hover' pseudo-event is deprecated, use 'mouseenter mouseleave'

**Cause:** Until jQuery 1.9, the string "hover" was allowed as an alias for the string "mouseenter mouseleave" when attaching an event handler. This unusual exception provided no real benefit and prevented the use of the name "hover" as a triggered event. _Note: This is not related to the `.hover()` method, which has not been deprecated._

**Solution:** Replace use of the string "hover" with "mouseenter mouseleave" within any `.on()`, `.bind()`, `.delegate()`, or `.live()` event binding method.

### JQMIGRATE: jQuery.fn.error() is deprecated

**Cause:** The `$().error()` method was used to attach an "error" event to an element but has been removed in 1.9 to reduce confusion with the `$.error()` method which is unrelated and has not been deprecated. It also serves to discourage the temptation to use `$(window).error()` which does not work because `window.onerror` does not follow standard event handler conventions.

**Solution:** Change any use of `$().error(fn)` to `$().on("error", fn)`.

### JQMIGRATE: jQuery.fn.toggle(handler, handler...) is deprecated

**Cause:** There are two completely different meanings for the `.toggle()` method. The use of `.toggle()` to show or hide elements is _not_ affected. The use of `.toggle()` as a specialized click handler was deprecated in 1.8 and removed in 1.9. 

**Solution:** Rewrite the code that depends on `$().toggle()`, use the minified production version of the jQuery Migrate plugin to provide the functionality, or extract the `$().toggle()` method from the plugin's source and use it in the application. 

### JQMIGRATE: jQuery.fn.live() is deprecated; jQuery.fn.die() is deprecated

**Cause:** The `.live()` and `.die()` methods were deprecated in 1.7 due to their [performance and usability drawbacks](http://api.jquery.com/live), and are no longer supported. 

**Solution:** Rewrite calls to `.live()` using `.on()` or `.delegate()`. Instructions for doing so are provided in the [`.live()` API documentation](http://api.jquery.com/live).

### JQMIGRATE: AJAX events should be attached to document

**Cause:** As of jQuery 1.9, the global AJAX events (ajaxStart, ajaxStop, ajaxSend, ajaxComplete, ajaxError, and ajaxSuccess) are only triggered on the `document` element. 

**Solution:** Change the program to listen for the AJAX events on the document. For example, if the code currently looks like this:
```javascript
$("#status").ajaxStart(function(){ $(this).text("Ajax started"); });
```
Change it to this:
```javascript
$(document).ajaxStart(function(){ $("#status").text("Ajax started"); });
```

### JQMIGRATE: Global events are undocumented and deprecated

**Cause:** jQuery 1.9 does not support globally triggered events. The only documented global events were the AJAX events and they are now triggered only on `document` as discussed above. jQuery never provided a documented interface for outside code to trigger global events.

**Solution:** Change the program to avoid the use of global events. The jQuery Migrate plugin warns about this case but does _not_ restore the previous behavior since it was undocumented. 

### JQMIGRATE: jQuery.fn.attr( props, pass ) is deprecated

**Cause**: Prior to jQuery 1.9, `$().attr()` supported an undocumented `pass` argument that was primarily used with the `$(html, props)` signature. This undocumented argument has been removed and `$(html, props)` is now implemented differently.

**Solution:** Update any code that makes use of the `pass` argument. Older versions of jQuery UI used this argument but they should be updated to version 1.8.24 at minimum for use with jQuery 1.8 or later.

### JQMIGRATE: property-based jQuery.fn.attr('value') is deprecated

**Cause**: Prior to jQuery 1.9, `$().attr("value")` retrieved the value *property* instead of the value *attribute* (which generally reflects the value that was read from HTML markup). This caused inconsistent behavior with selectors referencing the value attribute.

**Solution**: Use `$().val()` (for form controls) or `$().prop("value")` (for other elements) to get the *current* value.

### JQMIGRATE: property-based jQuery.fn.attr('value', val) is deprecated

**Cause**: Prior to jQuery 1.9, `$().attr( "value", val )` set the value *property* instead of the value *attribute*. This caused inconsistent behavior with selectors referencing the value attribute.

**Solution**: Use `$().val( val )` (for form controls) or `$().prop( "value", val )` (for other elements) to set the *current* value.

### JQMIGRATE: jQuery.buildFragment() is deprecated

**Cause**: The `jQuery.buildFragment()` method was an undocumented internal method removed in jQuery 1.9. However, we are aware of some plugins or other code that may be using it.

**Solution**: Rewrite any code that makes use of this method or other undocumented methods. For example the `jQuery.parseHTML()` method, introduced in jQuery 1.8, can convert HTML to an array of DOM elements that you can append to a document fragment.

### JQMIGRATE: Use of jQuery.fn.data('events') is deprecated

**Cause**: Prior to 1.9, `.data("events")` could be used to retrieve jQuery's undocumented internal event data structure for an element if no other code had defined a data element with the name "events". This special case has been removed in 1.9. 

**Solution**: There is no public interface to retrieve this internal data structure, and it remains undocumented. The only useful applications might be for debugging. The data is available via `jQuery._data("events")` but this is not a documented interface.
