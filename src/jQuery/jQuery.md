
# JQuery Handbook

jQuery is a JavaScript Library.

jQuery greatly simplifies JavaScript programming.

#### [jQuery Examples](https://www.w3schools.com/jquery/jquery_examples.asp)
#### [jQuery Selector](https://www.w3schools.com/jquery/trysel.asp)

# jQuery Introduction

## What is jQuery?

jQuery is a lightweight, "write less, do more", JavaScript library.

The purpose of jQuery is to make it much easier to use JavaScript on your website.

jQuery takes a lot of common tasks that require many lines of JavaScript code to accomplish, and wraps them into methods that you can call with a single line of code.

jQuery also simplifies a lot of the complicated things from JavaScript, like AJAX calls and DOM manipulation.

The jQuery library contains the following features:

- HTML/DOM manipulation
- CSS manipulation
- HTML event methods
- Effects and animations
- AJAX
- Utilities

**Tip:** In addition, jQuery has plugins for almost any task out there.

## Adding jQuery to Your Web Pages

There are several ways to start using jQuery on your web site. You can:

- Download the jQuery library from jQuery.com
- Include jQuery from a CDN, like Google

### Downloading jQuery

There are two versions of jQuery available for downloading:

- Production version - this is for your live website because it has been minified and compressed
- Development version - this is for testing and development (uncompressed and readable code)

