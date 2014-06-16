Notes on the second part of the workshop.

git clone https://github.com/briandipalma/spa-2014-contacts-list

Explain the structure/contents of the project.

Open up index.html, you can see the polyfills that provide the Custom Elements functionality.

https://github.com/Polymer/CustomElements

	Provides Custom Element polyfill.

There is no model solution defined by Web Components, left open to libraries to come up with their own.
If everyone standardises on one it will be added to Web Components.

A simple model solution is provided with the repo, it's incidental.
The only module that is of relevance to Custom Elements is the ContactListElement one.

index is main module for the package and it exports it.

Register the custom element and add it to the page.

Open ContactListElement, quick explanation.

	The store/actions/flux imports are solely for the model wiring, free to ignore them.
	The Element imports its own CSS and template using a SystemJS plugin, keeping templates modular and not adding them to the global id scope.
	No HTML plugin yet but should be easy to make, hence why text is used for template.
	Module exports a class that extends HTMLElement.
	The four commented methods are the Custom Element callbacks, logging allows to test them (add attribute, remove node).
	Receiving attached/detached callbacks means browser helps keep track of lifecycle/resources.

The state member variable holds the information that needs to be added to the DOM, it's an ES6 Map.
It's keys are usernames and values are a objects with keys "status" (value of "online" or "offline") and
"imageSource" which is a URL for a user avatar.

render is the function that's called whenever the state changes. For simplicity you can discard the entire DOM everytime.

Map has a forEach for iteration.

A Custom Element is just a HTML element so standard DOM methods will work on it.

Once the list of contacts is displayed you can use the template to create the "Contacts" header and a container for the contacts.
The contacts should be added to the container.

Add a click event listener to the custom element, use arrow functions to minimize boiler plate for the callback.

Use destructuring to extract all the details from the event. Destructuring works to multiple levels.
