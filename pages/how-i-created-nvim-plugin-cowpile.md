# HOW I CREATED NVIM PLUGIN | FEAT. COWPILE

**neovim or nvim** is a keyboard-based text editor, without which I literally can't live. It got deeply intertwined with my workflow. One main advantage of Neovim is the ability to write nvim-plugins with lua language. 
Lua itself is a really simple, lightweight and powerful programming language which makes neovim highly extensible. In this blog I am going to walk you through how I created a really simple plugin "COWPILE" to run compile commands within nvim.
<br />

## COWPILE.nvim 
#### **# What problem it intents to solve?**
I code in various programming languages includes c/c++, haskell, python ... Each of the projects have different compile commands and execution commands. Even for c/c++ projects, some use conan, CMake, make or just script files to compile.
I had to run `:!command cmake -S . -B build && cmake --build build`, but these commands change for each project. To save my keystrokes I had to build my plugin.

#### **# How it works?**
create a `.sln.lua` file, and have the following contents:
```lua 
return {
    gen = " rm -rf build && conan install . --output-folder=. --build=missing && cmake --preset conan-release ",
    bld = " cmake --build build/Release ",
    exe = " ./build/Release/src/machine_learning ",
}
```
**COMMANDS** 
- `:CowpileAndRebuild` - runs the *gen (generation) + bld (build) + exe (execution)* script in nvim terminal
- `:CowpileAndBuild` - runs the *bld (build) + exe (execution)* script in nvim terminal
- `:CowpileAndRun` -  just runs the execution command
<br />
<br />

## How to build your own plugin
The *project structure* for any plugin,

```bash
cowpile.nvim
├── lua
│   └── cowpile
│       └── init.lua
└── plugin
    └── cowpile.lua
```

**lua directory** - will be added to nvim runtime, therefore you can call `:<command>`
**plugin directory** - its contents will be executed when you open nvim, stuff like version checking will be done here.

If your are familiar with lua language, you know `.lua` files modules that generally return a table. Most nvim plugins out there have a table M with a setup function and return it.
```lua
local M = {}

local function <any_helper_function> ()
    -- DO YOUR STUFF HERE
end

M.setup = function(opts)
    -- DO YOUR STUFF HERE
end

return M
```
<br />

## how to install and use,
well, i just put my `cowpile.nvim` directory inside `.config/nvim` and `require('cowpile.nvim').setup()` (works if only your plugin table have setup function).
or if you are using package manager like lazy, then return dir key.
```lua
return {
    { dir = "~/path/to/plugin.nvim" }
}
```

<br />

## nvim functions that i used
```lua
vim.notify(<error_msg: string, vim.log.levels.<LEVEL>) -- prints on status line
vim.api.nvim_create_user_command(<cmd: string>, <callback: func>, <opts: table>) -- execute nvim commands 
vim.api.nvim_command(<cmd: string>) -- execute nvim commands 
vim.fn.termopen(<cmd: string>) -- opens nvim terminal, and runs cmd
```
<br />
<br />

## **Resources that i've found useful**
[X in Y minutes for lua](https://learnxinyminutes.com/lua/)
[Scripting Neovim with Lua](https://jacobsimpson.github.io/nvim-lua-manual/docs/basic-plugin/)
[TJ's Plugin video](https://www.youtube.com/watch?v=VGid4aN25iI)
[TJ's Lua video](https://youtu.be/CuWfgiwI73Q?si=S6umbvU8cdtgBa72)

