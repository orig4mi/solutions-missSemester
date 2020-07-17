[Home](README.md)


# Lecture 3: Editors (Vim) 

1. > Complete `vimtutor`. Note: it looks best in a
   > [80x24](https://en.wikipedia.org/wiki/VT100) (80 columns by 24 lines)
   > terminal window.
    >
    Done
1. > Download our [basic vimrc](/2020/files/vimrc) and save it to `~/.vimrc`. Read
   > through the well-commented file (using Vim!), and observe how Vim looks and
   > behaves slightly differently with the new config.
    >
    Done
1. > Install and configure a plugin:
   > [ctrlp.vim](https://github.com/ctrlpvim/ctrlp.vim).
   > 1. Create the plugins directory with `mkdir -p ~/.vim/pack/vendor/start`
   > 1. Download the plugin: `cd ~/.vim/pack/vendor/start; git clone
   >    https://github.com/ctrlpvim/ctrlp.vim`
   > 1. Read the
   >    [documentation](https://github.com/ctrlpvim/ctrlp.vim/blob/master/readme.md)
   >    for the plugin. Try using CtrlP to locate a file by navigating to a
   >    project directory, opening Vim, and using the Vim command-line to start
   >    `:CtrlP`.
   > 1. Customize CtrlP by adding
   >    [configuration](https://github.com/ctrlpvim/ctrlp.vim/blob/master/readme.md#basic-options)
   >    to your `~/.vimrc` to open CtrlP by pressing Ctrl-P.
   >
    Done. There isn't anything to do in step iv, Ctrl-P is the default keyboard shortcut.
1. > To practice using Vim, re-do the [Demo](#demo) from lecture on your own
   machine.
    >
    Done
1. > Use Vim for _all_ your text editing for the next month. Whenever something
   > seems inefficient, or when you think "there must be a better way", try
   > Googling it, there probably is. If you get stuck, come to office hours or
   > send us an email.
>
    Ongoing
1. > Configure your other tools to use Vim bindings (see instructions above).
    >
    Added to .zshrc:
    ```bash
    export EDITOR=vim
    ```
1. > Further customize your `~/.vimrc` and install more plugins.
   >
    Installed NERDTree:
    ```bash
    git clone https://github.com/preservim/nerdtree.git ~/.vim/pack/vendor/start/nerdtree
    vim -u NONE -c "helptags ~/.vim/pack/vendor/start/nerdtree/doc" -c q
    ```
1. > (Advanced) Convert XML to JSON ([example file](/2020/files/example-data.xml))
   > using Vim macros. Try to do this on your own, but you can look at the
   > [macros](#macros) section above if you get stuck.
   >
   Skipped
