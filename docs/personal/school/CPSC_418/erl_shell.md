---
title: Erlang Shell
description: Overview of the CLI interface to execute Erlang
---

This a collection of tips and tricks for myself to manipulate the Erlang shell and make it easier to use. 

## Setup
### Installation
Refer to official [erlang docs](https://www.erlang.org/downloads)  

### Erlang Shell Configuration

The `erl` shell by default does not save history of commands entered. To enable this feature, add the following line to your shell profile

```sh
export ERL_AFLAGS="-kernel shell_history enabled"
```

## Compiling and Running Code
Erlang is written in modules that you can import into the `erl` shell. When you first initialize `erl`, your base directory is whichever folder you called the command on. To navigate to the location of your desired `.erl` file, use:

```erlang
Eshell
> cd("path/to/erl/file").
```

Then to import the module into the erl shell, run:

```erlang
Eshell
> c("file.erl").
```

### Compiling with debugger
To ensure the debugger turns on when running, compile using:

```erlang
Eshell
> c("file.erl", [debug_info]).
> debug.start().
```

### Other tips
In functional programming you cannot reassign variables once values are set. You can do this in the erlang shell:

```erlang
Eshell
> X = 10.
> f(X). % reset X so that you can bind a different value
```
