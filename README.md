Sorted Table jQuery Plugin
===========================

This is a simple jQuery table sorting plugin. Nothing fancy, nothing really
impressive. Overall, really simple.

[View the demo here][0]

See the example.html document to see how to implement it. 

Example Usage
-------------

The JS:

    $("table").sortedtable();

The HTML:
  
    <table>
      <thead>
        <tr>
          <th class="type-int">int</th>
          <th class="type-float">float</th>
          <th class="type-string">string</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>15</td>
          <td>-.18</td>
          <td>banana</td>
        </tr>
        ...
        ...
        ...

The thead and tbody tags must be used.  

Add a class of "type-DATATYPE" to the th's to make them sortable by that data
type. If you don't want that column to be sortable, just don't give it a 
type-DATATYPE class.

Trimming Data
-------------

By default, the plugin will trim the data (ie: text) that is retrieved from the html.  
This makes sorting consistent based on the data.  This can be turned off by setting the option trimtext to false.

ie:
$("table").sortedtable({trimtext : false});

Changing Associated Icons
-------------------------

To change the associated jQueryUI icons used, pass in an options object.

The following properties are available:
* useicon - true to show icon, false to disable display icon (default: true)
* iconascending - the icon to show for columns in ascending sort (default: ui-icon-triangle-1-n)
* icondescending - the icon to show for columns in descending sort (default: ui-icon-triangle-1-s)

Predefined data types
---------------------

Our aim is to keep this plugin as lightweight as possible. Consequently, the 
only predefined datatypes that you can pass to the th's are

* int
* float
* string

These data types will be sufficient for many simple tables. However, if you need
different data types for sorting, you can easily create your own!

Creating your own data types
----------------------------

Creating your own data type  for sorting purposes is easy as long as you are 
comfortable using custom functions for sorting. Consult [Mozilla's Docs][1] 
if you're not.

[0]: http://joequery.github.com/Stupid-Table-Plugin/
[1]: https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Array/sort

Let's create an alphanum datatype for a User ID that takes strings in the 
form "D10", "A40", and sorts them based on the number.

    <thead>
      <tr>
        <th class="type-string">Name</th>
        <th class="type-int">Age</th>
        <th class="type-alphanum">UserID</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Joseph McCullough</td>
        <td>20</td>
        <td>D10</td>
      </tr>
      <tr>
        <td>Justin Edwards</td>
        <td>29</td>
        <td>A40</td>
      </tr>
      ...
      ...
      ...

Now we need to specify how the **alphanum** type will be sorted. To do that, 
we do the following:

    $("table").sortedtable({ sortFns : {
        "alphanum":function(a,b){

          var pattern = "^[A-Z](\\d+)$";
          var re = new RegExp(pattern);

          var aNum = re.exec(a).slice(1);
          var bNum = re.exec(b).slice(1);

          return parseInt(aNum,10) - parseInt(bNum,10);
        }
      }
    });

This extracts the integers from the cell and compares them in the style
that sort functions use.

Data with multiple representations/predefined order
---------------------------------------------------

Often we find two distinct ways of offering data: In a machine friendly way,
and a Human-friendly way. A clear example is a Timestamp. Additionally, 
arbitrary data values may already have a predefined sort order. In either case, 
it's to our advantage to have a way to store the "sortable data" while letting 
the viewer see the Human-friendly representation of that data. While the
purpose of the custom sort methods is to take data and make it sortable 
(machine friendly), sometimes this is too hard or too expensive, computationally
speaking. 

To solve this problem, you can specify a ```data-order-by``` attribute to 
table cells, and the attribute value will be the basis of the sort as opposed
to the text value of the table cell. See the complex_example.html file, where
we sort a column of letters based not on their alphabetical order, but by their
frequency in the English language. You'll still need to specify a sort type
or come up with your own custom sort function, but the presence of the
```data-order-by``` attribute tells the plugin to use the value of the 
attribute as the basis of the sort.

License
-------

The Sorted Table jQuery Plugin is licensed under the MIT license. See the LICENSE 
file for full details.

Credit
------
Original Version: https://github.com/joequery/Stupid-Table-Plugin