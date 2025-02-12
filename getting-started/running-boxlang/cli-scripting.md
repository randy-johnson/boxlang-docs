---
icon: rectangle-terminal
description: The core runtime allows you to build CLI scripting applications
---

# CLI Scripting

<figure><img src="../../.gitbook/assets/BL-CLI.png" alt=""><figcaption></figcaption></figure>

BoxLang is a modern, dynamic scripting language built for more than just simple automation—it empowers you to create full-fledged, high-performance CLI applications with ease. Designed to run seamlessly on the JVM, BoxLang provides powerful scripting capabilities, a rich standard library, and first-class support for modular development.&#x20;

Whether you're automating repetitive tasks, building interactive command-line tools, or developing complex CLI-driven workflows, BoxLang offers the flexibility, expressiveness, and performance you need. With intuitive syntax, robust error handling, and seamless integration with Java and other JVM-based technologies, BoxLang makes CLI scripting more efficient and enjoyable.

## Script Files <a href="#execute-a-file-9" id="execute-a-file-9"></a>

With BoxLang, you can execute a few types of files right from any OS CLI by adding them as the second argument to our `boxlang`binary:

<table><thead><tr><th width="100">File</th><th width="100">OS<select><option value="3cJlLiaj5Xge" label="Windows" color="blue"></option><option value="HW3jPH5fCWIy" label="Mac + *Nix" color="blue"></option><option value="6Tg36nA4Yujw" label="All" color="blue"></option></select></th><th>Hint</th></tr></thead><tbody><tr><td>*.bx</td><td><span data-option="6Tg36nA4Yujw">All</span></td><td>BoxLang classes with a <code>main()</code>method</td></tr><tr><td>*.bxs</td><td><span data-option="6Tg36nA4Yujw">All</span></td><td>BoxLang scripts</td></tr><tr><td>*.bxm</td><td><span data-option="6Tg36nA4Yujw">All</span></td><td>BoxLang Templating Language</td></tr><tr><td>*.cfs</td><td><span data-option="6Tg36nA4Yujw">All</span></td><td>CFML scripts (If using the <code>bx-compat-cfml</code>module)</td></tr><tr><td>*.cfm</td><td><span data-option="6Tg36nA4Yujw">All</span></td><td>CFML templates (If using the <code>bx-compat-cfml</code>module)</td></tr><tr><td>*.sh</td><td><span data-option="HW3jPH5fCWIy">Mac + *Nix</span></td><td>Shebang scripts using <code>boxlang</code>as the environment</td></tr></tbody></table>

{% hint style="danger" %}
Please note that you will need the `bx-compat-cfml`module if you want to execute CFML scripts
{% endhint %}

Here are some examples of executing the files.  Just pass in the file by relative or absolute path location.

{% tabs %}
{% tab title="Mac / *Unix" %}
```bash
boxlang task.bx
boxlang myscript.bxs
boxlang mytemplate.bxm

boxlang /full/path/to/test.bxs
boxlang /full/path/to/Task.bx
```
{% endtab %}

{% tab title="Windows" %}
```powershell
boxlang.bat task.bx
boxlang.bat myscript.bxs
boxlang.bat mytemplate.bxm
```
{% endtab %}

{% tab title="Jar" %}
```ruby
java -jar boxlang-1.0.0-all.jar task.bx
java -jar boxlang-1.0.0-all.jar /full/path/to/test.bxs
```
{% endtab %}
{% endtabs %}

## Server CLI Scope

BoxLang has the concept of [variables scopes](../../boxlang-language/variable-scopes.md) that are backed by Java maps we lovingly call structs.  They are fast, case-insensitive, fluent and functional.  BoxLang creates a `server`scope that you can access from any executing file, and lives for the life-span of the application.  This scope contains many useful variables but it can also be used to store any simple/complex variables that you see fit that will be persisted until the application is shutdown or exist.

```java
// Create a variable in the server scope
server.appCreated = now()

// Use a variable in the scope
var inCLIMode = server.boxlang.cliMode
```

