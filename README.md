**This is currently a work-in-progress**

# FrescoRaja Themes for [Vim](http://www.vim.org)

> A vim plugin wrapper that provides a simple interface to dynamically control several of Vim's visual elements and
> behavior: cursor, textwidth and cursorcolumn, font, background, and colorscheme.

> **Note:** This is for my own educational use, if you want to try it go ahead but at your own risk.. It is my first
> time attempting to create a Vim plugin and I take no responsibility for any consequences of its use on your system.

## Functionality

This plugin has exposed several functions that make it easy to control certain Vim options and visual settings:

### Loading Customized Themes

Use `CustomizeTheme` to load specific theme of choice. Themes are a complementary colorscheme/vim-airline theme
combination, along with some highlighting tweaks I felt were beneficial.

Use `<Plug>(customize_theme)` to bring up autocompletion menu with available themes to load.
Use `<Plug>(cycle_custom_themes_next)` to cycle forwards through available customized themes.
Use `<Plug>(cycle_custom_thems_prev)` to cycle backwards through available customized themes.

```viml
" This function is mapped to the <Nul> character sequence (Ctrl + <Space>) if that mapping has not been defined already.
" It provides tab-autocompletion for selecting themes.
<Plug>(customize_theme)

" Define your own mappings:
nmap <Leader>ct <Plug>(customize_theme)
nmap <F7> <Plug>(cycle_custom_themes_prev)
nmap <F9> <Plug>(cycle_custom_themes_next)

" Load a specific theme:
nmap <F12> :CustomizeTheme material<CR>
```

**Note** if you add a new supported colorscheme during a session, or if for some reason the list of themes disappears,
you need to refresh the list of available themes. I've provided a method to do this: `<Plug>(refresh_theme_list)` or
`:RefreshThemeList`

### Toggle Background Color and Transparency

Use `<Plug>(toggle_dark)` to toggle Vim `background` option value between *dark* and *light*

Use `<Plug>(toggle_background)` to toggle background color between the colorscheme's defined background and the
background color you have defined for your terminal. (sets `guibg/ctermbg` to *none*, making Vim background transparent)
Works in both gui mode and cterm mode.

### Toggle ColorColumn / Textwidth

Use `<Plug>(set_textwidth)` to set &textwidth value
Use `<Plug>(toggle_column)` to toggle the display of a highlighted cursorcolumn at `&textwidth` value

As an example, the following mapping would enable you to type ***tw=90<Enter>*** in normal mode to change the textwidth to 90.
(It will also automatically move the colorcolumn to 90 as well)

```viml
" Set textwidth
nmap tw= <Plug>(set_textwidth)
```

### Colorize/Italicize Comments, Colorize ColorColumn

Use `<Plug>(italicize)` to toggle italics mode for Comments and some other predefined syntax groups like HTML attribute args.
Use `<Plug>(set_comments_color)` to colorize Comments.
Use `<Plug>(set_column_color)` to colorize ColumnColor.

For example, the following mapping would enable you to type ***<Leader>cwhite<Enter>*** to change comments to white in cterm or
gui mode. (If providing a hex color value like #fafafa, surround with quotes when typing command and , ie
<Leader>c'#fafafa'<Enter> - the '#' is optional)

```viml
nmap <Leader>c <Plug>(set_comments_color)
```

You can toggle italics for any syntax group you'd like. Just use the `Italicize!` method followed by the syntax
group you want to toggle italics for. You can specify multiple groups separated by a comma. You can make a mapping if
you like to italicize specific groups frequently, or perhaps set and autocmd to do it for specific filetypes:

```viml
" Shift+F1 to toggle italics for comments, html attributes, WildMenu
nmap <S-F1> <Plug>(italicize)
" Shift+F2 to toggle italics for String, Statement, Identifier
nmap <S-F2> :Italicize! String,Statement,Identifier<CR>

" Automatically italicize keywords when opening javascript files
autocmd FileType javascript* Italicize! Identifier
```

### Get Syntax Highlighting group for term under cursor

`<Plug>(get_syntax)` This is useful for customizing themes and defining my own syntax highlighting colors. Will print
a statement in command line showing the highlight group name of the word under the cursor and which highlight group it
might be linked with, ie `vimCommand => Statement`

### Custom cursor shapes

