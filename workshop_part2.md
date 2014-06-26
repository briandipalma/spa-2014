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

    `System.config` now has `css` and `text` loading plugins which allow the `System` module loader to load css and text.
    The CSS plugin automatically attaches the css to the head as soon as you import the css module.
    The text plugin returns the content of the file as a javascript string.
    The other packages are for integration later.

2. Open `src/index.js`

    This is our package's entry point. It's also the module that's loaded in the `index.html` file.
    You can see that it re-exports `ContactsListElement` from the `'./ContactsListElement'` module.
    The name it re-exports it as is `ContactsListElement`.

3. Open `src/ContactsListElement.js`

    The module exports a class that extends `HTMLElement` and has the Custom Element callbacks as methods.

    A Custom Element is just a HTML element so standard DOM methods will work on it.
    For example in the `createdCallback` callback the element injects its template into its DOM structure.

    ```javascript
    this.innerHTML = contactsListTemplate;
    ```

    All DOM methods can be now run on the `this` pointer. 
    For instance if you wanted to add an event listener to your element you would write.

    ```javascript    
    this.addEventListener('event', (event) => console.log('click'));
    ```

4. Serve the package

	All these packages can be served by a static server rooted at the package directory.
	One is included in the package and can be launched with the command.
	
	```bash
	$ npm run serve
	```
	
	The included server will serve the package at http://127.0.0.1:8080
	Opening the console should show none of the log lines from the element.

5. Register the custom element

    For the browser to match up a Custom Element tag name with your JS Custom Element class you need to register it.
    The method to use for registration is `document.registerElement`.
    It expects the element type (tagname) and then the element class to be passed in.

    Inside `index.html` register `ContactsListElement` under the name `spa2014-contacts-list`.
    The module object will provide the element as `module.ContactsListElement` since it's exported by `index`.

    Now add a `spa2014-contacts-list` tag to the body of the page.
    Refresh and verify that the element logs are in the console.

6. S
    As there is no default model solution for Web Components we've had to provide our own.
    This custom element is already wired up to be notified of model changes.

    The state member variable holds the information that needs to be added to the DOM, it's an ES6 Map.
    It's keys are usernames and values are a objects with keys "status" (value of "online" or "offline") and
    "imageSource" which is a URL for a user avatar.

    render is the function that's called whenever the state changes. For simplicity you can
    discard the entire DOM everytime.

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
