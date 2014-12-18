TagDragon - jQuery Autosuggest plugin
=========

**Note**: TagDragon used to be a commercial jQuery plugin. It is hereby free, donated to the community to use, fork and improve. Support by the original author is limited. Use as you wish.

####Introduction
Tagdragon is a simple, robust and configurable autosuggest plugin that allows you to easily implement *auto suggest* or *type-ahead* functionality on your web page.

The basic working of the plugin is to assign it to an input or textarea element on your web page and to link it to a back-end JSON service that provides the suggestions. There are several options to configure the exact behavior. In addition, the control can run in default mode, where a dropdown-like control is rendered, or in override mode, where you can completely overrule the rendering of the output of the returned suggestions.

####Features

* Single value, multiple value, free value
* Works for input boxes and text areas
* Just one line of Javascript
* Lightweight library (<5K)
* Easy to style
* Superb cross browser support
* Full Unicode support
* Keyboard navigation
* Easy to integrate into any back-end
* Does not conflict with other plugins
* Gracefully handles disabled Javascript
* Slim markup, no trickeries
* Allows for multiple controls per page
* Configurable value seperator
* Configurable caching
* Configurable delay
* Configurable result limit
* Configurable minimum characters
* Configurable postdata
* Rich callback API for even more control
* Alternate rows

####Usage

Basic usage of TagDragon is to include the correct markup, load the library, and make a single javascript call. 

#####HTML

TagDragon is designed to work with regular HTML forms. You can apply it to input text boxes or textarea controls. You can have multiple TagDragon controls on one page. You simple place your regular controls with the form tags, and then apply the correct markup to the controls you want to have autosuggest functionality.

The markup to create a TagDragon control for an input text box is as follow:

```html
<div id="tagbox" class="tagbox">
   <input type="text" value="" id="tags" />
</div>
```

We wrap the input text box in a div and give it a unique name, which we will use in both the CSS and Javascript that follows. It is also important to give the input field a unique id value. You can of course preceed the control with your own label or fieldset tags.

The markup to create a TagDragon control for a text area box is as follow:

```html
<div id="tagbox2" class="tagbox">
   <textarea id="tags2" rows="5" cols="50"></textarea>
</div>
```

The principles and rules here are exactly the same as for text input controls. You can of course set the rows and cols values of the text area to your own preferences. As you can see, we are giving this second control a different id value.

#####Javascript - basics

Before we can enable the suggestion lists for the controls we just created, we first need to include the TagDragon libraries in our web page. You can choose between the uncompressed version (jquery.tagdragon.js), which has full code comments and is easy to debug, or the production version (jquery.tagdragon.min.js), which is compressed and very light. We also need a copy of jQuery, here we can either download the latest version, or link directly to Google's online version. For now we will assume that you are using the production version of both jQuery and TagDragon. Place the following markup inside the head section of your web page:

```javascript
<script type="text/javascript" src="js/jquery.min.js"></script>
<script type="text/javascript" src="js/jquery.tagdragon.min.js"></script>
<script type="text/javascript" src="js/main.js"></script>
```

Let us discuss this line by line. Line 1 starts the jQuery code, it is jQuery's main event, much like an onload event. Line 2 triggers TagDragon for the "tagbox" div we used to surround our input box. As parameters, we are passing the name of the field, and the address of the backend script that will return the suggestion results. Line 3 does the same thing, but this time for the second tagbox, using the field "tags2". We are also giving it to other parameters.

As you can see, it only takes one line of Javascript code to enable autosuggestion for your fields. In these calls you can pass in parameters to control the behavior. Here are the available options:

Option  | Description
------------- | -------------
**field**  | The name of the field you want to use. **Required**.
**url**  | The address of the backend script that returns the suggestion list. This can be an absolute or relative URL. **Required**.

The following parameters are optional, if you do not specify a value, the default value will be used:

Option  | Description
------------- | -------------
**tagsep**  | The seperator character to use for multiple values. You can specify any character, common values are ',' and ' ' (space). **Default**: ','
**enclose**  | When a value from the suggestion list is selected that contains multiple words, it is enclosed by the character specified here. You will typically want to use this when you specify a space character as tag seperator (tagsep). In that case, you will want to set the enclose value to "(quote). **Default**: not set
**max** | The maximum number of entries to show in the suggestion list. **Default**: 10
**cache** | Whether to enable result caching or not. It is recommended to have this enabled. **Default**: true
**delay** | The time in milliseconds TagDragon waits after the last key up event of the user to start loading the suggestion list results. When you have fast-typing users this may lead to a rapid set of remote lookups that are not needed, since only the last character is relevant. By setting a slight delay, less requests are fired and more relevant results are returned. **Default**: 500
**charMin** | The minimum characters to enter before the suggestion list is loaded. **Default**: 1. When you set this to 0, the suggestion list is loaded upon focus of the field.
**dblClick** | Whether or not the suggestion list is loaded upon double-clicking inside the control. **Default**: true. Note that the suggestion list will only show when there are at least minChar characters are entered and when the remote lookup actually returned any result.
**postData** | Extra postdata to send to the back-end. Specify this value in Javascript object notation, for example: postData:{'myparam1':'myvalue1', 'myparam2':'myvalue2'}
**visible** | Parameter that indicates whether the suggestion list should be rendered at all. **Default**: true. You typically set this to false when you want to completely override the rendering of the results from the back-end.
**dataType** | Parameter to set the data type of the result from the back-end. **Default**: json. You can set this to "html" if that is what your back-end returns and you want to take care of your own rendering.

#####Javascript - callbacks
