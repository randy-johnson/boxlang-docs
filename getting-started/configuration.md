---
icon: wrench-simple
description: Configure it your way!
---

# Runtime Configuration

BoxLang has an installation-level configuration file that allows developers to adjust various settings from the compiler to default cache providers, runtime-wide data sources, and much more. You can adjust boxlang settings by placing a `config/boxlang.json` file in your BoxLang home directory.

By default, we create an initial  `config/boxlang.json` for you once you execute the runtime for the first time.

You may also change the granular config settings at runtime using the environment or Java properties by prefixing any configuration item with `BOXLANG_`or `boxlang.`  See below.

{% hint style="info" %}
If you are running BoxLang within CommandBox, the configuration file will be inside the server directory inside of CommandBox under `WEB-INF/boxlang/`. You can also run the following command to see the server home directory:

```
server status property=serverHome
```
{% endhint %}

## Runtime Home Directory

By default, the BoxLang home directory is a `.boxlang/` directory inside your user's home directory. For instance, on a Ubuntu machine, this might be `/home/elpete/.boxlang/` if you are executing BoxLang under the `elpete` user account.

ℹ️ The BoxLang home can be adjusted on startup via a `--home` flag:

```bash
boxlang --home /path/to/boxlang-home
```

By allowing a custom home directory, you can manage multiple BoxLang runtimes and allow:

1. custom, per-runtime configuration
2. a custom set of BoxLang modules
3. etc

## Boxlang.json Reference

Here is a full reference of the current default `boxlang.json` file:

