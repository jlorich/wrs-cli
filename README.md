# Wind River CLI

## Purpose

This project strives to to provide a common command line interface for installing and interacting with Wind River Tooling.  This common interface creates stability across multiple edge products and versions, allowing tooling such as extensions to third-party DevOps ecosystems, customer automation scripts, etc. to be developed and maintined with speed and ease. This also creates a modern and intuitive way to navigate product functionality across the broader Wind River ecosystem.

## Concepts

The CLI is broken down into 
`wr`


versioning

## Core Components

The Wind River CLI includes four default components, which aserve as a base for all additional functionality.

### `package`

The `package` component serves as an interface to Wind River's WindShare Package Management, and provides the ability to browse and install packages.

### `extension`

In `wr`, extensions are sets of functionality added to the CLI for various edge products.  When packages are installed, the extension is added automatically after the fact.  However, for use on systems with existing product installations, the extensions can be added manually to provide the same common interface for interaction.

The `extension` component provides the ability to install 

### `license`
### `serve`

### Common Features
## Components
## Structure
