---
description: They gotta exist somewhere!
---

# Variable Scopes

In the BoxLang language, many persistence and visibility scopes exist for variables to be placed. These are differentiated by context: in a class, function, tag module, thread, or template.&#x20;

{% hint style="info" %}
All BoxLang scopes are implemented as BoxLang [structures](structures.md), basically case-insensitive concurrent hash maps behind the scenes.&#x20;
{% endhint %}

This idea that the variables you declare in templates, classes, and functions are stored in a structure makes it highly flexible since you can interact with the entire scope fluently and with many different BIFs available.  You can also bind them to function calls, attributes, etc.  Thus, it capitulates on being a dynamic language.

```javascript
// Examples

/**
 * Get the state representation of the scheduler task
 */
function getMemento(){
	// I can do a filter on an entire scope
	return variables.filter( ( key, value ) => {
		return isCustomFunction( value ) || listFindNoCase( "this", key ) ? false : true;
	} );
}


// Argument binding
results = matchClassRules( argumentCollection = arguments );
results = matchClassRules( argumentCollection = myMap );

// I can dump entire scopes
writedump( variables )

```

The only language keyword that can be used to tell the variable to store in a function's local scope is called `var`.  However, you can also just use the `local`scope directly or none at all. However, we do recommend being explicit on some occasions to avoid ambiguity.&#x20;

{% hint style="warning" %}
It's a good idea to scope variables to avoid scope lookups, which could in turn create issues or even leaks.
{% endhint %}



```javascript
function getData(){
	// Placed in the function's local scope
	var data = "luis"
	// Also placed in the function's local scope
	data = "luis"
	// Also placed in the function's local scope
	local.data = "luis"
}
```

## Scripts & Template Scopes (bxm,bxs)

All scripts and templates have the following scopes available to them.  Please note that the `variables`scope can also be implicit.  You don't have to declare it.

* `variables` - The default or implicit scope to which all variables are assigned.

```javascript
a = "hello"
writeOutput( a )

// Is the same as
variables.a = "hello"
writeOutput( variables.a )
```

## Class Scopes (bx)

All classes in BoxLang follow Object-oriented patterns and have the following scopes available.  Another critical aspect of classes is that all declared functions will be placed in a visibility scope.

* `variables` - Private scope, visible internally to the class only
* `this` - Public scope, visible from the outside world
* `static` - Store variables in the classes blueprint and not the instance
* `super`- Only available if you use inheritance

```java
class extends="MyParent"{
    
    static {
        className = "MyClass"
    }
    
    variables.created = now()
    this.PUBLIC = "Hola"

    function init(){
        super.init()
    }
    
    function main(){
        println( "hello #static.className#" )
    }

}
```

### Function Assignment Scopes

Depending on the function's visibility, BoxLang places a pointer for the function in the different scopes below.  Why? Because BoxLang is a dynamic language. Meaning at runtime, you can add/remove/modify functions if you want to.

* `Private` Function
  * `variables`
* `Public or Remote` Functions
  * `variables`
  * `this`

```java
// Try it out
class {

    function main(){
        writedump( var: this, label: "public" );
        writedump( var: variables, label: "private" );
    }
    
    private function test(){}
    public function hello(){}

}
```

## Function Scopes

All user-defined functions will have the following scopes available:

* `variables` - Has access to private variables within a Class or Page
* `this` - Has access to public variables within a Class or Page
* `local` - Function-scoped variables only exist within the function execution. Referred to as `var` scoping. The default assignment scope in a function.
* `arguments` - Incoming variables to a function

### Closure Scopes

All closures are context-aware. Meaning they know about their birthplace and surrounding scopes.&#x20;

* `variables` - Has access to private variables from where they where created
* `this` - Has access to public variables from where they where created
* `local` - Function-scoped variables only exist within the function execution. Referred to as `var` scoping. The default assignment scope in a function.
* `arguments` - Incoming variables to a function

### Lambdas (Pure Function) Scopes

Lambdas&#x20;

## Tag Scopes

* `attributes` - Incoming tag attributes
* `variables` - The default scope for variable assignments
* `caller` - Used within a custom tag to set or read variables within the template that called it.

## Thread Scopes

* `attributes` - Passed variables via a thread
* `thread` - A thread specific scope that can be used for storage and retrieval
* `local` - Variables local to the thread context

## **Evaluating Unscoped Variables**

If you use a variable name **without** a scope prefix, BoxLang checks the scopes in the following order to find the variable:

1. Local (function-local, UDFs and Classes only)
2. Arguments
3. Thread local (inside threads only)
4. Query (not a true scope; variables in query loops)
5. Thread
6. Variables
7. CGI
8. CFFILE
9. URL
10. Form
11. Cookie
12. Client

{% hint style="danger" %}
**IMPORTANT**: Because BoxLang must search for variables when you do not specify the scope, you can improve performance by specifying the scope for all variables. It can also help you avoid nasty lookups or unexpected results.
{% endhint %}

## Persistence Scopes

Can be used in any context, used for persisting variables for a period of time.

* `session` - stored in server RAM or external storages tracked by unique web visitor
* `client` - stored in cookies, databases, or external storages (simple values only)
* `application` - stored in server RAM or external storage tracked by the running BoxLang application
* `cookie` - stored in a visitor's browser
* `server` - stored in server RAM for ANY application for that BoxLang instance
* `request` - stored in RAM for a specific user request ONLY
* `cgi` - read only scope provided by the servlet container and BoxLang
* `form` - Variables submitted via HTTP posts
* `URL` - Variables incoming via HTTP GET operations or the incoming URL