```json
/**
 * BoxLang Configuration File
 *
 * Here are some of the available variables you can use in this file:
 * ${boxlang-home} - The BoxLang home directory
 * ${user-home} - The user's home directory
 * ${user-dir} - The user's current directory
 * ${java-temp} - The java temp directory
 * ${env.variablename:defaultValue} - The value of a valid environment variable or the default value. Example: ${env.CFCONFIG_HOME:/etc/cfconfig}
 */
{
	// Extensions BoxLang will process as classes
	"validClassExtensions": [
		"bx",
		// Moving to compat at final release
		"cfc"
	],
	// Extensions BoxLang will process as templates.
	// This is used by the RunnableLoader
	"validTemplateExtensions": [
		"bxs",
		"bxm",
		"bxml",
		// Moving to compat at final release
		"cfm",
		"cfml",
		"cfs"
	],
	// Where all generated classes will be placed
	"classGenerationDirectory": "${boxlang-home}/classes",
	// This puts the entire runtime in debug mode
	// Which will produce lots of debug output and metrics
	// Also the debugging error template will be used if turned on
	"debugMode": false,
	// The default timezone for the runtime; defaults to the JVM timezone if empty
	// Please use the IANA timezone database values
	"timezone": "",
	// The default locale for the runtime; defaults to the JVM locale if empty
	// Please use the IETF BCP 47 language tag values
	"locale": "",
	// Enable whitespace compression in output.  Only in use by the web runtimes currently.
	"whitespaceCompressionEnabled": true,
	// By default BoxLang uses high-precision mathematics via BigDecimal operations
	// You can turn this off here for all applications
	"useHighPrecisionMath": true,
	// If true, you can call implicit accessors/mutators on object properties. By default it is enabled
	// You can turn it on here for all applications or in the Application.cfc
	"invokeImplicitAccessor": true,
	// Use Timespan syntax: "days, hours, minutes, seconds"
	"applicationTimeout": "0,0,0,0",
	// The request timeout for a request in seconds; 0 means no timeout
	"requestTimeout": "0,0,0,0",
	// The session timeout: 30 minutes
	"sessionTimeout": "0,0,30,0",
	// Where sessions will be stored by default.  This has to be a name of a registered cache
	// or the keyword "memory" to indicate our auto-created cache.
	// This will apply to ALL applications unless overridden in the Application.cfc
	"sessionStorage": "memory",
	// A collection of BoxLang mappings, the key is the prefix and the value is the directory
	"mappings": {
		"/": "${user-dir}"
	},
	// A collection of BoxLang module directories, they must be absolute paths
	"modulesDirectory": [
		"${boxlang-home}/modules"
	],
	// A collection of BoxLang custom tag directories, they must be absolute paths
	"customTagsDirectory": [
		"${boxlang-home}/customTags"
	],
	// A collection of directories we will class load all Java *.jar files from
	"javaLibraryPaths": [
		"${boxlang-home}/lib"
	],
	// Logging Settings for the runtime
	"logging": {
		// The location of the log files the runtime will produce
		"logsDirectory": "${boxlang-home}/logs",
		// The maximum number of days to keep log files before rotation
		// Default is 90 days or 3 months
		// Set to 0 to never rotate
		"maxLogDays": 90,
		// The maximum file size for a single log file before rotation
		// You can use the following suffixes: KB, MB, GB
		// Default is 100MB
		"maxFileSize": "100MB",
		// The total cap size of all log files before rotation
		// You can use the following suffixes: KB, MB, GB
		// Default is 5GB
		"totalCapSize": "5GB",
		// The root logger level
		// Valid values are in order of severity: ERROR, WARN, INFO, DEBUG, TRACE, OFF
		// If the runtime is in Debug mode, this will be set to DEBUG
		"rootLevel": "WARN",
		// Default Encoder for file appenders.
		// The available options are "text" and "json"
		"defaultEncoder": "text",
		// A collection of pre-defined loggers and their configurations
		"loggers": {
			// The runtime main and default log
			"runtime": {
				// Valid values are in order of severity: ERROR, WARN, INFO, DEBUG, TRACE, OFF
				// Leave out if it should inherit from the root logger
				//"level": "WARN",
				// Valid values are: "file", "console",
				// Coming soon: "smtp", "socket", "db", "syslog" or "java class name"
				// Please note that we only use Rolling File Appenders
				"appender": "file",
				// Use the defaults from the runtime
				"appenderArguments": {},
				// The available options are "text" and "json"
				"encoder": "text",
				// Additive logging: true means that this logger will inherit the appenders from the root logger
				// If false, it will only use the appenders defined in this logger
				"additive": true
			},
			// The modules log
			"modules": {
				// Valid values are in order of severity: ERROR, WARN, INFO, DEBUG, TRACE, OFF
				// Leave out if it should inherit from the root logger
				//"level": "WARN",
				// Valid values are: "file", "console",
				// Coming soon: "smtp", "socket", "db", "syslog" or "java class name"
				// Please note that we only use Rolling File Appenders
				"appender": "file",
				// Use the defaults from the runtime
				"appenderArguments": {},
				// The available options are "text" and "json"
				"encoder": "text",
				// Additive logging: true means that this logger will inherit the appenders from the root logger
				// If false, it will only use the appenders defined in this logger
				"additive": true
			},
			// All applications will use this logger
			"application": {
				// Valid values are in order of severity: ERROR, WARN, INFO, DEBUG, TRACE, OFF
				// Leave out if it should inherit from the root logger
				"level": "TRACE",
				// Valid values are: "file", "console",
				// Coming soon: "smtp", "socket", "db", "syslog" or "java class name"
				// Please note that we only use Rolling File Appenders
				"appender": "file",
				// Use the defaults from the runtime
				"appenderArguments": {},
				// The available options are "text" and "json"
				"encoder": "text",
				// Additive logging: true means that this logger will inherit the appenders from the root logger
				// If false, it will only use the appenders defined in this logger
				"additive": true
			},
			// All scheduled tasks logging
			"scheduler": {
				// Valid values are in order of severity: ERROR, WARN, INFO, DEBUG, TRACE, OFF
				// Leave out if it should inherit from the root logger
				"level": "INFO",
				// Valid values are: "file", "console",
				// Coming soon: "smtp", "socket", "db", "syslog" or "java class name"
				// Please note that we only use Rolling File Appenders
				"appender": "file",
				// Use the defaults from the runtime
				"appenderArguments": {},
				// The available options are "text" and "json"
				"encoder": "text",
				// Additive logging: true means that this logger will inherit the appenders from the root logger
				// If false, it will only use the appenders defined in this logger
				"additive": true
			}
		}
	},
	// This is the experimental features flags.
	// Please see the documentation to see which flags are available
	"experimental": {
		// This choose the compiler to use for the runtime
		// Valid values are: "java", "asm"
		"compiler": "java",
		// If enabled, it will generate AST JSON data under the project's /grapher/data folder
		"ASTCapture": false
	},
	// Global Executors for the runtime
	// These are managed by the AsyncService and registered upon startup
	// The name of the executor is the key and the value is a struct of executor settings
	// Types are: cached, fixed, fork_join, scheduled, single, virtual, work_stealing
	// The `threads` property is the number of threads to use in the executor. The default is 20
	// Some executors do not take in a `threads` property
	"executors": {
		"boxlang-tasks": {
			"type": "scheduled",
			"threads": 20
		},
		"cacheservice-tasks": {
			"type": "scheduled",
			"threads": 20
		}
	},
	// You can assign a global default datasource to be used in the language
	"defaultDatasource": "",
	// The registered global datasources in the language
	// The key is the name of the datasource and the value is a struct of the datasource settings
	"datasources": {
		// "testDB": {
		// 	  "driver": "derby",
		//    "connectionString": "jdbc:derby:memory:testDB;create=true"
		// }
		// "testdatasource": {
		// 	  "driver": "derby",
		// 	  "host": "localhost",
		// 	  "port": 3306,
		// 	  "database": "test"
		// }
	},
	// The default return format for class invocations via web runtimes
	"defaultRemoteMethodReturnFormat": "json",
	// The configuration for the BoxLang `default` cache.  If empty, we use the defaults
	// See the ortus.boxlang.runtime.config.segments.CacheConfig for all the available settings
	// This is used by query caching, template caching, and other internal caching.
	// You can use the cache() BIF in order to get access to the default cache.
	"defaultCache": {
		// How many to evict at a time once a policy is triggered
		"evictCount": 1,
		// The eviction policy to use: Least Recently Used
		// Other policies are: LRU, LFU, FIFO, LIFO, RANDOM
		"evictionPolicy": "LRU",
		// The free memory percentage threshold to trigger eviction
		// 0 = disabled, 1-100 = percentage of available free memory in heap
		// If the threadhold is reached, the eviction policy is triggered
		"freeMemoryPercentageThreshold": 0,
		// The maximum number of objects to store in the cache
		"maxObjects": 1000,
		// The maximum in seconds to keep an object in the cache since it's last access
		// So if an object is not accessed in this time or greater, it will be removed from the cache
		"defaultLastAccessTimeout": 1800,
		// The maximum time in seconds to keep an object in the cache regardless if it's used or not
		// A default timeout of 0 = never expire, careful with this setting
		"defaultTimeout": 3600,
		// The object store to use to store the objects.
		// The default is a ConcurrentStore which is a memory sensitive store
		"objectStore": "ConcurrentStore",
		// The frequency in seconds to check for expired objects and expire them using the policy
		// This creates a BoxLang task that runs every X seconds to check for expired objects
		"reapFrequency": 120,
		// If enabled, the last access timeout will be reset on every access
		// This means that the last access timeout will be reset to the defaultLastAccessTimeout on every access
		// Usually for session caches or to simulate a session
		"resetTimeoutOnAccess": false,
		// If enabled, the last access timeout will be used to evict objects from the cache
		"useLastAccessTimeouts": true
	},
	/**
	* Register any named caches here.
	* The key is the name of the cache and the value is the cache configuration.
	*
	* A `provider` property is required and the value is the name of the cache provider or the fully qualified class name.
	* The `properties` property is optional and is a struct of properties that are specific to the cache provider.
	*/
	"caches": {
		// This is the holder of all sessions in a BoxLang runtime.
		// The keys are prefixed by application to create separation.
		"bxSessions": {
			"provider": "BoxCacheProvider",
			"properties": {
				// How many objects to evict when the cache is full
				"evictCount": 1,
				// The eviction policy to use: FIFO, LFU, LIFO, LRU, MFU, MRU, Random
				"evictionPolicy": "LRU",
				// The maximum number of objects the cache can hold
				"maxObjects": 100000,
				// How long should sessions last for in seconds. Default is 60 minutes.
				"defaultTimeout": 3600,
				// The object store to use to store the objects.
				// The default is a ConcurrentStore which is a thread safe and fast storage.
				// Available Stores are: BlackHoleStore, ConcurrentSoftReferenceStore, ConcurrentStore, FileSystemStore, Your own.
				"objectStore": "ConcurrentStore",
				// The free memory percentage threshold to start evicting objects
				// Only use if memory is constricted and you need to relieve cache pressure
				// Please note that this only makes sense depending on which object store you use.
				"freeMemoryPercentageThreshold": 0,
				// The frequency in seconds to check for expired objects and expire them using the policy
				// This creates a BoxLang task that runs every X seconds to check for expired objects
				// Default is every 2 minutes
				"reapFrequency": 120,
				// This makes a session extend it's life when accessed.  So if a users uses anything or puts anything
				// In session, it will re-issue the timeout.
				"resetTimeoutOnAccess": true,
				// Sessions don't rely on the last access timeouts but on the default timeout only.
				"useLastAccessTimeouts": false
			}
		},
		"bxImports": {
			"provider": "BoxCacheProvider",
			"properties": {
				"evictCount": 1,
				"evictionPolicy": "LRU",
				"freeMemoryPercentageThreshold": 0,
				"maxObjects": 200,
				"defaultLastAccessTimeout": 1800,
				"defaultTimeout": 3600,
				"objectStore": "ConcurrentStore",
				"reapFrequency": 120,
				"resetTimeoutOnAccess": false,
				"useLastAccessTimeouts": true
			}
		}
	},
	// These are the security settings for the runtime
	"security": {
		// All regex patterns are case-insensitive
		// A list of regex patterns that will match class paths, and if matched, execution will be disallowed
		// This applies to import statements, createObject, new, and class creation
		// Ex: "disallowedImports": ["java\\.lang\\.(ProcessBuilder|Reflect", "java\\.io\\.(File|FileWriter)"]
		"disallowedImports": [],
		// A list of BIF names that will be disallowed from execution
		// Ex: "disallowedBifs": ["createObject", "systemExecute"]
		"disallowedBifs": [],
		// A list of Component names that will be disallowed from execution
		// Ex: "disallowedComponents": [ "execute", "http" ]
		"disallowedComponents": [],
		// An explicit whitelist of file extensions that are allowed to be uploaded - overrides any values in the disallowedWriteExtensions
		"allowedFileOperationExtensions": [],
		// The list of file extensions that are not allowed to be uploaded. Also enforced by file relocation operations ( e.g. copy/move )
		"disallowedFileOperationExtensions": [
			"bat",
			"exe",
			"cmd",
			"cfm",
			"cfc",
			"cfs",
			"bx",
			"bxm",
			"bxs",
			"sh",
			"php",
			"pl",
			"cgi",
			"386",
			"dll",
			"com",
			"torrent",
			"js",
			"app",
			"jar",
			"pif",
			"vb",
			"vbscript",
			"wsf",
			"asp",
			"cer",
			"csr",
			"jsp",
			"drv",
			"sys",
			"ade",
			"adp",
			"bas",
			"chm",
			"cpl",
			"crt",
			"csh",
			"fxp",
			"hlp",
			"hta",
			"inf",
			"ins",
			"isp",
			"jse",
			"htaccess",
			"htpasswd",
			"ksh",
			"lnk",
			"mdb",
			"mde",
			"mdt",
			"mdw",
			"msc",
			"msi",
			"msp",
			"mst",
			"ops",
			"pcd",
			"prg",
			"reg",
			"scr",
			"sct",
			"shb",
			"shs",
			"url",
			"vbe",
			"vbs",
			"wsc",
			"wsf",
			"wsh"
		]
	},
	/**
	 * The BoxLang module settings
	 * The key is the module name and the value is a struct of settings for that specific module
	 * The `disabled` property is a boolean that determines if the module should be enabled or not
	 * The `settings` property is a struct of settings that are specific to the module and will be override the module settings
	 */
	"modules": {
		// The Compat Module
		// "compat": {
		// 	"disabled": false,
		// 	"settings": {
		// 		"isLucee": true,
		// 		"isAdobe": true
		// 	}
		// }
	}
}
```