Use ***g:custom_cursors_enabled*** to set the following cursors:
  - block in normal mode
  - vertical line in insert mode (appears between characters so it's easier to see precisely where characters will be
      inserted
  - underline in replace mode

```viml
" enable custom cursors
let g:custom_cursors_enabled=1
```

### Reset functions

Use the following globals to define custom default colors:
  - `g:default_comments_color_c` -> define comment color in cterm mode (0-255), defaults to 59 (grey)
  - `g:default_comments_color_g` -> define comment color in gui mode (hex RGB), defaults to #658494 (blue-grey)
  - `g:default_column_color_c` -> define column color in cterm mode, defaults to 236 (dark grey)
  - `g:default_column_color_g` -> define column color in gui mode, defaults to #2a2a2a (dark grey)
  - `g:default_textwidth` -> define textwidth, defaults to &texwidth
  - `g:default_airline_theme` -> define airline theme, default to g:airline_theme
  - `g:custom_themes_name` -> define custom theme to use, defaults to 'default'

Use `<Plug>(reset_theme)` to reset custom theme
Use `<Plug>(refresh_theme)` to reapply current custom theme
Use `<Plug>(reset_texwidth)` to reset textwidth
Use `<Plug>(reset_comments_color)` to reset comments
Use `<Plug>(reset_column_color)` to reset columncolor

## Installation

I use and highly recommend [vim-plug](https://github.com/junegunn/vim-plug), in which case you would add something like this to your
`.vimrc`:

```viml
call plug#begin('/your-plugin-path')

Plug 'frescoraja/frescoraja-vim-themes'

call plug#end()
```

Then, just run `:PlugInstall` from within Vim, or `vim -c PlugInstall` from the command line.

Alternatively, this repo can be cloned to a your plugin manager's defined location:

```shell
git clone https://github.com/frescoraja/frescoraja-vim-themes ~/.vim/bundle
```

To enable plugin functionality, set a global trigger in vimrc:

```viml
let g:custom_theme_enabled=1

" to set a default theme to load on startup:
let g:custom_theme_name='blayu'

" OR you can call initializer directly (after plugins sourced, ie `call plug#end()`)
call frescoraja#init()
```

## Dependencies

> This is the current list of themes used by this plugin:
> Only include the ones you want to use.

* #### VimAirline / VimAirline Themes

    - [vim-airline](https://github.com/bling/vim-airline)
    - [vim-airline-themes](https://github.com/vim-airline/vim-airline-themes)

* #### ColorSchemes

    - [gruvbox](https://github.com/morhetz/gruvbox)
    - [oceanic-next](https://github.com/mhartington/oceanic-next)
    - [onedark](https://github.com/joshdick/onedark.vim)
    - [maui](https://github.com/zsoltf/vim-maui)
    - [kafka/dark](https://github.com/Konstruktionist/vim)
    - [distill](https://github.com/deathlyfrantic/vim-distill)
    - [vim-material](https://github.com/hzchirs/vim-material)
    - [material](https://github.com/jscappini/material.vim)
    - [hybrid-material](https://github.com/kristijanhusak/vim-hybrid-material)
    - [neodark](https://github.com/KeitaNakamura/neodark.vim)
    - [quantum](https://github.com/tyrannicaltoucan/vim-quantum)
    - [molokai](https://github.com/tomasr/molokai)
    - [dracula](https://github.com/dracula/vim)
    - [blayu](https://github.com/tjammer/blayu)
    - [edar/elit](https://github.com/DrXVII/vim_colors)
    - [thaumaturge](https://github.com/baines/vim-colorscheme-thaumaturge)
    - [deus](https://github.com/ajmwagar/vim-deus)
    - [spring-night](https://github.com/rhysd/vim-color-spring-night)
    - [afterglow](https://github.com/danilo-augusto/vim-afterglow)
    - [gotham](https://github.com/whatyouhide/vim-gotham)
    - [material-theme](https://github.com/jdkanani/vim-material-theme)
    - [ceudah](https://github.com/emhaye/ceudah.vim)
    - [srcery](https://github.com/srcery-colors/srcery-vim)
    - [colorsbox](https://github.com/mkarmona/colorsbox)
    - [material-monokai](http://github.com/skielbasa/vim-material-monokai)
    - [tender](https://github.com/jacoborus/tender.vim)
    - [iceberg](https://github.com/cocopon/iceberg.vim)
    - The following colorschemes from [vim-colorschemes](https://github.com/flazz/vim-colorschemes)
        - CandyPaper
        - jellybeans
        - herokudoc-gvim
        - busybee
        - flatcolor
        - znake
        - orange-moon/pink-moon/yellow-moon

My appreciation goes to all the maintainers of above plugins/themes for their attention to aesthetics and detail.

---

[frescoraja-vim-themes](https://github.com/frescoraja/frescoraja-vim-themes)
