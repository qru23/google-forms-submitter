# Google Forms Ajax Submitter

Easily submit a client side form to a Google Forms by posting to the action URL of the form, and translate the fields to the corresponding Goolge Form field names.

## 1. Getting Form URL

Get the form URL by inspecting the form element on your google forms page and copying the link from the action attribute .

![Get Post URL](https://i.imgur.com/HaFjLjQ.png)

## 2. Getting Input Field Names

Similarly, get the appropriate name attributes by inspecting each input field in your google form.

![Get Input Name](https://i.imgur.com/0pgPE0F.png)

## 3. Set it all up in your client side JS

This example uses jQuery and Ajax, but this could be done however you find comfortable.

```javascript
// The URL you got from the form action attribute
let formURL = "https://docs.google.com/forms/u/5/d/e/CxxxOxxOxxxL_ExxXxxAxxxMxxxPxxxxLxxE-FxxOxxxRxxM/formResponse";

// Your own form implementation
let form = $('#yourForm');

// An object that lets easily translate our own form's name fields with the name fields from the google form
let nameTranslationTable = {
  'name': 'entry.288288288',
  'company': 'entry.23232323',
  'email': 'entry.12341234',
  'phone': 'entry.99999999',
};

// Override the form submit action
form.on('submit', function(e) {
  // Prevent default action
  e.preventDefault()

  // If you want to do any client side validation, do it here.

  // Use the name translation table to convert our own form's response with google form's expected response
  var params = {};
  Object.keys(nameTranslationTable).forEach(e => {
    params[nameTranslationTable[e]] = form.find(`[name='${e}']`).val();
  });

  // Serialize
  let serializedData = $.param(params);
  
  // Submit the form
  $.ajax({
    url: formURL,
    method: 'POST',
    data: serializedData,
  })
  // Get a response. This response will ALWAYS BE A CORS ERROR.
  .always(function(r){
    console.log('Form submitted!')
    // Display thank you page, fire a pixel, or show a message.
  });
});
```

### Happy forming!