The `server` scope is composed of the following internal structures of information:

<table><thead><tr><th width="206">Key</th><th>Hint</th></tr></thead><tbody><tr><td><code>boxlang</code></td><td>A structure containing the runtime information about the language and execution mode.</td></tr><tr><td><code>os</code></td><td>Information about the running operating system</td></tr><tr><td><code>separator</code></td><td>Information about the separators used in the operating system</td></tr><tr><td><code>cli</code></td><td>A collection of variables pertaining to the CLI execution (if any) of the application</td></tr><tr><td><code>java</code></td><td>Information about the running JRE/JDK</td></tr><tr><td><code>system</code></td><td>It contains all the Java properties and System environment variables used to execute the application.</td></tr></tbody></table>

Let's investigate each struct of data because they contain much information about CLI-based scripting and apps.

### boxlang

The following variables are in this structure.

<table><thead><tr><th width="205">Variable</th><th width="100"><select><option value="M25oLpjbfLeo" label="String" color="blue"></option><option value="FKu4P8Bcnsat" label="Number" color="blue"></option><option value="3aeCl3Vi6Hn0" label="Boolean" color="blue"></option><option value="aGop9y9MZCRu" label="Timestamp" color="blue"></option><option value="aC32fOXQJVht" label="Path" color="blue"></option></select></th><th>Hint</th></tr></thead><tbody><tr><td><code>buildDate</code></td><td><span data-option="aGop9y9MZCRu">Timestamp</span></td><td>The build date of BoxLang</td></tr><tr><td><code>boxLangID</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The unique identifier of the current installation</td></tr><tr><td><code>codename</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The codename of the version of BoxLang</td></tr><tr><td><code>cliMode</code></td><td><span data-option="3aeCl3Vi6Hn0">Boolean</span></td><td>True if we are in CLI running mode, else false.</td></tr><tr><td><code>debugMode</code></td><td><span data-option="3aeCl3Vi6Hn0">Boolean</span></td><td>True if we are in debug mode or not.</td></tr><tr><td><code>jarMode</code></td><td><span data-option="3aeCl3Vi6Hn0">Boolean</span></td><td>True if we are running the language from a JAR or an exploded source.</td></tr><tr><td><code>runtimeHome</code></td><td><span data-option="aC32fOXQJVht">Path</span></td><td>The absolute path to the runtime's home directory</td></tr><tr><td><code>version</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The semantic version of the language</td></tr></tbody></table>

### OS

The following variables are in this structure.

<table><thead><tr><th width="210">Variable</th><th width="100"><select><option value="M25oLpjbfLeo" label="String" color="blue"></option><option value="FKu4P8Bcnsat" label="Number" color="blue"></option><option value="3aeCl3Vi6Hn0" label="Boolean" color="blue"></option><option value="aGop9y9MZCRu" label="Timestamp" color="blue"></option><option value="aC32fOXQJVht" label="Path" color="blue"></option></select></th><th>Hint</th></tr></thead><tbody><tr><td><code>arch</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>OS Architecture</td></tr><tr><td><code>archModel</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>OS Architecture model</td></tr><tr><td><code>hostname</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The machine's hostname</td></tr><tr><td><code>ipAddress</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The machine's IP Address</td></tr><tr><td><code>macAddress</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The machine's mac address</td></tr><tr><td><code>name</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>Operating system name</td></tr><tr><td><code>version</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>Operating system version</td></tr></tbody></table>

### separator

The following variables are in this structure.

<table><thead><tr><th width="220">Variable</th><th width="100"><select><option value="M25oLpjbfLeo" label="String" color="blue"></option><option value="FKu4P8Bcnsat" label="Number" color="blue"></option><option value="3aeCl3Vi6Hn0" label="Boolean" color="blue"></option><option value="aGop9y9MZCRu" label="Timestamp" color="blue"></option><option value="aC32fOXQJVht" label="Path" color="blue"></option></select></th><th>Hint</th></tr></thead><tbody><tr><td><code>path</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>Path character separator</td></tr><tr><td><code>file</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>File path character separator</td></tr><tr><td><code>line</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>Line character separator</td></tr></tbody></table>