{% hint style="success" %}
Please note that JSON support in BoxLang allows you to use comments inline.
{% endhint %}

### Environmental/Properties Configuration

BoxLang gives you the ability to override any of the runtime configuration or module settings via environment variables or java system properties.  For example adding an environment variable of `boxlang.security.allowedFileOperationExtensions=.exe,.sh`  would override the disallowed defaults above, and allow users to upload and rename files with these extensions ( not a good idea! ).  &#x20;

The variable names can be either snake-cased or dot-delimited.  For example `BOXLANG_DEBUGMODE=true`  will work the same as `boxlang.debugMode=true`  to set the entire runtime in to debug mode. &#x20;

This convention allows  you to make granular changes to sub-segments of the configuration without overriding parent items.   JSON is also allowed when overriding config settings.  For example, if I wanted to set the runtime logging level to trace without putting the runtime in to DebugMode, I could set the environment variable: `boxlang.logging.loggers.runtime.level=TRACE`  or add the JVM arg `-Dboxlang.logging.loggers.runtime.level=TRACE`

### Internal Variables

The following are the internal variable substitutions you can use in any value:

```json
 * ${boxlang-home} - The BoxLang home directory
 * ${user-home} - The user's home directory
 * ${user-dir} - The user's current directory
 * ${java-temp} - The java temp directory
```

