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

<table><thead><tr><th width="100">Key</th><th>Hint</th></tr></thead><tbody><tr><td><code>boxlang</code></td><td>A structure containing the runtime information about the language and execution mode.</td></tr><tr><td><code>os</code></td><td>Information about the running operating system</td></tr><tr><td><code>separator</code></td><td>Information about the separators used in the operating system</td></tr><tr><td><code>cli</code></td><td>A collection of variables pertaining to the CLI execution (if any) of the application</td></tr><tr><td><code>java</code></td><td>Information about the running JRE/JDK</td></tr><tr><td><code>system</code></td><td>It contains all the Java properties and System environment variables used to execute the application.</td></tr></tbody></table>

Let's investigate each struct of data because they contain much information about CLI-based scripting and apps.

### boxlang

The following variables are in this structure.

<table><thead><tr><th width="175">Variable</th><th width="100"><select><option value="M25oLpjbfLeo" label="String" color="blue"></option><option value="FKu4P8Bcnsat" label="Number" color="blue"></option><option value="3aeCl3Vi6Hn0" label="Boolean" color="blue"></option><option value="aGop9y9MZCRu" label="Timestamp" color="blue"></option><option value="aC32fOXQJVht" label="Path" color="blue"></option></select></th><th>Hint</th></tr></thead><tbody><tr><td><code>buildDate</code></td><td><span data-option="aGop9y9MZCRu">Timestamp</span></td><td>The build date of BoxLang</td></tr><tr><td><code>boxLangID</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The unique identifier of the current installation</td></tr><tr><td><code>codename</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The codename of the version of BoxLang</td></tr><tr><td><code>cliMode</code></td><td><span data-option="3aeCl3Vi6Hn0">Boolean</span></td><td>True if we are in CLI running mode, else false.</td></tr><tr><td><code>debugMode</code></td><td><span data-option="3aeCl3Vi6Hn0">Boolean</span></td><td>True if we are in debug mode or not.</td></tr><tr><td><code>jarMode</code></td><td><span data-option="3aeCl3Vi6Hn0">Boolean</span></td><td>True if we are running the language from a JAR or an exploded source.</td></tr><tr><td><code>runtimeHome</code></td><td><span data-option="aC32fOXQJVht">Path</span></td><td>The absolute path to the runtime's home directory</td></tr><tr><td><code>version</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The semantic version of the language</td></tr></tbody></table>

### OS

The following variables are in this structure.

<table><thead><tr><th>Variable</th><th width="100"><select><option value="M25oLpjbfLeo" label="String" color="blue"></option><option value="FKu4P8Bcnsat" label="Number" color="blue"></option><option value="3aeCl3Vi6Hn0" label="Boolean" color="blue"></option><option value="aGop9y9MZCRu" label="Timestamp" color="blue"></option><option value="aC32fOXQJVht" label="Path" color="blue"></option></select></th><th>Hint</th></tr></thead><tbody><tr><td><code>arch</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>OS Architecture</td></tr><tr><td><code>archModel</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>OS Architecture model</td></tr><tr><td><code>hostname</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The machine's hostname</td></tr><tr><td><code>ipAddress</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The machine's IP Address</td></tr><tr><td><code>macAddress</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The machine's mac address</td></tr><tr><td><code>name</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>Operating system name</td></tr><tr><td><code>version</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>Operating system version</td></tr></tbody></table>

### separator

The following variables are in this structure.

<table><thead><tr><th>Variable</th><th width="100"><select><option value="M25oLpjbfLeo" label="String" color="blue"></option><option value="FKu4P8Bcnsat" label="Number" color="blue"></option><option value="3aeCl3Vi6Hn0" label="Boolean" color="blue"></option><option value="aGop9y9MZCRu" label="Timestamp" color="blue"></option><option value="aC32fOXQJVht" label="Path" color="blue"></option></select></th><th>Hint</th></tr></thead><tbody><tr><td><code>path</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>Path character separator</td></tr><tr><td><code>file</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>File path character separator</td></tr><tr><td><code>line</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>Line character separator</td></tr></tbody></table>

### CLI

The following variables are in this structure.

