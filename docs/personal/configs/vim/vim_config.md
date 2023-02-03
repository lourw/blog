---
title: Vim Configuration
---

## Plugin Manager
I like to use vim plug
``` vim title=".vimrc"
call plug#begin()

Plug 'scrooloose/nerdtree'

call plug#end()
```

## Themeing
```
colorscheme slate
```

## Use Configurations
```
set mouse=a
nnoremap <LeftMouse> <LeftMouse>a
syntax on
```

## Typing Experience
```
set cursorline
set cursorcolumn
```

## Sample .vimrc
``` vim title=".vimrc"
# --- Base Configs ---
syntax on
set mouse=a
nnoremap <LeftMouse> <LeftMouse>a

set cursorline
set cursorcolumn

# --- Themeing ---
coloscheme slate

# --- Plugins ---
call plug#begin()

Plug 'scrooloose/nerdtree'

call plug#end()
```

```
