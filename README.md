# Wind River CLI

## Purpose

This project strives to to provide a common command line interface for installing and interacting with Wind River edge developer products.  This common interface creates stability across multiple edge products and versions, allowing tooling such as extensions to third-party DevOps ecosystems, customer automation scripts, etc. to be developed and maintined with speed and ease. This also creates a modern and intuitive way to navigate product functionality across the broader Wind River ecosystem.

The initial goal of this CLI is to provide support for Linux.  Support for Windows (outside of WSL) is not an identified priority and not part of the current delivery scope.

## Concepts

The `wr` CLI is broken down into a collection of `components`, which each provide sets of `commands` relevant to a given scope.  Core components responsible for things like licensing and installation of Wind River products are provided by default with the CLI. Components responsible for managing specific edge products (e.g., VxWorks or Wind River Linux) can be added using the `extension` component.

## Core Components

The Wind River CLI includes four default components, which serve as a base for all additional functionality.

### Packages

Packages are sets of installable tools, applications, and more that are provided through Wind River's WindShare delivery site.  The `package` component of the CLI provides a mechanism for browsing, installing, and removing individual packages.

*Note*: The `package` component only has visibility to packages that have been installed using the `wr` CLI.  It does not have visibility to existing installations of edge tools such as Workbench, the Wind River Linux Distro Builder, etc.  As a best practice-  in new enviornments, containers, etc., Wind River edge products should be installed using the `package` component of the CLI to enable better management of tools and ensure future-upgradability.

**How is this different from today's Wind River Installer (setup_linux)?**

Previously `setup_linux` functioned as a UI-driven global tool installer for Wind River, with some options for headless interaction.  This was not designed for lightweight automation tools, containers, granular package installation, etc. The `package` component provides a modern, lightweight, command-line friendly alternative with much more fine-grained controls.

#### Commands

- `wr package list-available`
  
  Lists packages which are able to be installed

- `wr package show --name [package-name]`

  Shows details of the given package

- `wr package list`
  
  Lists packages installed on the current system.

- `wr package install --name package-name`
  
  Installs a new package

- `wr package remove --name package-name`
  
  Removes a previously installed package


### Extensions

Extensions are sets of functionality added to the `wr` CLI after initial installation.  These extensions provide commands that enable interaction with the relevant Wind River Edge products. For example, the `vxworks` extension provides the ability to execute platform and app builds for VxWorks with commands such as `wr vx vsb create`, which would create a VxWorks Source Build.

The `extension` component of the CLI provides a means to browse, add, and remove extensions for various Wind River edge proudcts.

Extensions are *not limited* to interacting with packages installed using the `wr` CLI.  Extensions can be used to interact with existing installations of Wind River edge products, so long as the correct underlying software (and version) is available and the correct configuration (e.g., installation path) is provided.

#### Commands

- `wr extension list-available`
  
  Lists extensions available for install.

- `wr extension list`
  
  Lists extensions that are currently installed on the system.

- `wr extension add --name extension-name`

  Adds a new extension to the CLI

- `wr extension remove --name extension-name`

  Removes an extension from the CLI


### Licenses

The license component manages a collection of locally stored license configurations (and license files) which can be referenced within CLI extensions.

#### Commands

- `wr license list`
  
  Lists the available licenses that have been configured for use on this system.

- `wr license show --name license-name`
  
  Shows configuration details of the license with the given name

- `wr license add [license-details]`
  
  Adds a new license configuration

- `wr license remove --name license-name`
  
  Removes a license configuration from the current system.

  

### Serve

The serve component enables an API server which exposes all functionality of the CLI and extensions over a restful interface.

#### Commands