<table><thead><tr><th width="216">Variable</th><th width="100"><select><option value="M25oLpjbfLeo" label="String" color="blue"></option><option value="FKu4P8Bcnsat" label="Number" color="blue"></option><option value="3aeCl3Vi6Hn0" label="Boolean" color="blue"></option><option value="aGop9y9MZCRu" label="Timestamp" color="blue"></option><option value="aC32fOXQJVht" label="Path" color="blue"></option><option value="NsqJAGMFv1VJ" label="Struct" color="blue"></option></select></th><th>Hint</th></tr></thead><tbody><tr><td><code>executionPath</code></td><td><span data-option="aC32fOXQJVht">Path</span></td><td>The absolute path from which the execution of the CLI application started.  Maps to the Java <code>user.dir</code>property</td></tr><tr><td><code>command</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The full commandline used to execute</td></tr><tr><td><code>args</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The original command line arguments used in the execution</td></tr><tr><td><code>parsed</code></td><td><span data-option="NsqJAGMFv1VJ">Struct</span></td><td>A structure containing the parsed command line arguments: <code>positionals</code>and <code>options</code></td></tr></tbody></table>

### Java

The following variables are in this structure.

<table><thead><tr><th width="216">Variable</th><th width="100"><select><option value="M25oLpjbfLeo" label="String" color="blue"></option><option value="FKu4P8Bcnsat" label="Number" color="blue"></option><option value="3aeCl3Vi6Hn0" label="Boolean" color="blue"></option><option value="aGop9y9MZCRu" label="Timestamp" color="blue"></option><option value="aC32fOXQJVht" label="Path" color="blue"></option><option value="NsqJAGMFv1VJ" label="Struct" color="blue"></option></select></th><th>Hint</th></tr></thead><tbody><tr><td><code>executionPath</code></td><td><span data-option="aC32fOXQJVht">Path</span></td><td>The absolute path from which the execution of the CLI application started.  Maps to the Java <code>user.dir</code>property</td></tr><tr><td><code>command</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The full commandline used to execute</td></tr><tr><td><code>args</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The original command line arguments used in the execution</td></tr><tr><td><code>parsed</code></td><td><span data-option="NsqJAGMFv1VJ">Struct</span></td><td>A structure containing the parsed command line arguments: <code>positionals</code>and <code>options</code></td></tr></tbody></table>



### System

The following variables are in this structure.

<table><thead><tr><th width="216">Variable</th><th width="100"><select><option value="M25oLpjbfLeo" label="String" color="blue"></option><option value="FKu4P8Bcnsat" label="Number" color="blue"></option><option value="3aeCl3Vi6Hn0" label="Boolean" color="blue"></option><option value="aGop9y9MZCRu" label="Timestamp" color="blue"></option><option value="aC32fOXQJVht" label="Path" color="blue"></option><option value="NsqJAGMFv1VJ" label="Struct" color="blue"></option></select></th><th>Hint</th></tr></thead><tbody><tr><td><code>executionPath</code></td><td><span data-option="aC32fOXQJVht">Path</span></td><td>The absolute path from which the execution of the CLI application started.  Maps to the Java <code>user.dir</code>property</td></tr><tr><td><code>command</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The full commandline used to execute</td></tr><tr><td><code>args</code></td><td><span data-option="M25oLpjbfLeo">String</span></td><td>The original command line arguments used in the execution</td></tr><tr><td><code>parsed</code></td><td><span data-option="NsqJAGMFv1VJ">Struct</span></td><td>A structure containing the parsed command line arguments: <code>positionals</code>and <code>options</code></td></tr></tbody></table>



### Ex

### Executing Classes

BoxLang allows you to execute any `*.bx`class as long as it has a method called `main()`by convention.

```java
class{

    function main( args = [] ){
    
    }

}
```

All the arguments passed into the file execution will be collected and passed in to the function via the `args`argument.

### Producing Output

As you navigate all the built-in functions and capabilities of BoxLang, let's learn how to produce output to the system console.

* `printLn()` - Print with a line break
* `print()` - Print with no line break
* `writeOutput()` - Writes to the output buffer (Each runtime decides what it's buffer is. The CLI is the system output, the Web is the HTML response buffer, etc)

```groovy
println( "Time is #now()#" )
```

I get the output:

```bash
╰─ boxlang test.bxs
Time is {ts '2024-05-22 22:09:56'}
```

Hooray! You have executed your first script using BoxLang. Now let's build a class with a `main( args=[] )` convention. This is simliar to Java or Groovy.

```groovy
class{

        function main( args=[] ){

                println( "Task called with " & arguments.toString() )

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

### Piping code <a href="#piping-code-11" id="piping-code-11"></a>

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
