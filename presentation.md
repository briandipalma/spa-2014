Some ideas to add to the presentation slides.

"
Modules are dependency managed JavaScript files, while packages are collections of modules along with other assets.

The only way to achieve true modularity is by having complete dependency management.
"

Make all resource loading clear and explicit.

"
As for HTML imports, the issues with them are all because they use URLs, while the module system uses a more flexible naming mechanism.

If I want to load another HTML import from within an HTML import, I have to assume I know the full URL to that other HTML import. If a relative URL is used, one has to always use backtracking and a lot of naming conventions just to be able to know how to reference another import.

This is causing issues even before considering version management.

The module system offers two ways of solving this problem:

    Dependencies can be referred to by name. I only need to have a dependency called jquery.
    Dependencies can be remapped at runtime. I can specify which URL to use for jquery, even having it resulting in different URLs based on the module that it is required from. So two modules can potentially get two versions of the same require to jquery, allowing version forking (which as dependency trees grow, may be useful one day).

"


Before/after slide showing Custom Element with classes and with ES5.