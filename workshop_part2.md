Topic - WebComponents
---------------------

http://slides.com/kybernetikos/webcomponents

Workshop - Contacts List
------------------------

The next package that we look at is the contacts list.
It is a custom element that shows a list of contacts for our chat application.

To start

    git clone https://github.com/briandipalma/spa-2014-contacts-list

1. Open `index.html`
    
    There is one new `script` tag.
    
    It's the Custom Elements polyfill, part of the polymer project.
    https://github.com/Polymer/CustomElements
    
    System.config now has css and text loading plugin which allows the es6
    module loader to load css and text files.  The CSS plugin automatically
    attaches the css to the head as soon as you import the css module.  The
    text plugin returns the content of the file as a javascript string.
    
    The other libraries are for integration later.
    
    This custom element is already wired up to be notified of model changes.
    
    Look at ContactsListElement.js
    
    The state member variable holds the information that needs to be added to the DOM, it's an ES6 Map.
    It's keys are usernames and values are a objects with keys "status" (value of "online" or "offline") and
    "imageSource" which is a URL for a user avatar.
    
    render is the function that's called whenever the state changes. For simplicity you can
    discard the entire DOM everytime.
    
    A Custom Element is just a HTML element so standard DOM methods will work on it 
    (this).

2. Register the custom element implementation ContactsListElement in index.html
    under the name spa2014-contacts-list
    
3. Add a <spa2014-contacts-list> element to the body of the index.html
    Load up the index.html file in your browser and see the element in the inspector
    and check for the console logs.

4. In the render method, iterate over the state map and display its data in the
    DOM.
    
    Use a document fragment to hold the DOM to add while iterating over the Map.
    
    var documentFragment = document.createDocumentFragment();
    
    this.state.forEach((contactState, contact) => {
        //create DOM elements to hold the contact data
        //which consists of the contact avatar and contact name.
        //Append to documentFragment.
    });
    // append the documentFragment to your

5. Once the list of contacts is displayed create a template that represents the
    component.  The template should have a header with the text 'Contacts', an
    <hr> and a section for the contacts to be placed.

6. Change your render method to place the document fragment in the contacts section.
    You can save the contacts section during the attachedCallback as this.contactsSection
    
7. Add an event listener to yourself to handle click events.
    Use arrow functions to minimize  boilerplate for the callback.  Pass the
    mouse event in to the _onContactSelected method.

    Use destructuring to extract all the details from the event.
    Destructuring works to multiple levels.

    Look at the page in the browser, and click on one of the contact rows. You
    should see a log line in the console.

    Look at contactSelected in ContactsListActions.js to see the console log line
    which uses String templates.
