# Wind River CLI

## Purpose

This project strives to to provide a common command line interface for installing and interacting with Wind River Tooling.  This common interface creates stability across multiple edge products and versions, allowing tooling such as extensions to third-party DevOps ecosystems, customer automation scripts, etc. to be developed and maintined with speed and ease. This also creates a modern and intuitive way to navigate product functionality across the broader Wind River ecosystem.

## Concepts

The CLI is broken down into 
`wr`


versioning

## Core Components

The Wind River CLI includes four default components, which aserve as a base for all additional functionality.

`Packages`

`Extensions`



### `package`

Packages are sets of installable tools, applications, and more that are provided through Wind River's WindShare delivery site.  The `package` component of the CLI provides a mechanism for browsing, installing, and removing individual packages.

*Note*: The `package` component only has visibility to packages that have been installed using the `wr` CLI.  It does not have visibility to existing installations of edge tools such as Workbench, the Wind River Linux Distro Builder, etc.  As a best practice-  in new enviornments, containers, etc., Wind River edge products should be installed using the `package` component of the CLI to enable better management of tools and ensure future-upgradability.

**What's different from the Wind River Installer (setup_linux)?**

Previously `setup_linux` functioned as a UI-driven global tool installer for Wind River, with some options for headless interaction.  This was not designed for lightweight automation tools, containers, granular package installation, etc. The `package` component provides a modern, lightweight, command-line friendly alternative with much more fine-grained controls.


### `extension`

Extensions are sets of functionality added to the `wr` CLI after initial installation.  These extensions provide commands that enable interaction with the relevant Wind River Edge products.  For example, the `vxworks` package provides the ability to execute platform and app builds for VxWorks with commands such as `wr vx vsb create`, which would create a VxWorks Source Build.

Extensions are not limited to interacting with packages installed using the `wr` CLI.  These extensions can be added to interact with existing installations of edge products, so long as the correct underlying software (and version) is available.



However, for use on systems with existing product installations, the extensions can be added manually to provide the same common interface for interaction.

The `extension` component provides the ability to install 

### `license`
### `serve`

### Common Features
## Components
## Structure