### CLI

The following variables are in this structure.

<table><thead><tr><th width="216">Variable</th><th width="100"><select><option value="M25oLpjbfLeo" label="String" color="blue"></option><option value="FKu4P8Bcnsat" label="Number" color="blue"></option><option value="3aeCl3Vi6Hn0" label="Boolean" color="blue"></option><option value="aGop9y9MZCRu" label="Timestamp" color="blue"></option><option value="aC32fOXQJVht" label="Path" color="blue"></option><option value="NsqJAGMFv1VJ" label="Struct" color="blue"></option></select></th><th>Hint</th></tr></thead><tbody><tr><td><code>executionPath</code></td><td><span data-option="aC32fOXQJVht">Path</span></td><td>The absolute path from which the execution of the CLI application started.  Maps to the Java <code>user.dir</code>property</td></tr><tr><td><code>command</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The full commandline used to execute</td></tr><tr><td><code>args</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The original command line arguments used in the execution</td></tr><tr><td><code>parsed</code></td><td><span data-option="NsqJAGMFv1VJ">Struct</span></td><td>A structure containing the parsed command line arguments: <code>positionals</code>and <code>options</code></td></tr></tbody></table>

### Java

The following variables are in this structure.

<table><thead><tr><th width="216">Variable</th><th width="100"><select><option value="M25oLpjbfLeo" label="String" color="blue"></option><option value="FKu4P8Bcnsat" label="Number" color="blue"></option><option value="3aeCl3Vi6Hn0" label="Boolean" color="blue"></option><option value="aGop9y9MZCRu" label="Timestamp" color="blue"></option><option value="aC32fOXQJVht" label="Path" color="blue"></option><option value="NsqJAGMFv1VJ" label="Struct" color="blue"></option></select></th><th>Hint</th></tr></thead><tbody><tr><td><code>executionPath</code></td><td><span data-option="aC32fOXQJVht">Path</span></td><td>The absolute path from which the execution of the CLI application started.  Maps to the Java <code>user.dir</code>property</td></tr><tr><td><code>freeMemory</code></td><td><span data-option="FKu4P8Bcnsat">Number</span></td><td>Free JVM memory</td></tr><tr><td><code>maxMemory</code></td><td><span data-option="FKu4P8Bcnsat">Number</span></td><td>Max JVM memory</td></tr><tr><td><code>totalMemory</code></td><td><span data-option="FKu4P8Bcnsat">Number</span></td><td>Total JVM memory</td></tr><tr><td><code>vendor</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>JDK Vendor</td></tr><tr><td><code>version</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>JDK Version</td></tr></tbody></table>



### System

The following variables are in this structure.

<table><thead><tr><th width="216">Variable</th><th width="100"><select><option value="M25oLpjbfLeo" label="String" color="blue"></option><option value="FKu4P8Bcnsat" label="Number" color="blue"></option><option value="3aeCl3Vi6Hn0" label="Boolean" color="blue"></option><option value="aGop9y9MZCRu" label="Timestamp" color="blue"></option><option value="aC32fOXQJVht" label="Path" color="blue"></option><option value="NsqJAGMFv1VJ" label="Struct" color="blue"></option></select></th><th>Hint</th></tr></thead><tbody><tr><td><code>environment</code></td><td><span data-option="NsqJAGMFv1VJ">Struct</span></td><td>A struct of ALL the environment vairables found when creating the runtime.</td></tr><tr><td><code>properties</code></td><td><span data-option="NsqJAGMFv1VJ">Struct</span></td><td>All the Java properties</td></tr></tbody></table>



## Other Scopes

Please note that you have access to other persistent scopes when building CLI applications:

