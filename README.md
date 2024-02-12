# Wind River CLI

## Purpose

This project strives to to provide a common command line interface for installing and interacting with Wind River Tooling.  This common interface creates stability across multiple edge products and versions, allowing tooling such as extensions to third-party DevOps ecosystems, customer automation scripts, etc. to be developed and maintined with speed and ease. This also creates a modern and intuitive way to navigate product functionality across the broader Wind River ecosystem.

## Concepts

The CLI is broken down into 
`wr`


versioning

## Core Components

The Wind River CLI includes four default components, which aserve as a base for all additional functionality.

The initial goal of this CLI is to provide support for Linux.  Support for Windows (outside of WSL) is not a priority.

`Packages`

`Extensions`



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

- `wr package add --name [package-name]`
  
  Installs a new package

- `wr package remove [package-name]'
  
  Removes a previously installed package


### Extensions

Extensions are sets of functionality added to the `wr` CLI after initial installation.  These extensions provide commands that enable interaction with the relevant Wind River Edge products. For example, the `vxworks` extension provides the ability to execute platform and app builds for VxWorks with commands such as `wr vx vsb create`, which would create a VxWorks Source Build.

The `extension` component of the CLI provides a means to browse, add, and remove extensions for various Wind River edge proudcts.

Extensions are *not limited* to interacting with packages installed using the `wr` CLI.  Extensions can be used to interact with existing installations of Wind River edge products, so long as the correct underlying software (and version) is available and the correct configuration (e.g., installation path) is provided.

#### Commands

- `wr extension list-available`
  
  Lists pac


The `extension` component provides the ability to install 

### `license`
### `serve`

### Common Features




###### Default actions



## Components

## Specification

*Call Structure*
All `wr` commands shall follow the following call structure:

```
[wr] [component] [command/alias]+ [--parameter]*
```

| Segment | Purpose |
| - | - |
| `[wr]` | The Wind River CLI executable | *required* |
| `[component]` | The name or alias of the component |
| `[command]` | The command to call within the component. Optionally, these can be nested with multiple subcommands. |
| `[parameter]` | A paramater to use with the given command. Any number of these can be provided. |

* Parameters*

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

*Aliasing*

Commands, components, and paramaters may provide aliases for simplicity.  For example, instead of using `vxworks` for the component that controls VxWorks `vx` can be used.  And in place of `source-build` as the command to control VxWorks Source Builds, `vsb` can be used.  For example, the following commands are equivalent:

```bash
>wr vx vsb create
```

```bash
>wr vxworks source-build create
```

Parameters may be also be aliased, but shall still require a `--` prefix.  However, an exception is to be made if a single letter alias is being createded for added convenience.  Single letter aliases can use a single `-` prefix.  For example, the following commands are equivalent:

```bash
>wr vx vsb create --name MyProject
```

```bash
>wr vxworks source-build create -n MyProject
```



*Casing*
All package, command, and parameter names shall be in lower [kebab case](https://developer.mozilla.org/en-US/docs/Glossary/Kebab_case).

For example, a command that requires a licens file could use `license-file` as the command name.
  
- All command parameters shall be individually specified with a `--` prefix


