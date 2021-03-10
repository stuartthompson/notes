# vim-plug

A plugin manager for vim *(works with Neovim)*.

### Repository

https://github.com/junegunn/vim-plug

### Plugin Installation

Most plugins supported by vim-plug describe how to add their configuration to 
the vim init file. An example is the lightline plugin that is installed with:
```
Plug 'itchyny/lightline.vim'
```

Once a plugin has been added to the vim init file, it can be installed by 
running the following command: *(remember to source the init file first)*
```
:PlugInstall
```

### Plugin Loading

The vim-plug plugin is placed in the autoload directory, at:
```
~/.local/share/nvim/site/autoload/plug.vim
```

When neovim parses init.vim, the plug#begin and plug#end sections describe the
plugins to load. The vim-plug plugin parses the plugins within that block, 
loading each one in turn.

``` mermaid
graph LR;
 init["init.vim"]
 plug["autoload/plug.vim"]
 pb["plug#begin"]
 pe["plug#end"]

 subgraph autoload [vim autoload]
 nvim-->plug;
 end
 nvim-->init;
 init-->plug;
 init-->pb;
 subgraph plugs [vim-plug plugins]
 pb-->plugins;
 plugins-->pe;
 end;
```

### Example Config Block

The following is an example of the configuration section for vim-plug in the 
vim init file.

>~/.config/nvim/init.vim
```
call plug#begin('~/.local/share/nvim/site/plugged')

Plug 'w0rp/ale'
Plug 'itchyny/lightline.vim'

call plug#end()
```