* `application`- This scope lives as long as your application lives as well, but tecnically attached to an `Application.bx`file that activates framework capabilities for your application.
* `request`- A scope that is matched to a specific request to your application. In our case, we also just get one per CLI app since there is no concept of sessions or user state. There is always only one request.

For CLI applications we recommend you use the `server`or `request` scope for singleton persistence.  Also note that you can use all the [caches](../configuration/caches.md) as well for persistence.  You can use `application`scope if you have an `Application.bx.`

## Executing Classes

BoxLang allows you to execute any `*.bx`class as long as it has a method called `main()`by convention.  All the arguments passed into the file execution will be collected and passed in to the function via the `args`argument.

{% code title="task.bx" %}
```java
class{

    function main( args = [] ){
        println( "Hola from my task! #now()#" )
        println( "The passed args are: " )
        println( args )
    }

}
```
{% endcode %}

The `args`argument is an array and it will contain all the arguments passed to the execution of the class.

```bash
boxlang task.bx hola --many options=test
```

If you execute this function above, the output will be:

```bash
Hola from my task! {ts '2025-02-11 22:15:44'}
The passed args are: 
[
  hello,
  luis,
  from=test
]
```

Class executions are a great way to build tasks that have a deterministic approach to execution.  We parse for you the arguments and you can focus on building your task.

## Executing Scripts / Templates

As well as as executing classes you can execute `*.bxs`scripts that can do your bidding.  The difference is that this is a flat script of source code that executes from the top down.  It can contain functions, scope usage, imports and create any class.

{% code title="hello.bxs" %}
```groovy
message = "Hola from my task! #now()#" 
println( message )
println( "The passed args are: " )
println( CLIGetArgs() )
```
{% endcode %}

Then if we execute it, we can see this output:

```bash
╰─ boxlang hello.bxs hola luis=majano --test

Hola from my task! {ts '2025-02-11 22:29:44'}
The passed args are: 
{
  positionals : [
      hola,
    luis=majano
  ],
  options : {
    test : true
  }
}
```

What do you see that's different? Well, we don't have the incoming arguments as an argument since it's a script.  However, we can use the `CLIGetArgs()`BIF and it will give you a struct of two keys:

* `positionals`- An array of positional values passed to the script
* `options`- Name value pairs detected as options

{% hint style="info" %}
You can also get the arguments via the `server.cli.parsed`variable.
{% endhint %}

```groovy
message = "Hola from my task! #now()#" 
println( message )
println( "The passed args are: " )
println( server.cli.parsed )
```

Here is the output:

```bash
╰─ boxlang hello.bxs hola luis=majano --test

Hola from my task! {ts '2025-02-11 22:29:44'}
The passed args are: 
{
  positionals : [
      hola,
    luis=majano
  ],
  options : {
    test : true
  }
}
```

{% hint style="warning" %}
Please note that executing templates is exactly the same as scripts but your template is using the templating language instead.
{% endhint %}



## SheBang Scripts

SheBang scripts are text files containing a sequence of commands for a computer operating system. The term "shebang" refers to the `#!` characters at the beginning of the script, which specify the interpreter that should be used to execute the script. These scripts are commonly used in Unix-like operating systems to automate tasks. By using a shebang line, you can run scripts directly from the command line without needing to invoke the interpreter explicitly.  BoxLang supports these types of scripts, so the OS sees them as just pure shell scripts, but you are really coding in BoxLang scripting.

{% hint style="success" %}
A SheBang script is just basically a `*.bxs`script.
{% endhint %}

{% code title="hola.sh" %}
```bash
#!/usr/bin/env boxlang

println( "Hello World! #now()#" )
println( CLIGetArgs() );
```
{% endcode %}

As you can see from the sample above, the first line is what makes it a SheBang script the operating system can use.  It passes it to the `boxlang`binary for interpretation.  Also, note that you can pass arguments to these scripts like any other script and the `CLIGetArgs()`or the `server.cli.parsed` variables will be there for you to use.

```bash
# Execute the script
./hola.sh

# Execute it with a name argument and a simple option
./hola.sh --name=luis -d
```



