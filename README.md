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

On top of the options above, there are a few callback functions which you can specify as part of the options. These allow you to control the behavior of the control based upon your own events and logic. If you do not specify these, the default behavior of the control is used.

```javascript
onRenderItem: function(val,index,total,filter) { return val.tag; }
```
This function is called before an item is added to the suggestion list. This means you can use this function to alter the rows displayed inside the suggestion list. As return value, you should return the value to display in the suggestion list. Incoming parameters:


* **val**: rich object row coming from the backend, contains all properties you send from the backend, i.e val.tag and val.id
* **index**: the index number of the row in the suggestion list
* **total**: the total number of rows returned by the backend
* **filter**: the search word entered by the user

**Example**: The following code puts index numbers in front of the row values in the suggestion list. The example assumes that you return both a value and an id from your backend.

```javascript
onRenderItem: function(val,index,total,filter) { return val.id + ". " + val.tag; }
```
```javascript
onSelectItem: function(val) { }
```

This function is called after a user selects a value from the suggestion list, but before it is actually inserted into the input control. You do not have to return anything. val contains the rich object row returned from your backend. At this point, you can for example get val.id to insert in a hidden input field. Or, you could trigger the loading of dynamic content based upon a property of the selected object.

```javascript
onSelectedItem: function(val) { }
```

This function is called after a user selects a value from the suggestion list and after it is inserted into the input control. You do not have to return anything. val contains the rich object row returned from your backend. At this point, you can for example overwrite the value that TagDragon inserted into your source field.

```javascript
onLoadList: function(filter) { }
```

This function is called right before the remote loading for the suggestion list is triggered. You do not have to return anything. filter contains the search value entered by the user. At this point you can for example show a loading indicator.

```javascript
onLoadedList: function(results) { }
```

This function is called right after the values for the suggestion list are loaded. You do not have to return anything. results contains the values returned from the back-end, the data type is by default json but it can be overriden using the "dataType" parameter (see parameter list above). This method is particularly useful in combination with the dataType and visible parameters, as it allows you to completely override the rendering of TagDragon results.

#####Javascript - extra methods

Besides the main tagdragon method, there are a few addition methods that you can call yourself during runtime:

```javascript
configure([options])
```
**Note**: As of Tagdragon 1.3, this method is renamed to "tagdragon_configure()"!

Typically, you specify tagdragon options during the call to the tagdragon method. However, at anytime you can change the runtime options using the configure method. As parameter, you pass a javascript object. The following example sets a specific option during runtime for the tagdragon control named "tagbox"

```javascript
$('#tagbox').configure({'max':10});
```
```javascript
load()
```
**Note**: As of Tagdragon 1.3, this method is renamed to "tagdragon_load()"!

The load() method allows you to trigger the suggestion list during runtime. Note that the suggestion list will only show when there is a valid filter and when at least minChar characters were entered by the user.

```javascript
clear()
```
**Note**: As of Tagdragon 1.3, this method is renamed to "tagdragon_clear()"!

The clear() method allows you to hide the suggestion list during runtime.

That's it! You can go the simple way (one line of Javascript) or extensively optimize the control for your situation using the options, callbacks and methods. On to the styling...

#####CSS

TagDragon is very easy and flexible to style, there are hardly any rules. You can create a seperate stylesheet for TagDragon or integrate some rules in your own stylesheet. Here are some pointers.