### Environment Variable Substitution

BoxLang supports environment variable substitution using the syntax `${env.environment_variable_name:default}`. For example, using `${env.MYSQL_HOST:localhost}` will result in the value of the `MYSQL_HOST` environment variable, if found, or fall back to the `localhost` value if the environment variable is not defined.

Inside your `boxlang.json` configuration file, you can use this to populate datasource credential secrets:

```json
{
    // ...
    "datasources": {
        "mySqlServerDB": {
            driver: "mssql",
            host: "localhost",
            port: "${env.MSSQL_PORT:1433}",
            database: "myDB",
            username: "${env.MSSQL_USERNAME:sa}",
            password: "${env.MSSQL_PASSWORD:123456Password}"
        }
    },
    
}
```

## Configuration Segments

Here, you will find each segment and its configuration details.

### Application Timeout

The default timeout for applications in BoxLang. The default is **0 = never expires**. The value must be a string timespan using the syntax shown:

```json
// Use Timespan syntax: "days, hours, minutes, seconds"
"applicationTimeout": "0,0,0,0",
```

### Class Generation Directory

This is the location where BoxLang will store compiled classes.

```json
"classGenerationDirectory": "${boxlang-home}/classes"
```

### Caches

Here is where you can register named caches that you can then retrieve via the `getBoxCache()` BIFs.

```json
/**
* Register any named caches here.
* The key is the name of the cache and the value is the cache configuration.
*
* A `provider` property is required and the value is the name of the cache provider or the fully qualified class name.
* The `properties` property is optional and is a struct of properties that are specific to the cache provider.
*/
"caches": {
	// This is the holder of all sessions in a BoxLang runtime.
	// The keys are prefixed by application to create separation.
	"bxSessions": {
		"provider": "BoxCacheProvider",
		"properties": {
			// How many objects to evict when the cache is full
			"evictCount": 1,
			// The eviction policy to use: FIFO, LFU, LIFO, LRU, MFU, MRU, Random
			"evictionPolicy": "LRU",
			// The maximum number of objects the cache can hold
			"maxObjects": 100000,
			// How long should sessions last for in seconds. Default is 60 minutes.
			"defaultTimeout": 3600,
			// The object store to use to store the objects.
			// The default is a ConcurrentStore which is a thread safe and fast storage.
			// Available Stores are: BlackHoleStore, ConcurrentSoftReferenceStore, ConcurrentStore, FileSystemStore, Your own.
			"objectStore": "ConcurrentStore",
			// The free memory percentage threshold to start evicting objects
			// Only use if memory is constricted and you need to relieve cache pressure
			// Please note that this only makes sense depending on which object store you use.
			"freeMemoryPercentageThreshold": 0,
			// The frequency in seconds to check for expired objects and expire them using the policy
			// This creates a BoxLang task that runs every X seconds to check for expired objects
			// Default is every 2 minutes
			"reapFrequency": 120,
			// This makes a session extend it's life when accessed.  So if a users uses anything or puts anything
			// In session, it will re-issue the timeout.
			"resetTimeoutOnAccess": true,
			// Sessions don't rely on the last access timeouts but on the default timeout only.
			"useLastAccessTimeouts": false
		}
	},
	"bxImports": {
		"provider": "BoxCacheProvider",
		"properties": {
			"evictCount": 1,
			"evictionPolicy": "LRU",
			"freeMemoryPercentageThreshold": 0,
			"maxObjects": 200,
			"defaultLastAccessTimeout": 1800,
			"defaultTimeout": 3600,
			"objectStore": "ConcurrentStore",
			"reapFrequency": 120,
			"resetTimeoutOnAccess": false,
			"useLastAccessTimeouts": true
		}
	}
},
```

The name is the key of the structure. Each configuration structure has two keys

* `provider` - The name of a core provider or a full class path to use. Ex: `BoxCacheProvider` which is the core one, or a module collaborated class or class path class: `ortus.boxlang.modules.redis.RedisCache`
* `properties` - A structure of configuration for the provider.

### Custom Tags Directory

BoxLang allows you to register global locations where we can register custom tags for use in your templates:

```json
// A collection of BoxLang custom tag directories, they must be absolute paths
"customTagsDirectory": [
	"${boxlang-home}/customTags"
],
```

### Debug Mode

This is a powerful setting. It puts the runtime into debug mode, where more verbose debugging will be activated, and when exceptions occur, more information will be shown by default. The default is `false` an we highly discourage its use in production.

```json
// This puts the entire runtime in debug mode
// Which will produce lots of debug output and metrics
// Also the debugging error template will be used if turned on
"debugMode": false,
```

### Default Datasource

The name of the datasource in the `datasources` configuration struct, which will act as the default one for the entire runtime.

```json
// You can assign a global default datasource to be used in the language
"defaultDatasource": "main",
"datasources" : {
    "main" : {...}
}
```

### Default Remote Method Return Format

The default return format for class invocations via web runtimes.

```json
// The default return format for class invocations via web runtimes
"defaultRemoteMethodReturnFormat": "json",
```