## CLI Built-In Functions

BoxLang also gives you several built-in functions for interacting with the CLI:

* `CLIGetArgs():struct` - Return a structure of the parsed incoming arguments
* `CLIRead( [prompt] ):any`- Read input from the CLI and return the value
* `CLIExit( [exitCode=0] )`- Do a `System.exit()`with the passed-in exit code

{% hint style="warning" %}
Please note that you have a wealth of built-in functions and components that you can use to build your scripts.
{% endhint %}



## Parsed Arguments



## Reading Input

You can easily read input from users by using our handy `CLIRead()`bif.  You can also pass in a `prompt`as part of the method call.

```groovy
var exit = cliRead( "Do you want to continue? (Y/N)" ).trueFalseFormat()
if( exit ){
  cliExit()
}
```



## Producing Output

As you navigate all the built-in functions and capabilities of BoxLang, let's learn how to produce output to the system console.

* `printLn()` - Print with a line break to System out
* `print()` - Print with no line break to System out
* `writeOutput(), echo()` - Writes to the output buffer (Each runtime decides what its buffer is. The CLI is the system output, the Web is the HTML response buffer, etc)
* `writeDump()`- Takes any incoming output and will serialize to a nice string output representation.  This will also do complex objects deeply.

```groovy
println( "Time is #now()#" )
```

I get the output:

```bash
╰─ boxlang test.bxs
Time is {ts '2024-05-22 22:09:56'}
```

Hooray! You have executed your first script using BoxLang. Now let's build a class with a `main( args=[] )` convention. This is similar to Java or Groovy.

```java
class{

        function main( args=[] ){
        
               println( "Task called with " & arguments.toString() )
                
                writedump( args )
        
        }

}
```

You can now call it with zero or more arguments!

```bash
╰─ boxlang Task.bx
Task called with {ARGS=[]}

╰─ boxlang Task.bx boxlang rocks
Task called with {ARGS=[boxlang, rocks]}
```



## Piping code <a href="#piping-code-11" id="piping-code-11"></a>

You can also pipe statements into the BoxLang binary for execution as well. This assumes script, not tags.

```bash
echo "2+2" | java -jar boxlang-1.0.0-all.jar
echo "2+2" | boxlang
```

or

```bash
# on *nix
cat test.cfs | java -jar boxlang-1.0.0-all.jar
cat test.cfs | boxlang

# on Windows
type test.cfs | java -jar boxlang-1.0.0-all.jar
type test.cfs | boxlang.bat
```



## Dad Joke Script

Thanks to our evangelist Raymond Camden, we have a cool dad joke script you can find in our demos: [https://github.com/ortus-boxlang/bx-demos](https://github.com/ortus-boxlang/bx-demos)

```java
class{
    variables.apiURL = "https://icanhazdadjoke.com/";

    /**
     * The first argument is the term in this scenario
     * Example: DadJoke.bx dad
     * Example: DadJoke.bx java
     * Example: DadJoke.bx
     */
    function main( args = [] ) {
        var term = args[ 1 ] ?: "";

        if( !term.isEmpty() ){
            apiURL &= "search?term=" & urlEncodedFormat( term )
        }

        println( "Getting dad joke for term [#term#], please wait..." );
        bx:http url=apiURL result="local.result" {
            bx:httpparam type="header" name="Accept" value="application/json";
        };

        var data = JSONDeserialize( result.fileContent );
        //println( data )

         // possible none were found
         if( data?.results?.len() == 0 ){
            println( "No jokes found for term: #term#" );
            return cliExit();
         }

        // If we searched for a term, we need to get a random joke from the results, otherwise, just .joke
        var joke = term.isEmpty() ? data.joke : data.results[ randRange( 1, data.results.len() ) ].joke;
        println( joke );
    }

}
```

Now you execute it

```bash
// Random joke
boxlang DadJoke.bx

// Term jokes
boxlang DadJoke.bx ice
```
