---
description: They gotta exist somewhere!
---

# Variable Scopes

In the BoxLang language, there are many persistence and visibility scopes that exist for variables to be placed in. These are differentiated by context: in a class, in a function, tag module, thread or in a template.&#x20;

{% hint style="info" %}
All BoxLang scopes are implemented as BoxLang structures which are basically case-insensitive concurrent hash maps behind the scenes.&#x20;
{% endhint %}

This idea that your variables you declare in templates, classes and functions are stored in a structure makes it extremely flexible since you can interact with the entire scope fluently and with many different BIFs available to you.  You can also bind them to function calls, attributes and much more.

```javascript
// Examples

/**
 * Get the state representation of the scheduler task
 */
function getMemento(){
	return variables.filter( ( key, value ) => {
		return isCustomFunction( value ) || listFindNoCase( "this", key ) ? false : true;
	} );
}


// Argument binding
results = matchClassRules( argumentCollection = arguments );
results = matchClassRules( argumentCollection = myMap );

writedump( variables )

```

The only language keyword that can be used to tell the variable to store in a function's local scope is called `var`.  However, you can also just use the `local`scope directly or none at all. However, we do recommend being explicit.

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

All scripts and templates have the following scopes available to it.  Please note that the `variables`scope can also be implicit.  You don't have to declare it.

* `variables` - The default or implicit scope where all variables are assigned to.

```javascript
a = "hello"
writeOutput( a )

or
variables.a = "hello"
writeOutput( variables.a )
```

## Class Scopes (bx)

All clases in BoxLang follow Object Oriented patterns and have the following scopes available to you.  Another important aspect of classes is that all declared functions in a class will be placed also in a visibility scope.

* `variables` - Private scope, visible internally to the class only
* `this` - Public scope, visible from the outside world
* `static` - No need for a class instance, available as a class representation
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

Depending on the function's visbility, BoxLang places a pointer of the function to the different scopes below.  Why? Because BoxLang is dynamic. Meaning at runtime you can add/remove/modify functions if you wanted to.

* Private Function
  * `variables`
* Public Function
  * `variables`
  * `this`

## Function Scopes

* `variables` - Has access to private variables within a Component or Page
* `this` - Has access to public variables within a Component or Page
* `local` - Function scoped variables, only exist within the function execution. Referred to as `var` scoping
* `arguments` - Incoming variables to a function

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
