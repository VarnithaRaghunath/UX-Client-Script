# UX-Client-Script
When creating the page scripts within UI builder, servicenow doesn't allow you to use Web API (except console and timeout), hence you are not able to access document or window objects. One neat trick that you can use is to create a reusable UX Client Script Include and import it into the page script.

## steps:
create a new UX client script include (type sys_ux_client_script_include.list into the app navigator and click new)
provide a name e.g. global and paste the following code into script field

```
function include({imports}) { 
  var Function = function(){}.constructor;
  var global = Function("return this")();

  return global;
}
```

save the record
create a new page script within UI builder and import your script include (make sure to use the whole API name of that script include. In a global scope it should be global.global)

```
function handler({api, event, helpers, imports}) {
    const {document, window} = imports['global.global']();
    window.alert(`Hello from ${document.URL}`);    
}
```
create a new event handler with your script
fire that event and observe


IN script.js
#Record Operations Utilities which can be in imported in any UIB client script (Make sure to make the client script include accessible in all scopes)

createRecord - Function to execute create record data broker with necessary arguments
updateRecord - Function to execute update record data broker with necessary arguments
deleteRecord - Function to execute delete record data broker with necessary arguments
/*Sample script to show how to import client script include can be included :- function handler({api, event, helpers, imports}) { const { createRecord, updateRecord, deleteRecord } = imports'global.Record Operation Utilities'; } */
