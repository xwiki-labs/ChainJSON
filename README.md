# ChainJSON

The purpose of this API is to have a way to synchronize JSON data types (specifically lists, maps and strings) in realtime across multiple computers for collaborative document editing. All you need is a running Chainpad instance.

# Specifications

[RealtimeJSON specifications](https://github.com/xwiki-labs/RealtimeJSON)

# How to use

Once you have a Chainpad instance you can initialize the API and create your collaborative object. Only one object can be associated to a Chainpad instance, but you can have as many sub-object as you want.
Here is an example of a collaborative form, with two fields.

```javascript
require(['ChainJSON.js'], function(ChainJSON) {

  // If you don't have a Chainpad instance yet, you can create it here.
  // ...
  // "realtime" is now representing the Chainpad object in our example.

  // Initialize the API with Chainpad :
  var RealtimeJSON = ChainJSON.init(realtime);

  // Create your collaborative object
  var obj = RealtimeJSON.getCollaborativeObject();

  // Manage the events' handlers
  RealtimeJSON.on('ready', function (fname) {
    // When fully synced, enable the modification of the fields :
    jQuery("#myform .fname").attr('disabled', false);
    jQuery("#myform .lname").attr('disabled', false);
    
    // WHen the user change the value of a field, update the collaborative object
    jQuery("#myform .fname").on('change', function() {
      obj.fname = jQuery("#myform .fname").val();
    });
    jQuery("#myform .lname").on('change', function() {
      obj.lname = jQuery("#myform .lname").val();
    });
  });

  RealtimeJSON.on('change', 'fname', function (fname) {
    // WHen the first name is change by a remote user, update the field
    jQuery("#myform .fname").val(fname);
  });

  RealtimeJSON.on('change', 'lname', function (lname) {
    // WHen the last name is change by a remote user, update the field
    jQuery("#myform .lname").val(lname);
  }); 

});
```