### Datasources

Here is where you can register datasources globally in the runtime. You can override them at runtime via:

* `Application.bx|cfc` - For the application
* A-la-carte via query execution calls

```json
// The registered global datasources in the language
// The key is the name of the datasource and the value is a struct of the datasource settings
"datasources": {
	"testDB": {
		"driver": "derby",
		"connectionString": "jdbc:derby:memory:testDB;create=true"
	}
	"testdatasource": {
	 	  "driver": "derby",
	 	  "host": "localhost",
	 	  "port": 3306,
	 	  "database": "test"
	}
},
```

The key is the name of the datasource and the value is a struct of configuration for the JDBC connection. Most of the items can be different depending on the JDBC module and driver used. However, at the end of the day we need to know at least either the `driver` , the `connectionString` or individual items of the connection. Check out our guide on [defining datasources here](../boxlang-language/datasources.md).

### Default Cache

BoxLang has a built-in enterprise caching framework. It also has a `default` cache backed by a BoxCacheProvider. It will always exist in operation. This is where you will configure it or leave it empty to use the defaults.

```json
// The configuration for the BoxLang `default` cache.  If empty, we use the defaults
// See the ortus.boxlang.runtime.config.segments.CacheConfig for all the available settings
// This is used by query caching, template caching, and other internal caching, unless you override it
"defaultCache": {},


// Use specifics
"defaultCache" : {
    // How many to evict at a time once a policy is triggered
    "evictCount": 1,
    // The eviction policy to use: Least Recently Used
    // Other policies are: LRU, LFU, FIFO, LIFO, RANDOM
    "evictionPolicy": "LRU",
    // The free memory percentage threshold to trigger eviction
    // 0 = disabled, 1-100 = percentage of available free memory in heap
   // If the threadhold is reached, the eviction policy is triggered
    "freeMemoryPercentageThreshold": 0,
    // The maximum number of objects to store in the cache
    "maxObjects": 200,
    // The maximum in seconds to keep an object in the cache since it's last access
    // So if an object is not accessed in this time or greater, it will be removed from the cache
    "defaultLastAccessTimeout": 1800,
    // The maximum time in seconds to keep an object in the cache regardless if it's used or not
    // A default timeout of 0 = never expire, careful with this setting
    "defaultTimeout": 3600,
    // The object store to use to store the objects.
    // The default is a ConcurrentStore
    "objectStore": "ConcurrentStore",
    // The frequency in seconds to check for expired objects and expire them using the policy
    // This creates a BoxLang task that runs every X seconds to check for expired objects
    "reapFrequency": 120,
    // If enabled, the last access timeout will be reset on every access
    // This means that the last access timeout will be reset to the defaultLastAccessTimeout on every access
    // Usually for session caches or to simulate a session
    "resetTimeoutOnAccess": false,
    // If enabled, the last access timeout will be used to evict objects from the cache
    "useLastAccessTimeouts": true
}
```

Available stores are:

<table><thead><tr><th width="293">Type</th><th>Description</th></tr></thead><tbody><tr><td><strong>BlackHoleStore</strong></td><td>Mocking store, just simulates a store, nothing is stored.</td></tr><tr><td><strong>ConcurrentSoftReferenceStore</strong></td><td>Memory-sensitive storage leveraging Java Soft References.</td></tr><tr><td><strong>ConcurrentStore</strong></td><td>Leverages concurrent hashmaps for storage.</td></tr><tr><td><strong>FileSystemStore</strong></td><td>Stores the cache items in a serialized fashion on disk</td></tr></tbody></table>

Each store can have different configuration properties as well.

### Experimental

This block is used to have experimental feature flags for BoxLang. Every experimental flag will be documented here once we have them.

```json
"experimental": {
    // This choose the compiler to use for the runtime
    // Valid values are: "java", "asm"
    "compiler": "java",
    // If enabled, it will generate AST JSON data under the project's /grapher/data folder
    "ASTCapture": false
},
```

#### Compiler

This is the compiler used for your BoxLang source. The available options are:

* `java` : We will transpile your BoxLang source to Java, then compile it
* `asm` : We will directly compile your BoxLang source to Java bytecode

#### AST Capture

If enabled, it will activate the AST capture interceptor and on parse it will create a `/grapher/data` folder in your project with useful AST JSON captures.

### Executors

BoxLang allows you to register named executors globally. The JSON object key is the name of the executor and the value is another object with the `type` and a `threads` property, which is optional. The default threads is `20` and only applicable.

```json
// Global Executors for the runtime
// These are managed by the AsyncService and registered upon startup
// The name of the executor is the key and the value is a struct of executor settings
// Types are: cached, fixed, fork_join, scheduled, single, virtual, work_stealing
// The `threads` property is the number of threads to use in the executor. The default is 20
// Some executors do not take in a `threads` property
"executors": {
	"boxlang-tasks": {
		"type": "scheduled",
		"threads": 20
	},
	"cacheservice-tasks": {
		"type": "scheduled",
		"threads": 20
	}
},
```

The available types in BoxLang are:

