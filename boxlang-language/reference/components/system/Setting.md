[comment]: # (Note: This documentation is generated dynamically in the build process.  To modify the contents, change the javadoc on the _invoke method of the Component class)
# Component: `Setting`

Controls some key request features of the runtime at the request level.

## Component Signature

```
<bx:Setting enableOutputOnly=[boolean]
showDebugOutput=[boolean]
requestTimeout=[long] />
```

### Attributes


| Atrribute | Type | Required | Description | Default |
|----------|------|----------|-------------|---------|
| `enableOutputOnly` | `boolean` | `false` | If true, the runtime will only output the result of the request and not the debug output |  |
| `showDebugOutput` | `boolean` | `false` | If true, the runtime will output the debug output according to the runtime in use |  |
| `requestTimeout` | `long` | `false` | The timeout in seconds for the request |  |

## Examples

```
<bx:Setting enableOutputOnly=[boolean]
showDebugOutput=[boolean]
requestTimeout=[long] />
```