Both versions can be downloaded from [jQuery.com](http://jquery.com/download/).

The jQuery library is a single JavaScript file, and you reference it with the HTML `<script>` tag (notice that the `<script>` tag should be inside the `<head>` section):

``` html
<head>
<script src="jquery-3.7.1.min.js"></script>
</head>
```

**Tip:** Place the downloaded file in the same directory as the pages where you wish to use it.

### jQuery CDN

If you don't want to download and host jQuery yourself, you can include it from a CDN (Content Delivery Network).

Google is an example of someone who host jQuery:

**Google CDN:**
``` html
<head>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.7.1/jquery.min.js"></script>
</head>
```

## Functions In a Separate File

If your website contains a lot of pages, and you want your jQuery functions to be easy to maintain, you can put your jQuery functions in a separate .js file.

When we demonstrate jQuery in this tutorial, the functions are added directly into the `<head>` section. However, sometimes it is preferable to place them in a separate file, like this (use the src attribute to refer to the .js file):

``` js
<head>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.7.1/jquery.min.js"></script>
<script src="my_jquery_functions.js"></script>
</head>
```

# jQuery Syntax

With jQuery you select (query) HTML elements and perform "actions" on them.

## jQuery Syntax

The jQuery syntax is tailor-made for **selecting** HTML elements and performing some **action** on the element(s).

Basic syntax is: **$(_selector_)._action_()**

- A $ sign to define/access jQuery
- A (_selector_) to "query (or find)" HTML elements
- A jQuery _action_() to be performed on the element(s)

Examples:

`$(this).hide()` - hides the current element.

`$("p").hide()` - hides all `<p>` elements.

`$(".test").hide()` - hides all elements with class="test".

`$("#test").hide()` - hides the element with id="test".

## The Document Ready Event

You might have noticed that all jQuery methods in our examples, are inside a document ready event:

``` js
$(document).ready(function(){

  _// jQuery methods go here..._

});
```

This is to prevent any jQuery code from running before the document is finished loading (is ready).

**It is good practice to wait for the document to be fully loaded and ready before working with it.** This also allows you to have your JavaScript code before the body of your document, in the head section.

Here are some examples of actions that can fail if methods are run before the document is fully loaded:

- Trying to hide an element that is not created yet
- Trying to get the size of an image that is not loaded yet

**Tip:** The jQuery team has also created an even shorter method for the document ready event:

``` js
$(function(){

  _// jQuery methods go here..._

});
```

Use the syntax you prefer. We think that the document ready event is easier to understand when reading the code.

# jQuery Selectors

jQuery selectors allow you to select and manipulate HTML element(s).

jQuery selectors are used to "find" (or select) HTML elements based on their `name`, `id`, `classes`, `types`, `attributes`, `values` of attributes and much more. It's based on the existing [CSS Selectors](https://www.w3schools.com/cssref/css_selectors.asp), and in addition, it has some own custom selectors.

All selectors in jQuery start with the dollar sign and parentheses: `$()`.

## The `element` Selector

The jQuery element selector selects elements based on the element name.

You can select all `<p>` elements on a page like this:

``` js
$("p")
```

### Example
``` js
$(document).ready(function(){
  $("button").click(function(){
    $("p").hide();
  });
});
```
[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_hide_p)

## The `#id` Selector

The jQuery `#id` selector uses the id attribute of an HTML tag to find the specific element.

An `id` should be unique within a page, so you should use the  `#id` selector when you want to find a single, unique element.

To find an element with a specific `id`, write a hash character, followed by the id of the HTML element:
``` js
$("#test")
```

### Example

When a user clicks on a button, the element with id="test" will be hidden:

``` js
$(document).ready(function(){
  $("button").click(function(){
    $("#test").hide();
  });
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_hide_id)

## The `.class` Selector

The jQuery `.class` selector finds elements with a specific class.

To find elements with a specific class, write a period character, followed by the name of the class:

``` js
$(".test")
```

### Example

When a user clicks on a button, the elements with class="test" will be hidden:

``` js
$(document).ready(function(){
  $("button").click(function(){
    $(".test").hide();
  });
});
```
[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_hide_class)

## [More Examples of jQuery Selectors](https://www.w3schools.com/jquery/jquery_selectors.asp)

Use our [jQuery Selector Tester](https://www.w3schools.com/jquery/trysel.asp) to demonstrate the different selectors.

## jQuery Selectors References

|Selector|Example|Selects|
|---|---|---|
|[*](https://www.w3schools.com/jquery/sel_all.asp)|$("*")|All elements|
|[#_id_](https://www.w3schools.com/jquery/sel_id.asp)|$("#lastname")|The element with id="lastname"|
|[._class_](https://www.w3schools.com/jquery/sel_class.asp)|$(".intro")|All elements with class="intro"|
|[._class,_._class_](https://www.w3schools.com/jquery/sel_multiple_classes.asp)|$(".intro,.demo")|All elements with the class "intro" or "demo"|
|[_element_](https://www.w3schools.com/jquery/sel_element.asp)|$("p")|All `<p>` elements|
|[_el1_,_el2_,_el3_](https://www.w3schools.com/jquery/sel_multiple_sel.asp)|$("h1,div,p")|All `<h1>`, `<div>` and `<p>` elements|
||||
|[:first](https://www.w3schools.com/jquery/sel_first.asp)|$("p:first")|The first `<p>` element|
|[:last](https://www.w3schools.com/jquery/sel_last.asp)|$("p:last")|The last `<p>` element|
|[:even](https://www.w3schools.com/jquery/sel_even.asp)|$("tr:even")|All even `<tr>` elements|
|[:odd](https://www.w3schools.com/jquery/sel_odd.asp)|$("tr:odd")|All odd `<tr>` elements|
||||
|[:first-child](https://www.w3schools.com/jquery/sel_firstchild.asp)|$("p:first-child")|All `<p>` elements that are the first child of their parent|
|[:first-of-type](https://www.w3schools.com/jquery/sel_firstoftype.asp)|$("p:first-of-type")|All `<p>` elements that are the first `<p>` element of their parent|
|[:last-child](https://www.w3schools.com/jquery/sel_lastchild.asp)|$("p:last-child")|All `<p>` elements that are the last child of their parent|
|[:last-of-type](https://www.w3schools.com/jquery/sel_lastoftype.asp)|$("p:last-of-type")|All `<p>` elements that are the last `<p>` element of their parent|
|[:nth-child(_n_)](https://www.w3schools.com/jquery/sel_nthchild.asp)|$("p:nth-child(2)")|All `<p>` elements that are the 2nd child of their parent|
|[:nth-last-child(_n_)](https://www.w3schools.com/jquery/sel_nthlastchild.asp)|$("p:nth-last-child(2)")|All `<p>` elements that are the 2nd child of their parent, counting from the last child|
|[:nth-of-type(_n_)](https://www.w3schools.com/jquery/sel_nthoftype.asp)|$("p:nth-of-type(2)")|All `<p>` elements that are the 2nd `<p>` element of their parent|
|[:nth-last-of-type(_n_)](https://www.w3schools.com/jquery/sel_nthlastoftype.asp)|$("p:nth-last-of-type(2)")|All `<p>` elements that are the 2nd `<p>` element of their parent, counting from the last child|
|[:only-child](https://www.w3schools.com/jquery/sel_onlychild.asp)|$("p:only-child")|All `<p>` elements that are the only child of their parent|
|[:only-of-type](https://www.w3schools.com/jquery/sel_onlyoftype.asp)|$("p:only-of-type")|All `<p>` elements that are the only child, of its type, of their parent|
||||
|[parent > child](https://www.w3schools.com/jquery/sel_parent_child.asp)|$("div > p")|All `<p>` elements that are a direct child of a `<div>` element|
|[parent descendant](https://www.w3schools.com/jquery/sel_parent_descendant.asp)|$("div p")|All `<p>` elements that are descendants of a `<div>` element|
|[element + next](https://www.w3schools.com/jquery/sel_previous_next.asp)|$("div + p")|The `<p>` element that are next to each `<div>` elements|
|[element ~ siblings](https://www.w3schools.com/jquery/sel_previous_siblings.asp)|$("div ~ p")|All `<p>` elements that appear after the `<div>` element|
||||
|[:eq(_index_)](https://www.w3schools.com/jquery/sel_eq.asp)|$("ul li:eq(3)")|The fourth element in a list (index starts at 0)|
|[:gt(_no_)](https://www.w3schools.com/jquery/sel_gt.asp)|$("ul li:gt(3)")|List elements with an index greater than 3|
|[:lt(_no_)](https://www.w3schools.com/jquery/sel_lt.asp)|$("ul li:lt(3)")|List elements with an index less than 3|
|[:not(_selector_)](https://www.w3schools.com/jquery/sel_not.asp)|$("input:not(:empty)")|All input elements that are not empty|
||||
|[:header](https://www.w3schools.com/jquery/sel_header.asp)|$(":header")|All header elements `<h1>`, `<h2>` ...|
|[:animated](https://www.w3schools.com/jquery/sel_animated.asp)|$(":animated")|All animated elements|
|[:focus](https://www.w3schools.com/jquery/sel_focus.asp)|$(":focus")|The element that currently has focus|
|[:contains(_text_)](https://www.w3schools.com/jquery/sel_contains.asp)|$(":contains('Hello')")|All elements which contains the text "Hello"|
|[:has(_selector_)](https://www.w3schools.com/jquery/sel_has.asp)|$("div:has(p)")|All `<div>` elements that have a `<p>` element|
|[:empty](https://www.w3schools.com/jquery/sel_empty.asp)|$(":empty")|All elements that are empty|
|[:parent](https://www.w3schools.com/jquery/sel_parent.asp)|$(":parent")|All elements that are a parent of another element|
|[:hidden](https://www.w3schools.com/jquery/sel_hidden.asp)|$("p:hidden")|All hidden `<p>` elements|
|[:visible](https://www.w3schools.com/jquery/sel_visible.asp)|$("table:visible")|All visible tables|
|[:root](https://www.w3schools.com/jquery/sel_root.asp)|$(":root")|The document's root element|
|[:lang(_language_)](https://www.w3schools.com/jquery/sel_lang.asp)|$("p:lang(de)")|All `<p>` elements with a lang attribute value starting with "de"|
||||
|[[_attribute_]](https://www.w3schools.com/jquery/sel_attribute.asp)|$("[href]")|All elements with a href attribute|
|[[_attribute_=_value_]](https://www.w3schools.com/jquery/sel_attribute_equal_value.asp)|$("[href='default.htm']")|All elements with a href attribute value equal to "default.htm"|
|[[_attribute_!=_value_]](https://www.w3schools.com/jquery/sel_attribute_notequal_value.asp)|$("[href!='default.htm']")|All elements with a href attribute value not equal to "default.htm"|
|[[_attribute_$=_value_]](https://www.w3schools.com/jquery/sel_attribute_end_value.asp)|$("[href$='.jpg']")|All elements with a href attribute value ending with ".jpg"|
|[[_attribute_\|=_value_]](https://www.w3schools.com/jquery/sel_attribute_prefix_value.asp)|$("[title\|='Tomorrow']")|All elements with a title attribute value equal to 'Tomorrow', or starting with 'Tomorrow' followed by a hyphen|
|[[_attribute_^=_value_]](https://www.w3schools.com/jquery/sel_attribute_beginning_value.asp)|$("[title^='Tom']")|All elements with a title attribute value starting with "Tom"|
|[[_attribute_~=_value_]](https://www.w3schools.com/jquery/sel_attribute_contains_value.asp)|$("[title~='hello']")|All elements with a title attribute value containing the specific word "hello"|
|[[_attribute*_=_value_]](https://www.w3schools.com/jquery/sel_attribute_contains_string_value.asp)|$("[title*='hello']")|All elements with a title attribute value containing the word "hello"|
||||
|[:input](https://www.w3schools.com/jquery/sel_input.asp)|$(":input")|All input elements|
|[:text](https://www.w3schools.com/jquery/sel_input_text.asp)|$(":text")|All input elements with type="text"|
|[:password](https://www.w3schools.com/jquery/sel_input_password.asp)|$(":password")|All input elements with type="password"|
|[:radio](https://www.w3schools.com/jquery/sel_input_radio.asp)|$(":radio")|All input elements with type="radio"|
|[:checkbox](https://www.w3schools.com/jquery/sel_input_checkbox.asp)|$(":checkbox")|All input elements with type="checkbox"|
|[:submit](https://www.w3schools.com/jquery/sel_input_submit.asp)|$(":submit")|All input elements with type="submit"|
|[:reset](https://www.w3schools.com/jquery/sel_input_reset.asp)|$(":reset")|All input elements with type="reset"|
|[:button](https://www.w3schools.com/jquery/sel_input_button.asp)|$(":button")|All input elements with type="button"|
|[:image](https://www.w3schools.com/jquery/sel_input_image.asp)|$(":image")|All input elements with type="image"|
|[:file](https://www.w3schools.com/jquery/sel_input_file.asp)|$(":file")|All input elements with type="file"|
|[:enabled](https://www.w3schools.com/jquery/sel_input_enabled.asp)|$(":enabled")|All enabled input elements|
|[:disabled](https://www.w3schools.com/jquery/sel_input_disabled.asp)|$(":disabled")|All disabled input elements|
|[:selected](https://www.w3schools.com/jquery/sel_input_selected.asp)|$(":selected")|All selected input elements|
|[:checked](https://www.w3schools.com/jquery/sel_input_checked.asp)|$(":checked")|All checked input elements|

# jQuery Event Methods

## What are Events?

All the different visitors' actions that a web page can respond to are called events.

An event represents the precise moment when something happens.

Examples:

- moving a mouse over an element
- selecting a radio button
- clicking on an element

The term **"fires/fired"** is often used with events. Example: "The keypress event is fired, the moment you press a key".

Here are some common DOM events:

|Mouse Events|Keyboard Events|Form Events|Document/Window Events|
|---|---|---|---|
|click|keypress|submit|load|
|dblclick|keydown|change|resize|
|mouseenter|keyup|focus|scroll|
|mouseleave||blur|unload

## jQuery Syntax For Event Methods

In jQuery, most DOM events have an equivalent jQuery method.

To assign a click event to all paragraphs on a page, you can do this:

```
$("p").click();
```

The next step is to define what should happen when the event fires. You must pass a function to the event:

``` js
$("p").click(function(){
  // action goes here!!
});
```

## Commonly Used jQuery Event Methods

**$(document).ready()**

The `$(document).ready()` method allows us to execute a function when the document is fully loaded. This event is already explained in the [jQuery Syntax](https://www.w3schools.com/jquery/jquery_syntax.asp) chapter.

**click()**

The `click()` method attaches an event handler function to an HTML element.

The function is executed when the user clicks on the HTML element.

The following example says: When a click event fires on a `<p>` element; hide the current `<p>` element:

``` js
$("p").click(function(){
  $(this).hide();
});
```
[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_click)

**dblclick()**

The `dblclick()` method attaches an event handler function to an HTML element.

The function is executed when the user double-clicks on the HTML element:

``` js
$("p").dblclick(function(){
  $(this).hide();
});
```
[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_dblclick)

**mouseenter()**

The `mouseenter()` method attaches an event handler function to an HTML element.

The function is executed when the mouse pointer enters the HTML element:

``` js
$("#p1").mouseenter(function(){
  alert("You entered p1!");
});
```
[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_mouseenter)

**mouseleave()**

The `mouseleave()` method attaches an event handler function to an HTML element.

The function is executed when the mouse pointer leaves the HTML element:

``` js
$("#p1").mouseleave(function(){
  alert("Bye! You now leave p1!");
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_mouseleave)

**mousedown()**

The `mousedown()` method attaches an event handler function to an HTML element.

The function is executed, when the left, middle or right mouse button is pressed down, while the mouse is over the HTML element:

``` js
$("#p1").mousedown(function(){
  alert("Mouse down over p1!");
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_mousedown)

**mouseup()**

The `mouseup()` method attaches an event handler function to an HTML element.

The function is executed, when the left, middle or right mouse button is released, while the mouse is over the HTML element:

``` js
$("#p1").mouseup(function(){
  alert("Mouse up over p1!");
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_mouseup)

**hover()**

The `hover()` method takes two functions and is a combination of the `mouseenter()` and `mouseleave()` methods.

The first function is executed when the mouse enters the HTML element, and the second function is executed when the mouse leaves the HTML element:

``` js
$("#p1").hover(function(){
  alert("You entered p1!");
},
function(){
  alert("Bye! You now leave p1!");
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_hover)

**focus()**

The `focus()` method attaches an event handler function to an HTML form field.

The function is executed when the form field gets focus:

``` js
$("input").focus(function(){
  $(this).css("background-color", "#cccccc");
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_focus_blur)

**blur()**

The `blur()` method attaches an event handler function to an HTML form field.

The function is executed when the form field loses focus:

``` js
$("input").blur(function(){
  $(this).css("background-color", "#ffffff");
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_focus_blur)

## The on() Method

The `on()` method attaches one or more event handlers for the selected elements.

Attach a click event to a `<p>` element:

### Example

``` js
$("p").on("click", function(){
  $(this).hide();
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_on_click)

Attach multiple event handlers to a `<p>` element:

### Example
``` js
$("p").on({
  mouseenter: function(){
    $(this).css("background-color", "lightgray");
  },
  mouseleave: function(){
    $(this).css("background-color", "lightblue");
  },
  click: function(){
    $(this).css("background-color", "yellow");
  }
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_on_multiple)

## jQuery Events References

The following table lists all the jQuery methods used to handle events.

| Method / Property                                                                                                 | Description                                                                                                                                                                                                            |
| ----------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [bind()](https://www.w3schools.com/jquery/event_bind.asp)                                                         | Deprecated in version 3.0. Use the [on()](https://www.w3schools.com/jquery/event_on.asp) method instead. Attaches event handlers to elements                                                                           |
| [blur()](https://www.w3schools.com/jquery/event_blur.asp)                                                         | Attaches/Triggers the blur event                                                                                                                                                                                       |
| [change()](https://www.w3schools.com/jquery/event_change.asp)                                                     | Attaches/Triggers the change event                                                                                                                                                                                     |
| [click()](https://www.w3schools.com/jquery/event_click.asp)                                                       | Attaches/Triggers the click event                                                                                                                                                                                      |
| [dblclick()](https://www.w3schools.com/jquery/event_dblclick.asp)                                                 | Attaches/Triggers the double click event                                                                                                                                                                               |
| [delegate()](https://www.w3schools.com/jquery/event_delegate.asp)                                                 | Deprecated in version 3.0. Use the [on()](https://www.w3schools.com/jquery/event_on.asp) method instead. Attaches a handler to current, or future, specified child elements of the matching elements                   |
| [die()](https://www.w3schools.com/jquery/event_die.asp)                                                           | Removed in version 1.9. Removes all event handlers added with the live() method                                                                                                                                        |
| [error()](https://www.w3schools.com/jquery/event_error.asp)                                                       | Removed in version 3.0. Attaches/Triggers the error event                                                                                                                                                              |
| [event.currentTarget](https://www.w3schools.com/jquery/event_currenttarget.asp)                                   | The current DOM element within the event bubbling phase                                                                                                                                                                |
| [event.data](https://www.w3schools.com/jquery/event_data.asp)                                                     | Contains the optional data passed to an event method when the current executing handler is bound                                                                                                                       |
| [event.delegateTarget](https://www.w3schools.com/jquery/event_delegatetarget.asp)                                 | Returns the element where the currently-called jQuery event handler was attached                                                                                                                                       |
| [event.isDefaultPrevented()](https://www.w3schools.com/jquery/event_isdefaultprevented.asp)                       | Returns whether event.preventDefault() was called for the event object                                                                                                                                                 |
| [event.isImmediatePropagationStopped()](https://www.w3schools.com/jquery/event_isimmediatepropagationstopped.asp) | Returns whether event.stopImmediatePropagation() was called for the event object                                                                                                                                       |
| [event.isPropagationStopped()](https://www.w3schools.com/jquery/event_ispropagationstopped.asp)                   | Returns whether event.stopPropagation() was called for the event object                                                                                                                                                |
| [event.namespace](https://www.w3schools.com/jquery/event_namespace.asp)                                           | Returns the namespace specified when the event was triggered                                                                                                                                                           |
| [event.pageX](https://www.w3schools.com/jquery/event_pagex.asp)                                                   | Returns the mouse position relative to the left edge of the document                                                                                                                                                   |
| [event.pageY](https://www.w3schools.com/jquery/event_pagey.asp)                                                   | Returns the mouse position relative to the top edge of the document                                                                                                                                                    |
| [event.preventDefault()](https://www.w3schools.com/jquery/event_preventdefault.asp)                               | Prevents the default action of the event                                                                                                                                                                               |
| [event.relatedTarget](https://www.w3schools.com/jquery/event_relatedtarget.asp)                                   | Returns which element being entered or exited on mouse movement                                                                                                                                                        |
| [event.result](https://www.w3schools.com/jquery/event_result.asp)                                                 | Contains the last/previous value returned by an event handler triggered by the specified event                                                                                                                         |
| [event.stopImmediatePropagation()](https://www.w3schools.com/jquery/event_stopimmediatepropagation.asp)           | Prevents other event handlers from being called                                                                                                                                                                        |
| [event.stopPropagation()](https://www.w3schools.com/jquery/event_stoppropagation.asp)                             | Prevents the event from bubbling up the DOM tree, preventing any parent handlers from being notified of the event                                                                                                      |
| [event.target](https://www.w3schools.com/jquery/event_target.asp)                                                 | Returns which DOM element triggered the event                                                                                                                                                                          |
| [event.timeStamp](https://www.w3schools.com/jquery/event_timestamp.asp)                                           | Returns the number of milliseconds since January 1, 1970, when the event is triggered                                                                                                                                  |
| [event.type](https://www.w3schools.com/jquery/event_type.asp)                                                     | Returns which event type was triggered                                                                                                                                                                                 |
| [event.which](https://www.w3schools.com/jquery/event_which.asp)                                                   | Returns which keyboard key or mouse button was pressed for the event                                                                                                                                                   |
| [focus()](https://www.w3schools.com/jquery/event_focus.asp)                                                       | Attaches/Triggers the focus event                                                                                                                                                                                      |
| [focusin()](https://www.w3schools.com/jquery/event_focusin.asp)                                                   | Attaches an event handler to the focusin event                                                                                                                                                                         |
| [focusout()](https://www.w3schools.com/jquery/event_focusout.asp)                                                 | Attaches an event handler to the focusout event                                                                                                                                                                        |
| [hover()](https://www.w3schools.com/jquery/event_hover.asp)                                                       | Attaches two event handlers to the hover event                                                                                                                                                                         |
| [keydown()](https://www.w3schools.com/jquery/event_keydown.asp)                                                   | Attaches/Triggers the keydown event                                                                                                                                                                                    |
| [keypress()](https://www.w3schools.com/jquery/event_keypress.asp)                                                 | Attaches/Triggers the keypress event                                                                                                                                                                                   |
| [keyup()](https://www.w3schools.com/jquery/event_keyup.asp)                                                       | Attaches/Triggers the keyup event                                                                                                                                                                                      |
| [live()](https://www.w3schools.com/jquery/event_live.asp)                                                         | Removed in version 1.9. Adds one or more event handlers to current, or future, selected elements                                                                                                                       |
| [load()](https://www.w3schools.com/jquery/event_load.asp)                                                         | Removed in version 3.0. Attaches an event handler to the load event                                                                                                                                                    |
| [mousedown()](https://www.w3schools.com/jquery/event_mousedown.asp)                                               | Attaches/Triggers the mousedown event                                                                                                                                                                                  |
| [mouseenter()](https://www.w3schools.com/jquery/event_mouseenter.asp)                                             | Attaches/Triggers the mouseenter event                                                                                                                                                                                 |
| [mouseleave()](https://www.w3schools.com/jquery/event_mouseleave.asp)                                             | Attaches/Triggers the mouseleave event                                                                                                                                                                                 |
| [mousemove()](https://www.w3schools.com/jquery/event_mousemove.asp)                                               | Attaches/Triggers the mousemove event                                                                                                                                                                                  |
| [mouseout()](https://www.w3schools.com/jquery/event_mouseout.asp)                                                 | Attaches/Triggers the mouseout event                                                                                                                                                                                   |
| [mouseover()](https://www.w3schools.com/jquery/event_mouseover.asp)                                               | Attaches/Triggers the mouseover event                                                                                                                                                                                  |
| [mouseup()](https://www.w3schools.com/jquery/event_mouseup.asp)                                                   | Attaches/Triggers the mouseup event                                                                                                                                                                                    |
| [off()](https://www.w3schools.com/jquery/event_off.asp)                                                           | Removes event handlers attached with the on() method                                                                                                                                                                   |
| [on()](https://www.w3schools.com/jquery/event_on.asp)                                                             | Attaches event handlers to elements                                                                                                                                                                                    |
| [one()](https://www.w3schools.com/jquery/event_one.asp)                                                           | Adds one or more event handlers to selected elements. This handler can only be triggered once per element                                                                                                              |
| [$.proxy()](https://www.w3schools.com/jquery/event_proxy.asp)                                                     | Takes an existing function and returns a new one with a particular context                                                                                                                                             |
| [ready()](https://www.w3schools.com/jquery/event_ready.asp)                                                       | Specifies a function to execute when the DOM is fully loaded                                                                                                                                                           |
| [resize()](https://www.w3schools.com/jquery/event_resize.asp)                                                     | Attaches/Triggers the resize event                                                                                                                                                                                     |
| [scroll()](https://www.w3schools.com/jquery/event_scroll.asp)                                                     | Attaches/Triggers the scroll event                                                                                                                                                                                     |
| [select()](https://www.w3schools.com/jquery/event_select.asp)                                                     | Attaches/Triggers the select event                                                                                                                                                                                     |
| [submit()](https://www.w3schools.com/jquery/event_submit.asp)                                                     | Attaches/Triggers the submit event                                                                                                                                                                                     |
| [toggle()](https://www.w3schools.com/jquery/event_toggle.asp)                                                     | Removed in version 1.9. Attaches two or more functions to toggle between for the click event                                                                                                                           |
| [trigger()](https://www.w3schools.com/jquery/event_trigger.asp)                                                   | Triggers all events bound to the selected elements                                                                                                                                                                     |
| [triggerHandler()](https://www.w3schools.com/jquery/event_triggerhandler.asp)                                     | Triggers all functions bound to a specified event for the selected elements                                                                                                                                            |
| [unbind()](https://www.w3schools.com/jquery/event_unbind.asp)                                                     | Deprecated in version 3.0. Use the [off()](https://www.w3schools.com/jquery/event_off.asp) method instead. Removes an added event handler from selected elements                                                       |
| [undelegate()](https://www.w3schools.com/jquery/event_undelegate.asp)                                             | Deprecated in version 3.0. Use the [off()](https://www.w3schools.com/jquery/event_off.asp) method instead. Removes an event handler to selected elements, now or in the future                                         |
| [unload()](https://www.w3schools.com/jquery/event_unload.asp)                                                     | Removed in version 3.0. Use the [on()](https://www.w3schools.com/jquery/event_on.asp) or [trigger()](https://www.w3schools.com/jquery/event_trigger.asp) method instead. Attaches an event handler to the unload event |

# # jQuery Effects

## jQuery Fading Methods

With jQuery you can fade an element in and out of visibility.

jQuery has the following fade methods:

- `fadeIn()`
- `fadeOut()`
- `fadeToggle()`
- `fadeTo()`

## jQuery Sliding Methods

With jQuery you can create a sliding effect on elements.

jQuery has the following slide methods:

- `slideDown()`
- `slideUp()`
- `slideToggle()`

## jQuery Animations - The animate() Method

The jQuery `animate()` method is used to create custom animations.

## jQuery stop() Method

The jQuery `stop()` method is used to stop an animation or effect before it is finished.

# jQuery Callback Functions

JavaScript statements are executed line by line. However, with effects, the next line of code can be run even though the effect is not finished. This can create errors.

To prevent this, you can create a callback function.

A callback function is executed after the current effect is finished.

Typical syntax: **`$(_selector_).hide(_speed,callback_);`**

**Examples**

The example below has a callback parameter that is a function that will be executed after the hide effect is completed:

### Example with Callback

``` js
$("button").click(function(){
  $("p").hide("slow", function(){
    alert("The paragraph is now hidden");
  });
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_hide_callback)

The example below has no callback parameter, and the alert box will be displayed before the hide effect is completed:

### Example without Callback

``` js
$("button").click(function(){
  $("p").hide(1000);
  alert("The paragraph is now hidden");
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_hide_no_callback)

# jQuery - Chaining

With jQuery, you can chain together actions/methods.

Chaining allows us to run multiple jQuery methods (on the same element) within a single statement.

## jQuery Method Chaining

Until now we have been writing jQuery statements one at a time (one after the other).

However, there is a technique called chaining, that allows us to run multiple jQuery commands, one after the other, on the same element(s).

**Tip:** This way, browsers do not have to find the same element(s) more than once.

To chain an action, you simply append the action to the previous action.

The following example chains together the `css()`, `slideUp()`, and `slideDown()` methods. The "p1" element first changes to red, then it slides up, and then it slides down:

### Example

``` js
$("#p1").css("color", "red").slideUp(2000).slideDown(2000);
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_chaining)

We could also have added more method calls if needed.

**Tip**: When chaining, the line of code could become quite long. However, jQuery is not very strict on the syntax; you can format it like you want, including line breaks and indentations.

This also works just fine:

### Example

``` js
$("#p1").css("color", "red")
  .slideUp(2000)
  .slideDown(2000);
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_chaining2)

jQuery throws away extra whitespace and executes the lines above as one long line of code.

# jQuery HTML

## jQuery DOM Manipulation

**DOM = Document Object Model**

## Get/Set Content

Three simple, but useful, jQuery methods for DOM manipulation are:

- `text()` - Sets or returns the text content of selected elements
- `html()` - Sets or returns the content of selected elements (including HTML markup)
- `val()` - Sets or returns the value of form fields

## Add New HTML Content

We will look at four jQuery methods that are used to add new content:

- `append()` - Inserts content at the end of the selected elements
- `prepend()` - Inserts content at the beginning of the selected elements
- `after()` - Inserts content after the selected elements
- `before()` - Inserts content before the selected elements

## Remove Elements/Content

To remove elements and content, there are mainly two jQuery methods:

- `remove()` - Removes the selected element (and its child elements)
- `empty()` - Removes the child elements from the selected element

## jQuery Manipulating CSS

jQuery has several methods for CSS manipulation. We will look at the following methods:

- `addClass()` - Adds one or more classes to the selected elements
- `removeClass()` - Removes one or more classes from the selected elements
- `toggleClass()` - Toggles between adding/removing classes from the selected elements
- `css()` - Sets or returns the style attribute

## jQuery css() Method

### Return a CSS Property

`css("_propertyname_");`

``` js
$("p").css("background-color");
```
### Set a CSS Property

`css("_propertyname_","_value_");`

``` js
$("p").css("background-color", "yellow");
```

### Set Multiple CSS Properties

`css({"_propertyname_":"_value_","_propertyname_":"_value_",...});`

``` js
$("p").css({"background-color": "yellow", "font-size": "200%"});
```

## jQuery HTML / CSS Reference

The following table lists all the methods used to manipulate the HTML and CSS.

The methods below work for both HTML and XML documents. Exception: the html() method.

| Method                                                                   | Description                                                                             |
| ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------- |
| [addClass()](https://www.w3schools.com/jquery/html_addclass.asp)         | Adds one or more class names to selected elements                                       |
| [after()](https://www.w3schools.com/jquery/html_after.asp)               | Inserts content after selected elements                                                 |
| [append()](https://www.w3schools.com/jquery/html_append.asp)             | Inserts content at the end of selected elements                                         |
| [appendTo()](https://www.w3schools.com/jquery/html_appendto.asp)         | Inserts HTML elements at the end of selected elements                                   |
| [attr()](https://www.w3schools.com/jquery/html_attr.asp)                 | Sets or returns attributes/values of selected elements                                  |
| [before()](https://www.w3schools.com/jquery/html_before.asp)             | Inserts content before selected elements                                                |
| [clone()](https://www.w3schools.com/jquery/html_clone.asp)               | Makes a copy of selected elements                                                       |
| [css()](https://www.w3schools.com/jquery/css_css.asp)                    | Sets or returns one or more style properties for selected elements                      |
| [detach()](https://www.w3schools.com/jquery/html_detach.asp)             | Removes selected elements (keeps data and events)                                       |
| [empty()](https://www.w3schools.com/jquery/html_empty.asp)               | Removes all child nodes and content from selected elements                              |
| [hasClass()](https://www.w3schools.com/jquery/html_hasclass.asp)         | Checks if any of the selected elements have a specified class name                      |
| [height()](https://www.w3schools.com/jquery/css_height.asp)              | Sets or returns the height of selected elements                                         |
| [html()](https://www.w3schools.com/jquery/html_html.asp)                 | Sets or returns the content of selected elements                                        |
| [innerHeight()](https://www.w3schools.com/jquery/html_innerheight.asp)   | Returns the height of an element (includes padding, but not border)                     |
| [innerWidth()](https://www.w3schools.com/jquery/html_innerwidth.asp)     | Returns the width of an element (includes padding, but not border)                      |
| [insertAfter()](https://www.w3schools.com/jquery/html_insertafter.asp)   | Inserts HTML elements after selected elements                                           |
| [insertBefore()](https://www.w3schools.com/jquery/html_insertbefore.asp) | Inserts HTML elements before selected elements                                          |
| [offset()](https://www.w3schools.com/jquery/css_offset.asp)              | Sets or returns the offset coordinates for selected elements (relative to the document) |
| [offsetParent()](https://www.w3schools.com/jquery/css_offsetparent.asp)  | Returns the first positioned parent element                                             |
| [outerHeight()](https://www.w3schools.com/jquery/html_outerheight.asp)   | Returns the height of an element (includes padding and border)                          |
| [outerWidth()](https://www.w3schools.com/jquery/html_outerwidth.asp)     | Returns the width of an element (includes padding and border)                           |
| [position()](https://www.w3schools.com/jquery/css_position.asp)          | Returns the position (relative to the parent element) of an element                     |
| [prepend()](https://www.w3schools.com/jquery/html_prepend.asp)           | Inserts content at the beginning of selected elements                                   |
| [prependTo()](https://www.w3schools.com/jquery/html_prependto.asp)       | Inserts HTML elements at the beginning of selected elements                             |
| [prop()](https://www.w3schools.com/jquery/html_prop.asp)                 | Sets or returns properties/values of selected elements                                  |
| [remove()](https://www.w3schools.com/jquery/html_remove.asp)             | Removes the selected elements (including data and events)                               |
| [removeAttr()](https://www.w3schools.com/jquery/html_removeattr.asp)     | Removes one or more attributes from selected elements                                   |
| [removeClass()](https://www.w3schools.com/jquery/html_removeclass.asp)   | Removes one or more classes from selected elements                                      |
| [removeProp()](https://www.w3schools.com/jquery/html_removeprop.asp)     | Removes a property set by the prop() method                                             |
| [replaceAll()](https://www.w3schools.com/jquery/html_replaceall.asp)     | Replaces selected elements with new HTML elements                                       |
| [replaceWith()](https://www.w3schools.com/jquery/html_replacewith.asp)   | Replaces selected elements with new content                                             |
| [scrollLeft()](https://www.w3schools.com/jquery/css_scrollleft.asp)      | Sets or returns the horizontal scrollbar position of selected elements                  |
| [scrollTop()](https://www.w3schools.com/jquery/css_scrolltop.asp)        | Sets or returns the vertical scrollbar position of selected elements                    |
| [text()](https://www.w3schools.com/jquery/html_text.asp)                 | Sets or returns the text content of selected elements                                   |
| [toggleClass()](https://www.w3schools.com/jquery/html_toggleclass.asp)   | Toggles between adding/removing one or more classes from selected elements              |
| [unwrap()](https://www.w3schools.com/jquery/html_unwrap.asp)             | Removes the parent element of the selected elements                                     |
| [val()](https://www.w3schools.com/jquery/html_val.asp)                   | Sets or returns the value attribute of the selected elements (for form elements)        |
| [width()](https://www.w3schools.com/jquery/css_width.asp)                | Sets or returns the width of selected elements                                          |
| [wrap()](https://www.w3schools.com/jquery/html_wrap.asp)                 | Wraps HTML element(s) around each selected element                                      |
| [wrapAll()](https://www.w3schools.com/jquery/html_wrapall.asp)           | Wraps HTML element(s) around all selected elements                                      |
| [wrapInner()](https://www.w3schools.com/jquery/html_wrapinner.asp)       | Wraps HTML element(s) around the content of each selected element                       |

## jQuery Dimension Methods

jQuery has several important methods for working with dimensions:

- `width()`
- `height()`
- `innerWidth()`
- `innerHeight()`
- `outerWidth()`
- `outerHeight()`

## jQuery Dimensions

<img src=https://www.w3schools.com/jquery/img_jquerydim.gif >

### Example

The following example sets the width and height of a specified `<div>` element:

``` js
$("button").click(function(){
  $("#div1").width(500).height(500);
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_dim_width_height_set)

# jQuery Traversing

## What is Traversing?

jQuery traversing, which means "move through", are used to "find" (or select) HTML elements based on their relation to other elements. Start with one selection and move through that selection until you reach the elements you desire.

The image below illustrates an HTML page as a tree (DOM tree). With jQuery traversing, you can easily move up (ancestors), down (descendants) and sideways (siblings) in the tree, starting from the selected (current) element. This movement is called traversing - or moving through - the DOM tree.

![jQuery Tree](https://www.w3schools.com/jquery/img_travtree.png)

Illustration explained:

- The `<div>` element is the **parent** of `<ul>`, and an **ancestor** of everything inside of it
- The `<ul>` element is the **parent** of both `<li>` elements, and a **child** of `<div>`
- The left `<li>` element is the **parent** of `<span>`, **child** of `<ul>` and a **descendant** of `<div>`
- The `<span>` element is a **child** of the left `<li>` and a **descendant** of `<ul>` and `<div>`
- The two `<li>` elements are **siblings** (they share the same parent)
- The right `<li>` element is the **parent** of `<b>`, **child** of `<ul>` and a **descendant** of `<div>`
- The `<b>` element is a **child** of the right `<li>` and a **descendant** of `<ul>` and `<div>`

**Note:**

- An ancestor is a parent, grandparent, great-grandparent, and so on.
- A descendant is a child, grandchild, great-grandchild, and so on.
- Siblings share the same parent.

## jQuery Traversing - Ancestors

With jQuery you can traverse up the DOM tree to find ancestors of an element.

An ancestor is a parent, grandparent, great-grandparent, and so on.

### Traversing Up the DOM Tree

Three useful jQuery methods for traversing up the DOM tree are:

| `parent()`       | `$("span").parent()`            |
| ---------------- | ------------------------------- |
| `parents()`      | `$("span").parents()`           |
| `parentsUntil()` | `$("span").parentsUntil("div")` |

## jQuery Traversing - Descendants

With jQuery you can traverse down the DOM tree to find descendants of an element.

A descendant is a child, grandchild, great-grandchild, and so on.

### Traversing Down the DOM Tree

| `children()` | `$("div").children()`   |
| ------------ | ----------------------- |
| `find()`     | `$("div").find("span")` |

## jQuery Traversing - Siblings

With jQuery you can traverse sideways in the DOM tree to find siblings of an element.

Siblings share the same parent.

### Traversing Sideways in The DOM Tree

- `siblings()`
- `next()`
- `nextAll()`
- `nextUntil()`
- `prev()`
- `prevAll()`
- `prevUntil()`

## jQuery Traversing - Filtering

### The first(), last(), eq(), filter() and not() Methods

The most basic filtering methods are `first()`, `last()` and `eq()`, which allow you to select a specific element based on its position in a group of elements.

Other filtering methods, like `filter()` and `not()` allow you to select elements that match, or do not match, a certain criteria.

## jQuery Traversing Reference

|Method|Description|
|---|---|
|[add()](https://www.w3schools.com/jquery/traversing_add.asp)|Adds elements to the set of matched elements|
|addBack()|Adds the previous set of elements to the current set|
|andSelf()|Deprecated in version 1.8. An alias for addBack()|
|[children()](https://www.w3schools.com/jquery/traversing_children.asp)|Returns all direct children of the selected element|
|[closest()](https://www.w3schools.com/jquery/traversing_closest.asp)|Returns the first ancestor of the selected element|
|[contents()](https://www.w3schools.com/jquery/traversing_contents.asp)|Returns all direct children of the selected element (including text and comment nodes)|
|[each()](https://www.w3schools.com/jquery/traversing_each.asp)|Executes a function for each matched element|
|end()|Ends the most recent filtering operation in the current chain, and return the set of matched elements to its previous state|
|[eq()](https://www.w3schools.com/jquery/traversing_eq.asp)|Returns an element with a specific index number of the selected elements|
|[filter()](https://www.w3schools.com/jquery/traversing_filter.asp)|Reduce the set of matched elements to those that match the selector or pass the function's test|
|[find()](https://www.w3schools.com/jquery/traversing_find.asp)|Returns descendant elements of the selected element|
|[first()](https://www.w3schools.com/jquery/traversing_first.asp)|Returns the first element of the selected elements|
|[has()](https://www.w3schools.com/jquery/traversing_has.asp)|Returns all elements that have one or more elements inside of them|
|[is()](https://www.w3schools.com/jquery/traversing_is.asp)|Checks the set of matched elements against a selector/element/jQuery object, and return true if at least one of these elements matches the given arguments|
|[last()](https://www.w3schools.com/jquery/traversing_last.asp)|Returns the last element of the selected elements|
|map()|Passes each element in the matched set through a function, producing a new jQuery object containing the return values|
|[next()](https://www.w3schools.com/jquery/traversing_next.asp)|Returns the next sibling element of the selected element|
|[nextAll()](https://www.w3schools.com/jquery/traversing_nextall.asp)|Returns all next sibling elements of the selected element|
|[nextUntil()](https://www.w3schools.com/jquery/traversing_nextuntil.asp)|Returns all next sibling elements between two given arguments|
|[not()](https://www.w3schools.com/jquery/traversing_not.asp)|Returns elements that do not match a certain criteria|
|[offsetParent()](https://www.w3schools.com/jquery/traversing_offsetparent.asp)|Returns the first positioned parent element|
|[parent()](https://www.w3schools.com/jquery/traversing_parent.asp)|Returns the direct parent element of the selected element|
|[parents()](https://www.w3schools.com/jquery/traversing_parents.asp)|Returns all ancestor elements of the selected element|
|[parentsUntil()](https://www.w3schools.com/jquery/traversing_parentsuntil.asp)|Returns all ancestor elements between two given arguments|
|[prev()](https://www.w3schools.com/jquery/traversing_prev.asp)|Returns the previous sibling element of the selected element|
|[prevAll()](https://www.w3schools.com/jquery/traversing_prevall.asp)|Returns all previous sibling elements of the selected element|
|[prevUntil()](https://www.w3schools.com/jquery/traversing_prevuntil.asp)|Returns all previous sibling elements between two given arguments|
|[siblings()](https://www.w3schools.com/jquery/traversing_siblings.asp)|Returns all sibling elements of the selected element|
|[slice()](https://www.w3schools.com/jquery/traversing_slice.asp)|Reduces the set of matched elements to a subset specified by a range of indices|

# jQuery - AJAX Introduction

AJAX is the art of exchanging data with a server, and updating parts of a web page - without reloading the whole page.

## What is AJAX?

AJAX = Asynchronous JavaScript and XML.

In short; AJAX is about loading data in the background and display it on the webpage, without reloading the whole page.

## What About jQuery and AJAX?

jQuery provides several methods for AJAX functionality.

With the jQuery AJAX methods, you can request text, HTML, XML, or JSON from a remote server using both HTTP Get and HTTP Post - And you can load the external data directly into the selected HTML elements of your web page!

# jQuery - AJAX load() Method

## jQuery load() Method

The jQuery `load()` method is a simple, but powerful AJAX method.

The `load()` method loads data from a server and puts the returned data into the selected element.

**Syntax:**

``` js
$(_selector_).load(_URL,data,callback_);
```

The required URL parameter specifies the URL you wish to load.

The optional data parameter specifies a set of querystring key/value pairs to send along with the request.

The optional callback parameter is the name of a function to be executed after the `load()` method is completed.

**Here is the content of our example file: "demo_test.txt":**

``` js
<h2>jQuery and AJAX is FUN!!!</h2>
<p id="p1">This is some text in a paragraph.</p>
```

The following example loads the content of the file "demo_test.txt" into a specific `<div>` element:

### Example

``` js
$("#div1").load("demo_test.txt");
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_ajax_load)

It is also possible to add a jQuery selector to the URL parameter.

The following example loads the content of the element with id="p1", inside the file "demo_test.txt", into a specific `<div>` element:

### Example

``` js
$("#div1").load("demo_test.txt #p1");
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_ajax_load2)

The optional callback parameter specifies a callback function to run when the `load()` method is completed. The callback function can have different parameters:

- `responseTxt` - contains the resulting content if the call succeeds
- `statusTxt` - contains the status of the call
- `xhr` - contains the XMLHttpRequest object

The following example displays an alert box after the load() method completes. If the `load()` method has succeeded, it displays "External content loaded successfully!", and if it fails it displays an error message:

### Example

``` js
$("button").click(function(){
  $("#div1").load("demo_test.txt", function(responseTxt, statusTxt, xhr){
    if(statusTxt == "success")
      alert("External content loaded successfully!");
    if(statusTxt == "error")
      alert("Error: " + xhr.status + ": " + xhr.statusText);
  });
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_ajax_load_callback)

# jQuery - AJAX get() and post() Methods

The jQuery get() and post() methods are used to request data from the server with an HTTP GET or POST request.

## HTTP Request: GET vs. POST

Two commonly used methods for a request-response between a client and server are: GET and POST.

- **GET** - Requests data from a specified resource
- **POST** - Submits data to be processed to a specified resource

GET is basically used for just getting (retrieving) some data from the server. **Note:** The GET method may return cached data.

POST can also be used to get some data from the server. However, the POST method NEVER caches data, and is often used to send data along with the request.

To learn more about GET and POST, and the differences between the two methods, please read our [HTTP Methods GET vs POST](https://www.w3schools.com/tags/ref_httpmethods.asp) chapter.

## jQuery $.get() Method

The `$.get()` method requests data from the server with an HTTP GET request.

**Syntax:**

``` js
$.get(_URL,callback_);
```

The required URL parameter specifies the URL you wish to request.

The optional callback parameter is the name of a function to be executed if the request succeeds.

The following example uses the `$.get()` method to retrieve data from a file on the server:

### Example

``` js
$("button").click(function(){
  $.get("demo_test.asp", function(data, status){
    alert("Data: " + data + "\nStatus: " + status);
  });
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_ajax_get)

The first parameter of `$.get()` is the URL we wish to request ("demo_test.asp").

The second parameter is a callback function. The first callback parameter holds the content of the page requested, and the second callback parameter holds the status of the request.

**Tip:** Here is how the ASP file looks like ("demo_test.asp"):

``` js
<%
response.write("This is some text from an external ASP file.")
%>
```

## jQuery $.post() Method

The `$.post()` method requests data from the server using an HTTP POST request.

**Syntax:**

``` js
$.post(_URL,data,callback_);
```

The required URL parameter specifies the URL you wish to request.

The optional data parameter specifies some data to send along with the request.

The optional callback parameter is the name of a function to be executed if the request succeeds.

The following example uses the `$.post()` method to send some data along with the request:

### Example

``` js
$("button").click(function(){
  $.post("demo_test_post.asp",
  {
    name: "Donald Duck",
    city: "Duckburg"
  },
  function(data, status){
    alert("Data: " + data + "\nStatus: " + status);
  });
});
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_ajax_post)

The first parameter of `$.post()` is the URL we wish to request ("demo_test_post.asp").

Then we pass in some data to send along with the request (name and city).

The ASP script in "demo_test_post.asp" reads the parameters, processes them, and returns a result.

The third parameter is a callback function. The first callback parameter holds the content of the page requested, and the second callback parameter holds the status of the request.

**Tip:** Here is how the ASP file looks like ("demo_test_post.asp"):

``` js
<%
dim fname,city
fname=Request.Form("name")
city=Request.Form("city")
Response.Write("Dear " & fname & ". ")
Response.Write("Hope you live well in " & city & ".")
%>
```

## jQuery AJAX Reference

|Method|Description|
|---|---|
|[$.ajax()](https://www.w3schools.com/jquery/ajax_ajax.asp)|Performs an async AJAX request|
|$.ajaxPrefilter()|Handle custom Ajax options or modify existing options before each request is sent and before they are processed by $.ajax()|
|[$.ajaxSetup()](https://www.w3schools.com/jquery/ajax_ajaxsetup.asp)|Sets the default values for future AJAX requests|
|$.ajaxTransport()|Creates an object that handles the actual transmission of Ajax data|
|[$.get()](https://www.w3schools.com/jquery/ajax_get.asp)|Loads data from a server using an AJAX HTTP GET request|
|[$.getJSON()](https://www.w3schools.com/jquery/ajax_getjson.asp)|Loads JSON-encoded data from a server using a HTTP GET request|
|$.parseJSON()|Deprecated in version 3.0, use [JSON.parse()](https://www.w3schools.com/js/js_json_parse.asp) instead. Takes a well-formed JSON string and returns the resulting JavaScript value|
|[$.getScript()](https://www.w3schools.com/jquery/ajax_getscript.asp)|Loads (and executes) a JavaScript from a server using an AJAX HTTP GET request|
|[$.param()](https://www.w3schools.com/jquery/ajax_param.asp)|Creates a serialized representation of an array or object (can be used as URL query string for AJAX requests)|
|[$.post()](https://www.w3schools.com/jquery/ajax_post.asp)|Loads data from a server using an AJAX HTTP POST request|
|[ajaxComplete()](https://www.w3schools.com/jquery/ajax_ajaxcomplete.asp)|Specifies a function to run when the AJAX request completes|
|[ajaxError()](https://www.w3schools.com/jquery/ajax_ajaxerror.asp)|Specifies a function to run when the AJAX request completes with an error|
|[ajaxSend()](https://www.w3schools.com/jquery/ajax_ajaxsend.asp)|Specifies a function to run before the AJAX request is sent|
|[ajaxStart()](https://www.w3schools.com/jquery/ajax_ajaxstart.asp)|Specifies a function to run when the first AJAX request begins|
|[ajaxStop()](https://www.w3schools.com/jquery/ajax_ajaxstop.asp)|Specifies a function to run when all AJAX requests have completed|
|[ajaxSuccess()](https://www.w3schools.com/jquery/ajax_ajaxsuccess.asp)|Specifies a function to run when an AJAX request completes successfully|
|[load()](https://www.w3schools.com/jquery/ajax_load.asp)|Loads data from a server and puts the returned data into the selected element|
|[serialize()](https://www.w3schools.com/jquery/ajax_serialize.asp)|Encodes a set of form elements as a string for submission|
|[serializeArray()](https://www.w3schools.com/jquery/ajax_serializearray.asp)|Encodes a set of form elements as an array of names and values|

# jQuery - Filters

Use jQuery to filter/search for specific elements.

## Filter Tables

Perform a case-insensitive search for items in a table:

``` js
<script>
$(document).ready(function(){
  $("#myInput").on("keyup", function() {
    var value = $(this).val().toLowerCase();
    $("#myTable tr").filter(function() {
      $(this).toggle($(this).text().toLowerCase().indexOf(value) > -1)
    });
  });
});
</script>
```

[Try it Yourself »](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_filters_table)

## [Filter Lists](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_filters_list)

Perform a case-insensitive search for items in a list:

## [Filter Anything](https://www.w3schools.com/jquery/tryit.asp?filename=tryjquery_filters_anything)

Perform a case-insensitive search for text inside a div element:

### EOF