<table><thead><tr><th width="203">Type</th><th>Description</th></tr></thead><tbody><tr><td><strong>cached</strong></td><td><p>Creates a thread pool that creates new threads as needed but will reuse previously constructed threads when available.</p><p><strong>Use Case:</strong> Best for applications that execute many short-lived asynchronous tasks.</p></td></tr><tr><td><strong>fixed</strong></td><td><p>Creates a thread pool with a fixed number of threads. If all threads are busy, new tasks will wait in a queue until a thread is available.</p><p><strong>Use Case:</strong> Ideal for situations where you need a consistent number of threads to handle tasks, ensuring that the resource usage remains predictable.</p></td></tr><tr><td><strong>fork_join</strong></td><td><p>Designed for tasks that can be broken down into smaller tasks and executed in parallel. It uses a work-stealing algorithm to optimize throughput.</p><p><strong>Use Case:</strong> Suitable for recursive algorithms, parallel processing, and tasks that can be divided into subtasks.</p></td></tr><tr><td><strong>scheduled</strong></td><td><p>Allows tasks to be scheduled to run after a given delay or to execute periodically with a fixed number of threads.</p><p><strong>Use Case</strong>: Perfect for tasks that need to run at regular intervals or after a specific delay, such as periodic maintenance or monitoring tasks.</p></td></tr><tr><td><strong>single</strong></td><td><p>Creates a single-threaded executor that executes tasks sequentially.</p><p><strong>Use Case</strong>: Useful for scenarios where tasks must be executed in order, ensuring that no two tasks run simultaneously.</p></td></tr><tr><td><strong>virtual</strong></td><td><p>It uses virtual threads, also known as fibers, which are lightweight and managed by the JVM, providing high scalability with low overhead.</p><p><strong>Use Case</strong>: Best for high-concurrency applications where many lightweight tasks need to be managed efficiently.</p></td></tr><tr><td><strong>work_stealing</strong></td><td><p>Creates a pool of threads that attempts to keep all threads busy by stealing tasks from other threads when they have completed their work.</p><p><strong>Use Case:</strong> Excellent for irregular workloads where tasks may vary significantly in complexity and duration, optimizing resource usage and improving performance.</p></td></tr></tbody></table>

### Invoke Implicit Accessors

In BoxLang, the default for implicit accessors is `true`. This means that properties on a class can be accessed externally, like field properties for mutation or access.

```json
"invokeImplicitAccessor" : true
```

Simple example:

```cfscript
// Class has implicit accessors and mutators by default in BoxLang
class{
    property firstName
    property lastName
    property email
}


// Example
p = new Person()
// This calls the generated setters in the class
p.firstName = "luis"
p.lastname = "majano"
p.email = "info@boxlang.io"
// This calls the generated getters in the class
println( p.firstName );
```

### Java Library Paths

BoxLang allows you to register an array of locations or array of jars or classes that the runtime will class load into the runtime class loader. This allows you to class-load Java applications at runtime.

```json
// A collection of directories we will class load all Java *.jar files from
"javaLibraryPaths": [
	"${boxlang-home}/lib"
],
```

By default, we look into the `lib` folder in your BoxLang home.

### Locale

