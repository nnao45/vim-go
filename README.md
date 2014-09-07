# vim-go

Go (golang) support for Vim. Contains official misc/vim files. It comes with
pre-defined sensible settings (like auto gofmt on save), has autocomplete,
snippet support, improved syntax highlighting, go toolchain commands, etc...
If needed vim-go installs all necessary binaries for providing seamless Vim
integration with current commands.  It's highly customizable and each
individual feature can be disabled/enabled easily.

![vim-go](https://dl.dropboxusercontent.com/u/174404/vim-go.png)

## Features

* Improved Syntax highlighting, such as Functions, Operators, Methods..
* Auto completion support via `gocode`
* Better `gofmt` on save, keeps cursor position and doesn't break your undo
  history
* Go to symbol/declaration with `godef`
* Look up documentation with `godoc` inside Vim or open it in browser.
* Automatically import packages via `goimports`
* Compile and `go build` your package, install it with `go install`
* `go run` quickly your current file/files
* Run `go test` and see any errors in quickfix window
* Create a coverage profile and display annotated source code in browser to see
  which functions are covered.
* Lint your code with `golint`
* Run your code through `go vet` to catch static errors.
* Advanced source analysis tool with `oracle`
* List all source files and dependencies
* Checking with `errcheck` for unchecked errors.
* Integrated and improved snippets. Supports `ultisnips` or `neosnippet`
* Share your current code to [play.golang.org](http://play.golang.org)
* On-the-fly type information about the word under the cursor
* Tagbar support to show tags of the source code in a sidebar with `gotags`

## Install

First of all, do not use it with other Go plugins. It already contains the
official misc/vim. If you use pathogen, just clone it into your bundle
directory:

```bash
$ cd ~/.vim/bundle
$ git clone https://github.com/fatih/vim-go.git
```

For Vundle add this line to your vimrc:

```vimrc
Plugin 'fatih/vim-go'
```
and execute `:PluginInstall` (or `:BundleInstall` for older versions of Vundle)

Please be sure all necessary binares are installed (such as `gocode`, `godef`,
`goimportes`, etc..). You can easily install them with the included
`:GoInstallBinaries`. Those binaries will be automatically downloaded and
installed to your `$GOBIN` environment (if not set it will use `$GOPATH/bin`).
It requires `git` and `hg` for fetching the individual Go packages.

### Optional

* Autocompletion is enabled by default via `<C-x><C-o>`, to get real-time
completion (completion by type) install:
[YCM](https://github.com/Valloric/YouCompleteMe) or
[neocomplete](https://github.com/Shougo/neocomplete.vim).
* To get displayed source code tag informations on a sidebar install
[tagbar](https://github.com/majutsushi/tagbar).
* For snippet feature install:
[ultisnips](https://github.com/SirVer/ultisnips) or
[neosnippet](https://github.com/Shougo/neosnippet.vim).
* Screenshot color scheme is a slightly modified molokai: [fatih/molokai](https://github.com/fatih/molokai).

## Usage

All [features](#features) are enabled by default. There are no additional
settings needed.  Usage and commands are listed in `doc/vim-go.txt`. Just open
the help page to see all commands:

    :help vim-go


## Mappings

vim-go has several `<Plug>` mappings which can be used to create custom
mappings. Below are some examples you might find useful:

Show type info for the word under your cursor with `<leader>i` (useful if you
have disabled auto showing type info via `g:go_auto_type_info`)

```vim
au FileType go nmap <Leader>i <Plug>(go-info)
```

Open the relevant Godoc for the word under the cursor with `<leader>gd` or open
it vertically with `<leader>gv`

```vim
au FileType go nmap <Leader>gd <Plug>(go-doc)
au FileType go nmap <Leader>gv <Plug>(go-doc-vertical)
```

Or open the Godoc in browser

```vim
au FileType go nmap <Leader>gb <Plug>(go-doc-browser)
```

Run commands, such as  `go run` with `<leader>r` for the current file or `go
build` and `go test` for the current package with `<leader>b` and `<leader>t`.
Display a beautiful annotated source code to see which functions are covered
with `<leader>c`.

```vim
au FileType go nmap <leader>r <Plug>(go-run)
au FileType go nmap <leader>b <Plug>(go-build)
au FileType go nmap <leader>t <Plug>(go-test)
au FileType go nmap <leader>c <Plug>(go-coverage)
```

Replace `gd` (Goto Declaration) for the word under your cursor (replaces current buffer):

```vim
au FileType go nmap gd <Plug>(go-def)
```

Or open the definition/declaration in a new vertical, horizontal or tab for the
word under your cursor:

```vim
au FileType go nmap <Leader>ds <Plug>(go-def-split)
au FileType go nmap <Leader>dv <Plug>(go-def-vertical)
au FileType go nmap <Leader>dt <Plug>(go-def-tab)
```

More `<Plug>` mappings can be seen with `:he go-mappings`. Also these are just
recommendations, you are free to create more advanced mappings or functions
based on `:he go-commands`.

## Settings
Below are some settings you might find useful. For the full list see `:he go-settings`.

Disable opening browser after posting to your snippet to `play.golang.org`:

```vim
let g:go_play_open_browser = 0
```

By default vim-go shows errors for the fmt command, to disable it:

```vim
let g:go_fmt_fail_silently = 1
```

Enable goimports to automatically insert import paths instead of gofmt:

```vim
let g:go_fmt_command = "goimports"
```

Disable auto fmt on save:

```vim
let g:go_fmt_autosave = 0
```

## Snippets

Snippets are useful and very powerful. By default ultisnips is
used, however you can change it to neosnippet with:

```vim
let g:go_snippet_engine = "neosnippet"
```

Snippet feature is enabled only if the snippet plugins are installed.  Below are
some examples snippets and the corresponding trigger keywords, The `|`
character defines the cursor. Ultisnips has support for multiple cursors


`ff` is useful for debugging:

```go
fmt.Printf(" | %+v\n", |)
```

`errn` expands to:

```go
if err != nil {
    return err
}
```

Use `gof` to quickly create a anonymous goroutine :

```go
go func() {
    |
}()
```

To add `json` tags to a struct field, use `json` trigger:

```
type foo struct {
    bar string  `json:"myField"
           ^ type `json` here, hit tab and type "myField". It will expand to `json:"myField"`
}
```

...

And many more! For the full list have a look at the
[included snippets](https://github.com/fatih/vim-go/blob/master/gosnippets/):

## Troubleshooting

### I'm using Fish shell but have some problems using Vim-go

First environment variables in Fish are applied differently, it should be like:

	set -x GOPATH /your/own/gopath

Second, Vim needs a POSIX compatible shell (more info here:
https://github.com/dag/vim-fish#teach-a-vim-to-fish). If you use Fish to open
vim, it will make certainx shell based commands fail (means vim-go will fail
too). To overcome this problem change the default shell by adding the following
into your .vimrc (on the top of the file):

	if $SHELL =~ 'fish'
	  set shell='/bin/sh'
	endif

or

	set shell='/bin/sh'



### I'm seeing weirds errors during the startup

If you see errors like this:

	Installing code.google.com/p/go.tools/cmd/goimports Error installing code.google.com/p/go.tools/cmd/goimports:
	Installing code.google.com/p/rog-go/exp/cmd/godef Error installing code.google.com/p/rog-go/exp/cmd/godef:

that means your local Go setup is broken or the remote website is down.  For
example sometimes code.google.com times out. To test, just execute a simple go
get:

	go get code.google.com/p/go.tools/cmd/goimports

You'll see a more detailed error. If this works, vim-go will work too.


## Why another plugin?

This plugin/package is born mainly from frustration. I had to re-install my Vim
plugins and especially for Go I had to install a lot of separate different
plugins, setup the necessary binaries to make them work together and hope not
to lose them again. Lots of plugins out there lack proper settings.
This plugin is improved and contains all my fixes/changes that I'm using for
months under heavy go development environment.

Give it a try. I hope you like it. Feel free to contribute to the project.

## Credits

* Go Authors for official vim plugins
* Gocode, Godef, Golint, Oracle, Goimports, Gotags, Errcheck projects and authors of those projects.
* Other vim-plugins, thanks for inspiration (vim-golang, go.vim, vim-gocode, vim-godef)