In our stylesheet rules, we will refer to the id and class values that we set in the markup. There are two approaches to styling. We can style each instance individually, by refering to its id (for example #tagbox), or we can decide to make all TagDragon controls look alike. In that case we will refer to it's class (for example .tagbox).

The actual styling is best explained by an example. Let's see how style a black control

```css
   1:  #tags {

   2:  width:500px;

   3:  background:#ccc;

   4:  border:1px solid #fff;

   5:  }

   6:   

   7:  #tagbox {

   8:  width:500px;

   9:  }

  10:   

  11:  #tagbox ol  {

  12:  position:absolute;

  13:  background:#000;

  14:  list-style:none;

  15:  list-style-position: inside;

  16:  margin:0;

  17:  padding:0;

  18:  }

  19:   

  20:  #tagbox ol li {

  21:  width:500px;

  22:  border-left:1px solid #333;

  23:  border-right:1px solid #333;

  24:  }

  25:   

  26:  #tagbox ol li em {

  27:  color:#cf3;

  28:  font-weight: bold;

  29:  font-style: normal;

  30:  }

  31:   

  32:  #tagbox ol li a {

  33:  text-decoration:none;

  34:  color:#fff;

  35:  display:block;

  36:  padding:5px;

  37:  border-bottom:1px solid #333;

  38:  }

  39:   

  40:  #tagbox ol li a:hover, .hl {

  41:  background:#333;

  42:  }

  43:   

  44:  #tagbox input {

  45:  width:100%;

  46:  height: 1.5em;

  47:  padding: 2px 0px 0px 0px;

  48:  }

  49:   

  50:  #tagbox-lkup {

  51:  width:100%;

  52:  }
```

Seems like a lot? Don't worry, it's easy:

A very important thing to do is to align the suggestion list with the actual input control. To do this, we set the width of the field (#tags) and it's surrounding div (#tagbox) to the same width value.

At line 2-4 we are styling the input control itself. This is completely optional. At line 11 we are styling the suggestion list by giving it a black background color. Next, on line 20 we are styling one individual entry in the suggestion list. At line 26 we are styling the filter words in the suggestion list. These are the highlight colours you see when a partial match was made in the list when a user typed something.

At line 32 we are styling what links look like inside the suggestion list, whereas at line 40 we are styling what a highlighted row (by mouse or keyboard) in the suggestion list looks like.

Line 44 is optional additional styling for the actual input field. Finally, at line 50 we are styling the hidden div that is used for the suggestion list. It is best to leave that one alone.

As of TagDragon 1.1, you can also style alternate rows, by styling odd and even rows differently:

```css
#tagbox ol li a.td-odd {
// your css styles here
}
```

```css
#tagbox ol li a.td-even {
// your css styles here
}
```
When you know basic CSS, you can style TagDragon controls easily!

#####Back-end

Finally, the important part of integrating TagDragon with your backend. We need to get the values for the suggestion list from somewhere. Luckily, this is an easy thing to do. TagDragon can easily be integrated with any platform that:

* Can receive a POST value
* Use that post value to do a lookup in the data source
* Can return the results in JSON format

There are too many back-end platforms to explain everything here. However, some pointers may help you implement this in your own scenario. For now, we will use a simplified PHP/MySQL example.

######Receiving the POST

TagDragon uses AJAX POSTS to communicate with the backend. To receive this value in PHP, we simply do this:

```php
$search = $_POST['tag'];
```

Note that this is a simplified example. You should do security checks and input cleaning here to make for a robust backend. In addition, if you expect to receive UTF-8 characters outside the Latin-1 set, you should do a raw UTF8 URL decode here.

TagDragon by default also posts the "max" parameter that you specified in the options (if not set, the value is 10). You can use this parameter to limit the results to return from the backend.

```php
$limit = $_POST['max'];
```

Finally, you can receive any additional postdata that you specified in the postData option, using:

```php
$yourvar = $_POST['yourparamname'];
```

Once we have a reliable and secure search filter, we will use it to do a lookup in a datasource, in this case MySQL where we have a table of snake names (for example). For brevity here is the query, connection handling code excluded:

```mysql
$query= "SELECT * FROM snakes WHERE name LIKE '%$search%' ORDER BY name LIMIT 0, $limit";
```

There are a few important remarks to make here. First is security, be aware of MySQL injection, make sure $search is safe. Second, we are using SQL's LIKE clause to search for results. Note how the search word is enclosed with % characters. This means that the database will return everything that matches both the left and right side of the search word. For example, "A" will return both "Anaconda" and "Black Mamba", since both contain "A". If you only want to return results that match the right side, use '$search%'.

Of course it is also important to ORDER the results by the correct column. You will also want to use the LIMIT clause to only return a maximum number of results.

Assuming we now have the results, we need to output them in JSON format. In PHP, we first set the content type:

```php
header("Content-type: application/json");
```

Next, we loop through the query results and output the entries in JSON format. TagDragon expects a return result in this format (example output):

```json
[{"tag":"Adder"},{"tag":"African puff adder"},{"tag":"Berg adder"},{"tag":"Common adder"},{"tag":"Deaf adder"},{"tag":"Death adder"},{"tag":"Desert death adder"},{"tag":"Dwarf sand adder"},{"tag":"Horned adder"},{"tag":"Long-nosed adder"}]
```

This is JSON formatting. Notice the outside [], curly braces per item {} and name:value pairs that are "quoted", seperated by a comma. Make sure the last value does not end with a comma.

The above JSON data shows the absolute minimum data to return, that is, we are return the required "tag" name/value pair which is used to display entries in the suggestion list in the front-end. However, you are free to add as much extra object properties for each row:

```json
[{"tag":"Adder","id":12},{"tag":"African puff adder","id":23}]
```

In the example above, we have added the id property for each row. This extra property will not be displayed in the suggestion list, however, you can access it from the callback functions onRenderItem and onSelectItem. There, you can use these extra properties for your own logic.

It should be easy to output this formatting in most platforms. Here is some example PHP code:

```php
   1:  mysql_connect(localhost,$username,$password);

   2:  @mysql_select_db($database) or die( "Unable to select database");

   3:  $query= "SELECT * FROM snakes WHERE name LIKE '%$search%' ORDER BY name LIMIT 0,$limit";

   4:  $result=mysql_query($query);

   5:  $num=mysql_numrows($result);

   6:  mysql_close();

   7:   

   8:  $i=0;

   9:  $tagstring = '';

  10:  while ($i < $num) {

  11:     $tag=mysql_result($result,$i,"tag");

  12:     $tagstring.= "{\"tag\":" . json_encode($tag) . "}" . (($num-1)===$i?"":",");

  13:     $i++;

  14:  }

  15:  echo '[' . $tagstring . ']';
```

Line 1-6 deal with opening the database, doing the query and closing it. From line 10, we are looping through the results. For each row we encounter, we add output to the tagstring which will contain the total output in the end. We are using json_encode() (PHP 5.2 only) to make sure that our special characters and Unicode characters are well encoded. This is an important step, because a special character can break the validity of the output, and nothing is returned. Furthermore, on the same line (line 12) we are doing a check to see if we need to append a comma. We should not do this for the last entry. Finally, at line 15 we output the total JSON string at once.

That's all! This may seem like a lot, but trust us, you can have a fully working TagDragon control set up in a very short time!