This is the default locale for the runtime. By default, we use the JVM locale. This value must be an IETF BCP language tag: [https://www.oracle.com/java/technologies/javase/jdk21-suported-locales.html](https://www.oracle.com/java/technologies/javase/jdk21-suported-locales.html)

```json
// The default locale for the runtime; defaults to the JVM locale if empty
// Please use the IETF BCP 47 language tag values
"locale": "es_sv",

// You can also use hypens
"locale": "es-sv",

// Or just the first part
"locale": "es",
```

### Logging

This section configures the logging framework in BoxLang.  Please note that BoxLang leverages `RollingFileAppenders` for most of its loggers.  This provides consistency for the language and a consistent destination.  You can customize it as you see fit, but this provides uniformity to the core and modules.

{% hint style="success" %}
Please also note that in BoxLang, you can log data as **text** or as **JSON**.
{% endhint %}

```json
// Logging Settings for the runtime
"logging": {
	// The location of the log files the runtime will produce
	"logsDirectory": "${boxlang-home}/logs",
	// The maximum number of days to keep log files before rotation
	// Default is 90 days or 3 months
	// Set to 0 to never rotate
	"maxLogDays": 90,
	// The maximum file size for a single log file before rotation
	// You can use the following suffixes: KB, MB, GB
	// Default is 100MB
	"maxFileSize": "100MB",
	// The total cap size of all log files before rotation
	// You can use the following suffixes: KB, MB, GB
	// Default is 5GB
	"totalCapSize": "5GB",
	// The root logger level
	// Valid values are in order of severity: ERROR, WARN, INFO, DEBUG, TRACE, OFF
	// If the runtime is in Debug mode, this will be set to DEBUG
	"rootLevel": "WARN",
	// Default Encoder for file appenders.
	// The available options are "text" and "json"
	"defaultEncoder": "text",
	// A collection of pre-defined loggers and their configurations
	"loggers": {
		// The runtime main and default log
		"runtime": {
			// Valid values are in order of severity: ERROR, WARN, INFO, DEBUG, TRACE, OFF
			// Leave out if it should inherit from the root logger
			//"level": "WARN",
			// Valid values are: "file", "console",
			// Coming soon: "smtp", "socket", "db", "syslog" or "java class name"
			// Please note that we only use Rolling File Appenders
			"appender": "file",
			// Use the defaults from the runtime
			"appenderArguments": {},
			// The available options are "text" and "json"
			"encoder": "text",
			// Additive logging: true means that this logger will inherit the appenders from the root logger
			// If false, it will only use the appenders defined in this logger
			"additive": true
		},
		// The modules log
		"modules": {
			// Valid values are in order of severity: ERROR, WARN, INFO, DEBUG, TRACE, OFF
			// Leave out if it should inherit from the root logger
			//"level": "WARN",
			// Valid values are: "file", "console",
			// Coming soon: "smtp", "socket", "db", "syslog" or "java class name"
			// Please note that we only use Rolling File Appenders
			"appender": "file",
			// Use the defaults from the runtime
			"appenderArguments": {},
			// The available options are "text" and "json"
			"encoder": "text",
			// Additive logging: true means that this logger will inherit the appenders from the root logger
			// If false, it will only use the appenders defined in this logger
			"additive": true
		},
		// All applications will use this logger
		"application": {
			// Valid values are in order of severity: ERROR, WARN, INFO, DEBUG, TRACE, OFF
			// Leave out if it should inherit from the root logger
			"level": "TRACE",
			// Valid values are: "file", "console",
			// Coming soon: "smtp", "socket", "db", "syslog" or "java class name"
			// Please note that we only use Rolling File Appenders
			"appender": "file",
			// Use the defaults from the runtime
			"appenderArguments": {},
			// The available options are "text" and "json"
			"encoder": "text",
			// Additive logging: true means that this logger will inherit the appenders from the root logger
			// If false, it will only use the appenders defined in this logger
			"additive": true
		},
		// All scheduled tasks logging
		"scheduler": {
			// Valid values are in order of severity: ERROR, WARN, INFO, DEBUG, TRACE, OFF
			// Leave out if it should inherit from the root logger
			"level": "INFO",
			// Valid values are: "file", "console",
			// Coming soon: "smtp", "socket", "db", "syslog" or "java class name"
			// Please note that we only use Rolling File Appenders
			"appender": "file",
			// Use the defaults from the runtime
			"appenderArguments": {},
			// The available options are "text" and "json"
			"encoder": "text",
			// Additive logging: true means that this logger will inherit the appenders from the root logger
			// If false, it will only use the appenders defined in this logger
			"additive": true
		}
	}
},
```

#### Logs Directory

This is the folder where BoxLang will store it's log files.  By default we use the following:

```json
// The location of the log files the runtime will produce
"logsDirectory": "${boxlang-home}/logs",
```

#### Max Log Days

The maximum number of days to keep log files before rotations.  The default is 90 days or 3 months.  If you put a `0` then rotation will never happen and you will log forever!

```json
"maxLogDays": 90,
```

#### Max File Size

The maximum filesize for a **single** log file before rotation occurs.  The default is 100 Megabytes.  You can use a number or the following suffixes: KB, MB, GB.

```json
"maxFileSize": "100MB",
```

#### Total Cap Size

The total cap size of ALL log files before rotation and compression begins.  The default is 5 Gigabytes.  You can use a number or the following suffixes: KB, MB, GB.

```json
"totalCapSize": "5GB",
```

#### Root Level

This is the level at which the root logger will be allowed to be logged.  By default, it is `WARN`, However, if it detects you are in debug mode, it will bump it to `DEBUG`.

```json
"rootLevel": "WARN",
```

#### Default Encoder

By default, BoxLang is configured to log using a pattern textual encoder.  However, if you want to leverage the new JSON Lines format, you can switch the encoder for ALL loggers to be `JSON`.  Valid values are `text` or `json`

```json
"defaultEncoder" : "text"
```

#### Loggers

BoxLang allows you to pre-define named loggers that will be configured when used via BoxLang calls to:

* `writeLog()` BIF
* `log` component

However, you can also retrieve named loggers via the `LoggingService.` By default, we will register the following named loggers:

* `application` - Application-specific logs
* `modules` - For all modular information, activation, etc
* `runtime` - The default log file for all runtime-related logging
* `scheduler` - All tasks and schedulers can log here

```json
"runtime": {
    // Valid values are in order of severity: ERROR, WARN, INFO, DEBUG, TRACE, OFF
    // Leave out if it should inherit from the root logger
    //"level": "WARN",
    // Valid values are: "file", "console",
    // Coming soon: "smtp", "socket", "db", "syslog" or "java class name"
    // Please note that we only use Rolling File Appenders
    "appender": "file",
    // Use the defaults from the runtime
    "appenderArguments": {},
    // The available options are "text" and "json"
    "encoder": "text",
    // Additive logging: true means that this logger will inherit the appenders from the root logger
    // If false, it will only use the appenders defined in this logger
    "additive": true
},
```

Each logger will have the following configuration items:

<table><thead><tr><th width="205">Property</th><th width="129">Default</th><th width="121">Type</th><th>Description</th></tr></thead><tbody><tr><td><strong>additive</strong></td><td><code>true</code></td><td><code>boolean</code></td><td><strong>true</strong> means that this logger will inherit the appenders from the root logger and log through all of them.  <code>false</code> means it doesn't buble up log messages.</td></tr><tr><td><strong>appender</strong></td><td><code>file</code></td><td><code>string</code></td><td>The type of appender to use for this logger. By default we use the rolling file appender.  <br><br>Valid values are:<br>- file<br>- console<br><br>Coming soon values:<br>- smtp<br>- socket<br>- db<br>- syslog<br>- class name</td></tr><tr><td><strong>appenderArguments</strong></td><td>---</td><td><code>object</code></td><td>Name-value pairs that configure the appender.  Each appender can have different arguments.</td></tr><tr><td><strong>encoder</strong></td><td><code>logging > defaultEncoder</code></td><td><code>text</code> or <code>json</code></td><td>The encoder to use for logging. By default it levereages what was defined in the <code>logging.defaultEncoder</code> configuration.</td></tr><tr><td><strong>level</strong></td><td><code>TRACE</code></td><td><code>logLevel</code></td><td>The log level is to be assigned to the appender.  By default, each appender is wide open to the maximum level of <code>TRACE</code></td></tr></tbody></table>



### Mappings

Here is where you can create global class mappings in BoxLang. The value is a JSON object where the key is the name of the mapping, and the value is the absolute path location of the mapping. You can prefix the name of the mapping with `/` or not. Ultimately, we will add it for you as well.

```json
// A collection of BoxLang mappings, the key is the prefix and the value is the directory
"mappings": {
	"/": "${user-dir}",
	"/core" : "/opt/core"
},
```

Mappings are used to discover BoxLang classes, files and more.

### Modules Directory

BoxLang will search your home for a `modules` folder and register the modules found. However, you can add multiple locations to search for BoxLang modules. Each entry must be an absolute location.

```json
// A collection of BoxLang module directories, they must be absolute paths
"modulesDirectory": [
    "${boxlang-home}/modules"
],
```

### Modules

BoxLang is a modular language. Each module can have a configuration structure for usage within the language. The name of the key is the name of the module and each module config has the following keys available:

* `disabled` : Boolean indicator to disable or eanble the module from loading
* `settings` : A structure of configuration settings each module exposes

```json
/**
 * The BoxLang module settings
 * The key is the module name and the value is a struct of settings for that specific module
 * The `disabled` property is a boolean that determines if the module should be enabled or not
 * The `settings` property is a struct of settings that are specific to the module and will be override the module settings
 */
"modules": {
	// The Compat Module
	"compat": {
	 	"settings": {
	 		"engine" : "Adobe"
	 	}
	}
}
```

### Request Timeout

The default timeout for requests in BoxLang. The default is 0 = never expire. The value must be a string timespan using the syntax shown:

```json
// Use Timespan syntax: "days, hours, minutes, seconds"
"requestTimeout": "0,0,0,0",
```

### Security

This segment is where you can configure the security elements of BoxLang.

#### Allowed File Operation Extensions

An explicit whitelist of file extensions that are allowed to be uploaded - overrides any values in the `disallowedWriteExtensions`

```json
"allowedFileOperationExtensions": [],
```

#### Disallowed Imports

An array of regex patterns (case-sensitive) that will try to be matched to imports or to creation of classes. If they match the patterns a security exception wil be thrown.

```json
// Ex: "disallowedImports": ["java\\.lang\\.(ProcessBuilder|Reflect", "java\\.io\\.(File|FileWriter)"]
"disallowedImports": [],
```

#### Disallowed BIFS

An array of BIF names that will be disallowed from execution.

```json
// Ex: "disallowedBifs": ["createObject", "systemExecute"]
"disallowedBifs": [],
```

#### Disallowed Components

An array of Component names that will be disallowed from execution.

```json
// Ex: "disallowedComponents": ["execute", "http"]
"disallowedComponents": [],
```

#### Disallowed File Operation Extensions

The list of file extensions that are not allowed to be uploaded. Also enforced by file relocation operations ( e.g. copy/move )

```json
"disallowedFileOperationExtensions": [
		"bat",
		"exe",
		"cmd",
		"cfm",
		"cfc",
		"cfs",
		"bx",
		"bxm",
		"bxs",
		"sh",
		"php",
		"pl",
		"cgi",
		"386",
		"dll",
		"com",
		"torrent",
		"js",
		"app",
		"jar",
		"pif",
		"vb",
		"vbscript",
		"wsf",
		"asp",
		"cer",
		"csr",
		"jsp",
		"drv",
		"sys",
		"ade",
		"adp",
		"bas",
		"chm",
		"cpl",
		"crt",
		"csh",
		"fxp",
		"hlp",
		"hta",
		"inf",
		"ins",
		"isp",
		"jse",
		"htaccess",
		"htpasswd",
		"ksh",
		"lnk",
		"mdb",
		"mde",
		"mdt",
		"mdw",
		"msc",
		"msi",
		"msp",
		"mst",
		"ops",
		"pcd",
		"prg",
		"reg",
		"scr",
		"sct",
		"shb",
		"shs",
		"url",
		"vbe",
		"vbs",
		"wsc",
		"wsf",
		"wsh"
	],
```

### Session Timeout

The default timeout for sessions in BoxLang. The default is 30 minutes. The value must be a string timespan using the syntax shown:

```json
// Use Timespan syntax: "days, hours, minutes, seconds"
"sessionTimeout": "0,0,30,0",
```

### Session Storage

In BoxLang, you can configure your user sessions to be stored in `memory` by default in a BoxLang sessions cache, or you can give it a custom cache name to store them in. If it's not `memory`\` then it must be a valid registered cache. (See [Caches](configuration.md#caches))

```json
// Where sessions will be stored by default.  This has to be a name of a registered cache
// or the keyword "memory" to indicate our auto-created cache.
// This will apply to ALL applications unless overridden in the Application.cfc
"sessionStorage": "redis",
```

### Timezone

This is the global timezone to use in the runtime. By default, it will use the JVM timezone. This value requires IANA timezone database values: [https://en.wikipedia.org/wiki/List\_of\_tz\_database\_time\_zones#List](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List)

```json
"timezone": "UTC"
```

### Use High Precision Math

By default BoxLang uses high-precision mathematics via `BigDecimal` operations. It analyses your operations and determines the precision accordingly. You can turn this off here for all applications and use Double based operations. If you disable this feature, then if you want high precision you will have to use the `precisionEvaluate( expression )` [BIF instead](../boxlang-language/reference/built-in-functions/math/PrecisionEvaluate.md).

```json
// By default BoxLang uses high-precision mathematics via BigDecimal operations
// You can turn this off here for all applications
"useHighPrecisionMath": true,
```

### Valid Class Extensions

This is an array of all the extensions that will be processed as BoxLang classes. By default we target `bx, cfc`.

```json
// Extensions BoxLang will process as classes
"validClassExtensions": [
	"bx",
	// Moving to compat at final release
	"cfc"
],
```

### Valid Template Extensions

This is an array of all the extensions that will be processed as BoxLang templates. Meaning you can execute them and include them.

```json
// Extensions BoxLang will process as templates.
// This is used by the RunnableLoader
"validTemplateExtensions": [
	"bxs",
	"bxm",
	"bxml",
	// Moving to compat at final release
	"cfm",
	"cfml",
	"cfs"
],
```