- `wr server start [--background]`

  Starts a [Flask](https://flask.palletsprojects.com/en/3.0.x/) API Server, attached to the current terminal and will print output logs until the process is terminated or Ctrl+C is pressed.

  If the `--background` parameter is specified, the HTTP server is launched as a background process. This call does not allow for launching multiple servers, and will error if another process is already running.

- `wr server stop`

  Stops a running background instance of the HTTP server.

## CLI Specification


#### Naming Conventions
All package, command, and parameter names shall be in lower [kebab case](https://developer.mozilla.org/en-US/docs/Glossary/Kebab_case).

*For example*, a command that requires a license file could use `license-file` as a paramater name.


#### Call Structure
All `wr` commands shall follow the following call structure:

```
[wr] [component] [command/alias]+ [--parameter] [--help]*
```

Which represent the following:

| Segment | Purpose |
| - | - |
| `[wr]` | The Wind River CLI executable | *required* |
| `[component]` | The name or alias of the component |
| `[command]` | The command to call within the component. Optionally, these can be nested with multiple subcommands. |
| `[parameter]` | A paramater to use with the given command. Any number of these can be provided. |

#### Parameters

All paramaters shall be specified using `--` as a prefix. For example:

```bash
> wr vx vsb create --name MyProject
```

Parameters which require array/list input shall use whitespace for delimeters. For example:

```bash
wr vx vsb create --components INCLUDE_IPTELNETS INCLUDE_SSH
```

If paramaters need to include a key/value pair, an equal sign may then be used.  For example:

```bash
wr vx vsb create --components _WRS_CONFIG_BOOST_THREAD=y
```

When white space as is to be part of paramater input, the value shall be given in single or double quotes.  For example:

```bash
wr vx vsb create --name "My Project"
```

When quotes need to be used inside a paramter value, they should be escaled with a `\`.  For example:

```bash
wr vx vsb create --name "My \"Project\""
```

Parameters may also be specified by file, using the reserved parameter `--parameters`. 

Both [YAML](https://yaml.org/) and [JSON](https://www.json.org/json-en.html) formats are supported for paramaters files. If available, the relevant [content-type](https://www.iana.org/assignments/media-types/media-types.xhtml) will be implied by the extension of the file provided.

| Extension | Content-Type       |
|---------- | ------------------ |
| `.json`   | `applicaiton/json` |
| `.yaml`   | `application/yaml` |
| `.yml`    | `application/yaml` |

For example, the following will load a `params.yml` and parse it as YAML:

```bash
wr vx vsb create --parameters ./params.yml
```

File type can also be explicity set by using key-value pairs in parameters.

```bash
wr vx vsb create --parameters path=./params \
                              content-type=application/json
```

To load parameters from multiple files `--parameters` may be specified multiple times.  If there are conflicts in values, the last specified will take precedence and a warning message will be displayed.

```bash
wr vx vsb create --parameters ./vsb.yaml \
                 --parameters ./board.json \
                 --parameters ./layers.json 
```


#### Aliasing

Commands, components, and paramaters may provide aliases for simplicity.  For example, instead of using `vxworks` for the component that controls VxWorks `vx` can be used.  And in place of `source-build` as the command to control VxWorks Source Builds, `vsb` can be used.  For example, the following commands are equivalent:

```bash
wr vx vsb create
```

```bash
wr vxworks source-build create
```

Parameters may be also be aliased, but shall still require a `--` prefix.  However, an exception is to be made if a single letter alias is being createded for added convenience.  Single letter aliases can use a single `-` prefix.  For example, the following commands are equivalent:

```bash
wr vx vsb create --name MyProject
```

```bash
wr vxworks source-build create -n MyProject
```

#### Getting help

`[--help]` is an optional reserved parameter which can be applied to any CLI call and will return useful information about how to use the specific component, command, or the `wr` CLI executable. If specified on a command, it will immediately [no-op](https://en.wikipedia.org/wiki/NOP_(code)) and only provide relevant documentation for the component or command.

#### Default Operations

The `wr` executable and each component will by default apply the  `--help` paramater if no command is given.  If a command requires paramaters that are missing, or there is a formatting error of some kind, the error message will be displayed followed by the relevant `--help` response.


#### Output Formats

The default output format of the CLI is human-readable text, however any results which contain list or dictionary information (e.g., `wr extension list`) can also be exported in table, JSON, or YAML formats using the `--output` reserved parameter.

You can find a Table format example here:
```
> wr package list-avaialble --output table

|-----------------------------|
| Name              | Version |          
|-----------------------------|
| VxWorks 7           | 23.09 |
| Wind River Linux   |  23.03 |
|-----------------------------|

```

And JSON:
```
> wr package list-avaialble --output json

[
  {
    "name": "VxWorks 7"
    "category": "vxworks"
    "currentVersion": "23.09"
  },
  {
    "name": "Wind River Linux"
    "category": "linux"
    "currentVersion": "23.09"
  },
]
```
