The featherweight JavaScript meta-framework which hooks into any major JavaScript library to make scoped event delegation and modular encapsulation automatic and painless.

# Examples

## Scoping

The below code scopes everything within the function to the .hello-world namespace within the DOM.

Eve then binds two useful methods to the function: listen and attach. These methods ensure that all actions within the function are scoped to the parent (.hello-world) DOM namespace.

	Eve.scope('.hello-world', function() {

	    this.listen('div.line', 'click', function(e) {
	        console.log("You clicked on .hello-world div.line");
	    });

	});

## Reusable Modules

Eve allows you to create reusable modules which you can then attach to any scoped function or CSS namespace.

### Scoping to a CSS Namespace

	Eve.register('myAwesomePlugin', function() {

	    this.listen('a', 'click', function() {
	        console.log("You clicked on a link within my namespace!");
	    });

	});

	Eve.attach('myAwesomePlugin', '#section-of-site');
	
### Recursive Scoping to a Scope

	Eve.register('myAwesomePlugin', function() {

	    this.listen('a', 'click', function() {
	        console.log("You clicked on a link within my namespace!");
	    });

	});

	Eve.scope('#section-of-my-site', function() {

	    //This will attach the named module to the namespace of
	    //"#section-of-my-site .subsection".
	    this.attach('myAwesomePlugin', '.subsection');

	});
	
# Documentation

## Global Methods

### Eve.scope

Scopes the given function to a particular CSS namespace. Eve attaches listen and attach methods to each scoped function. When calling these methods from within the function, you'll automatically remain within the scoped namespace.

#### Syntax

Eve.scope(namespace, function);

#### Arguments

- namespace (string): The CSS selector of the elements which will be effected by the provided function.
- function (function): This function will be responsible for all code related to the given CSS namespace.

#### Example

	Eve.scope('.hello-world', function() {

	    this.listen('div.line', 'click', function(e) {
	        console.log("You clicked on .hello-world div.line");
	    });

	});
	
### Eve.register

Allows you to register a reusable module which can then be utilized via the attach method. Module functions have no default namespace of their own, and instead will rely on the namespace provided when attaching.

#### Syntax

Eve.register(moduleName, function);

#### Arguments

- moduleName (string): The name of the module. This can be anything, as long as it's unique.
- function (function): This function will be run within a scoped namespace when attached. Note: this function will only run ONCE per namespace.

#### Example

	Eve.register('myAwesomePlugin', function() {

	    this.listen('a', 'click', function() {
	        console.log("You clicked on a link within my namespace!");
	    });

	    //Since no namespace is provided, this event will be triggered by
	    //any click events within the scoped namespace.
	    this.listen('click', function() {
	        alert("You clicked somewhere within my namespace!");
	    });

	});


	//Now, we're giving the module a scoped namespace to work with,
	//effectively attaching events to "#section-of-my-site" and
	//"#section-of-my-site a".
	Eve.attach('myAwesomePlugin', '#section-of-site');
	
### Eve.attach

Attaches a previously registered module to the specified CSS namespace.

#### Syntax

Eve.attach(moduleName, namespace);

#### Arguments

- moduleName (string): The name of the module to attach. It must have previously been registered using Eve.register.
- namespace (string): The CSS selector of the elements which will be effected by the named module.

#### Example

	Eve.register('myAwesomePlugin', function() {

	    this.listen('a', 'click', function() {
	        console.log("You clicked on a link within my namespace!");
	    });

	});

	Eve.attach('myAwesomePlugin', '#section-of-site');
	
### Eve.debug

Logs detailed information concerning event execution and responsible modules to to the console.

#### Syntax

Eve.debug([moduleName]);

#### Arguments

- moduleName (optional): If a module name is provided, only events the given module is listening to will be logged. If no moduleName is provided, all events will be logged to the console.

#### Example

    Eve.debug('myAwesomePlugin');

## Scoped Methods

Eve attaches two methods to each scoped function attached via Eve.scope or Eve.register. When calling these methods from within the function, you'll automatically remain within the scoped namespace.

### listen

Begins watching for events which match the particular eventType and CSS selector, as long as the given CSS selector is part of the parent namespace.

#### Syntax

	this.listen([selector,] eventType, handler);

#### Arguments

- selector (string, optional): The CSS selector of the elements you want to attach events to. If no selector is given, any event of the given type within the scoped namespace will invoke the handler.
- eventType (string): The event type, such as click or mouseover.
handler (function): The function to call when the specified event type has taken place on a matching element.

#### Example

	Eve.register('myAwesomePlugin', function() {

	    this.listen('a', 'click', function() {
	        console.log("You clicked on a link within my namespace!");
	    });

	});

	Eve.scope('#section-of-my-site', function() {

	    //This will attach the named module to the namespace of
	    //"#section-of-my-site .subsection".
	    this.attach('myAwesomePlugin', '.subsection');

	});
	
### attach

Works exactly like Eve.attach, but will confine the attached module to the current scope.

#### Example

	Eve.register('myAwesomePlugin', function() {

	    this.listen('a', 'click', function() {
	        console.log("You clicked on a link within my namespace!");
	    });

	});

	Eve.scope('#section-of-my-site', function() {

	    //This will attach the named module to the namespace of
	    //"#section-of-my-site .subsection".
	    this.attach('myAwesomePlugin', '.subsection');

	});