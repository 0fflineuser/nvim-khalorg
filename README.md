# README - nvim-khalorg

[Click here for the GitHub page.](https://github.com/BartSte/nvim-khalorg)

Plugin to interact with `khalorg`: an interface between the org mode and the
`khal` cli calendar. If you never heard of `khalorg`, take a look at the
[GitHub page](https://github.com/BartSte/khalorg)

# Demo

The demo below demonstrates the following features using this neovim plugin:

- `khalorg new`: convert an org agenda item into a `khal` agenda item.
- `khalorg list`: convert a `khal` agenda item into an org agenda item.
- `khalorg edit`: edit an existing `khal` agenda item with org mode.
- `khalorg delete`: delete an existing `khal` item.

![neovim-plugin](https://github.com/BartSte/khalorg/blob/main/demo/neovim-plugin.gif?raw=true)

# CONTENTS

1. [Introduction](#introduction)
2. [Installation](#installation)
3. [Configuration](#configuration)
4. [Usage](#usage)
5. [Functions](#functions)
6. [Troubleshooting](#troubleshooting)
7. [Contributing](#contributing)
8. [License](#license)

# Introduction

The `nvim-khalorg` plugin sends folds in an org document to khalorg through
stdin. The functions are exposed through `nvim-orgmode` its custom export option.

# Installation

Install using your favorite plugin manager. For example packer:

```lua
use {'nvim-treesitter/nvim-treesitter'}
use {'nvim-orgmode/orgmode'}
use {'BartSte/nvim-khalorg'}
```

where `nvim-treesitter` and `nvim-orgmode` are required. Also, make sure you
installed `khalorg`, which can be found here: https://github.com/BartSte/khalorg

# Configuration

The following configuration options are available through the `require("khalorg").setup`
function:

- calendar: The name of the calendar to use (default: 'default').

Configuring `nvim-khalorg` can be done by placing the following in your
`init.lua`:

```lua
require("khalorg").setup({
    calendar = 'my_calendar'
})
```

where you need to replace `'my_calendar'` with the `khal` calendar you want to
use. You can add the export functions of `nvim-khalorg` to `nvim-orgmode` by
adding the following to your `init.lua`:

```lua
local khalorg = require('khalorg')
orgmode.setup_ts_grammar()
orgmode.setup({
    org_custom_exports = {
        n = { label = 'Add a new khal item', action = khalorg.new },
        d = { label = 'Delete a khal item', action = khalorg.delete },
        e = { label = 'Edit properties of a khal item', action = khalorg.edit },
        E = { label = 'Edit properties & dates of a khal item', action = khalorg.edit_all }
    }
})
```

# Usage

After configuring `nvim-khalorg` as is described above, you can access them
through `orgmode-org_export` (default mapping: `<leader>oe`). More information
can be found by running `:help orgmode-org_export`.

# Functions

The following functions are provided that send a fold in an org file to a
khalorg command through stdin:

- `require("khalorg").new()`: add a new item to khal.
- `require("khalorg").delete()`: delete a khal item.
- `require("khalorg").edit_props()`: edit properties of a khal item, the dates are not updated.
- `require("khalorg").edit_all()`: edit properties of a khal item together with the dates.

The following function is used to create the functions above and can be used to
make your own khalorg export functions.

- `require("khalorg").make_exporter(<khalorg_command>)`:  
  Returns a function that, when called, does the following:
  1. Get the current fold.
  2. Get the start and end line of the fold.
  3. Get the text of the fold.
  4. Send the text of the fold to `<khalorg_command>` through stdin.

The example below shows how to use `khalorg.make_exporter` function to create
the `khalorg.new` function:

```lua
new = khalorg.make_exporter('khalorg new my_calendar')
```

# Troubleshooting

If you encounter any issues, please report them on the issue tracker at:
[nvim-khalorg issues](https://github.com/BartSte/nvim-khalorg/issues)

If you think the issue arises from khalorg instead of nvim-khalorg, please
report them here: [khalorg issues](https://github.com/BartSte/khalorg/issues)

# Contributing

Contributions are welcome! Please see [CONTRIBUTING](./CONTRIBUTING.md) for
more information.

# License

Distributed under the MIT License.
