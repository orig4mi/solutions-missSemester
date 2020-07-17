[Home](README.md)

# Lecture 5: Command-line Environment 

### Job control

1. > From what we have seen, we can use some `ps aux | grep` commands to get our jobs' pids and then kill them, but there are better ways to do it. Start a `sleep 10000` job in a terminal, background it with `Ctrl-Z` and continue its execution with `bg`. Now use [`pgrep`](https://www.man7.org/linux/man-pages/man1/pgrep.1.html) to find its pid and [`pkill`](http://man7.org/linux/man-pages/man1/pgrep.1.html) to kill it without ever typing the pid itself. (Hint: use the `-af` flags).
  
   ```bash
   ~$ sleep 10000
      (Ctrl-Z)
   ~$ bg
   ~$ pgrep sleep
   ~$ pkill sleep
   ```

1. > Say you don't want to start a process until another completes, how you would go about it? In this exercise our limiting process will always be `sleep 60 &`.
   > One way to achieve this is to use the [`wait`](https://www.man7.org/linux/man-pages/man1/wait.1p.html) command. Try launching the sleep command and having an `ls` wait until the background process finishes.
    
   ```bash
   ~$ sleep 60 &
   ~$ wait | ls
   ```
   
   > However, this strategy will fail if we start in a different bash session, since `wait` only works for child processes. One feature we did not discuss in the notes is that the `kill` command's exit status will be zero on success and nonzero otherwise. `kill -0` does not send a signal but will give a nonzero exit status if the process does not exist.
   > Write a bash function called `pidwait` that takes a pid and waits until the given process completes. You should use `sleep` to avoid wasting CPU unnecessarily.
   
   ```bash
   #!/usr/bin/env bash
   pidwait () {
   while true
   so
       kill -0 "$1"
       if [[ $? -ne 0 ]]; then
           return 0
       fi
       sleep 1
   done
   }
   ```

### Terminal multiplexer

1. > Follow this `tmux` [tutorial](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) and then learn how to do some basic customizations following [these steps](https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/).
   
   Installed tmux:
   
   ```bash
   ~$ sudo apt-get install tmux
   ```
   
   Created a `.tmux.conf` file:
   
   ```
   # split panes using | and -
   bind | split-window -h
   bind - split-window -v
   unbind '"'
   unbind %
   
   # switch panes using Alt-arrow without prefix
   bind -n M-Left select-pane -L
   bind -n M-Right select-pane -R
   bind -n M-Up select-pane -U
   bind -n M-Down select-pane -D
   ```

### Aliases

1. > Create an alias `dc` that resolves to `cd` for when you type it wrongly.
   
   ```bash
   alias dc=cd
   ```
   
1.  > Run `history | awk '{$1="";print substr($0,2)}' | sort | uniq -c | sort -n | tail -n 10`  to get your top 10 most used commands and consider writing shorter aliases for them. Note: this works for Bash; if you're using ZSH, use `history 1` instead of just `history`.

      Done

### Dotfiles

> Let's get you up to speed with dotfiles.
1. > Create a folder for your dotfiles and set up version control
   
   ```bash
   ~$ mkdir .dotfiles
   ~$ cd .dotfiles
   ~$ git init
   ```
   
1. > Add a configuration for at least one program, e.g. your shell, with some
   > customization (to start off, it can be something as simple as customizing your shell prompt by setting `$PS1`).
   
   Created a .aliases file and added it to .bashrc and .zshrc:
   ```bash
   # Test if ~/.aliases exists and source it
   if [ -f ~/.aliases ]; then
       source ~/.aliases
   fi
   ```
   (I also added other configurations to .tmux.conf, .bashrc, .zshrc, .vimrc)
   
1. > Set up a method to install your dotfiles quickly (and without manual effort) on a new machine. This can be as simple as a shell script that calls `ln -s` for each file, or you could use a [specialized utility](https://dotfiles.github.io/utilities/).
   
   Installed dotbot from the link above, and set a `dotfiles` folder as my repository:
   ```bash
   ~$ mkdir dotfiles
   ~$ cd ~/dotfiles
   ~$ git init
   ~$ git submodule add https://github.com/anishathalye/dotbot
   ~$ git config -f .gitmodules submodule.dotbot.ignore dirty
   ~$ cp dotbot/tools/git-submodule/install .
   ~$ touch install.conf.yaml
   ```
   
1. > Test your installation script on a fresh virtual machine.
   
   Did this on a virtual machine (Debian guest on VMware Player) after Step 5 below:
   ```bash
   ~$ git clone https://github.com/orig4mi/dotfiles.git dotfiles
   ~$ cd dotfiles
   ~$ ./install
   ```
   
1. > Migrate all of your current tool configurations to your dotfiles repository.
    
   Moved my dotfiles to the repository folder, and also plugins for oh-my-zsh. In dotfiles folder I created an `install.conf.yaml`:
   
   ```
    - defaults:
        link:
            relink: true
            create: true
   
    - clean: ['~']
 
    - link:
        ~/.aliases:
        ~/.bashrc:
        ~/.dotfiles: ''
        ~/.gitconfig: 
        ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions: oh-my-zsh/custom/plugins/zsh-autosuggestions 
        ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting: oh-my-zsh/custom/plugins/zsh-syntax-highlighting
         ~/.profile:
        ~/.tmux.conf:
        ~/.vim:
        ~/.vimrc:
        ~/.zshrc:
 
    - create:
        - ~/.vim/undo-history
 
    - shell:
        - git submodule update --init --recursive``
   ```
   
   Then, I added the plugins for oh-my-zsh as git submodules:
   ```bash
   ~$ git submodule add https://github.com/zsh-users/zsh-syntax-highlighting.git \
   > oh-my-zsh/custom/plugins/zsh-syntax-highlighting
   ~$ git submodule add https://github.com/zsh-users/zsh-autosuggestions \
   > oh-my-zsh/custom/plugins/zsh-autosuggestions 
   ```
   
   Finally, added all to git and committed:
   ```bash
   ~$ git add .
   ~$ git commit -m "First commit"
   ```

1. > Publish your dotfiles on GitHub.
   
   Added a README.md file. Then, created a respository at [https://github.com/orig4mi/dotfiles]() and uploaded my local repository:
   ```bash
   ~$ git remote add origin https://github.com/orig4mi/dotfiles.git
   ~$ git push -u origin master
   ```

## Remote Machines

> Install a Linux virtual machine (or use an already existing one) for this exercise. If you are not familiar with virtual machines check out [this](https://hibbard.eu/install-ubuntu-virtual-box/) tutorial for installing one.

1. > Go to `~/.ssh/` and check if you have a pair of SSH keys there. If not, generate them with `ssh-keygen -o -a 100 -t ed25519`. It is recommended that you use a password and use `ssh-agent` , more info [here](https://www.ssh.com/ssh/agent).
   
   ```bash
   ~$ eval `ssh-agent`
   ~$ ssh-keygen -o -a 100 -t ed25519
   ```
   
1. > Edit `.ssh/config` to have an entry as follows
   >
   >  ```bash
   > Host vm
   >     User username_goes_here
   >     HostName ip_goes_here
   >     IdentityFile ~/.ssh/id_ed25519
   >     LocalForward 9999 localhost:8888
   > ```
   
   Done
   
1. > Use `ssh-copy-id vm` to copy your ssh key to the server.
   
   Done
   
1. > Start a webserver in your VM by executing `python -m http.server 8888`. Access the VM webserver by navigating to `http://localhost:9999` in your machine.
   
   Done (the exercise should explain that one has to be logged into the VM, via ssh, in order to access `http://localhost:9999`)
   
1. > Edit your SSH server config by doing  `sudo vim /etc/ssh/sshd_config` and disable password authentication by editing the value of `PasswordAuthentication`. Disable root login by editing the value of `PermitRootLogin`. Restart the `ssh` service with `sudo service sshd restart`. Try sshing in again.
   
   Done. I can log in the VM using ssh keys with `ssh vm` but not without them: `ssh (user)@vm -o PubkeyAuthentication=no`, nor as root: `ssh root@vm`.
   
1. > (Challenge) Install [`mosh`](https://mosh.org/) in the VM and establish a connection. Then disconnect the network adapter of the server/VM. Can mosh properly recover from it?
   
   Installed mosh in both host and VM. Then logged into the VM with `mosh vm`', disconnected the network adapter in the VM, reconnected, 
   and mosh recovered the connection to the VM.
   
1. > (Challenge) Look into what the `-N` and `-f` flags do in `ssh` and figure out what a command to achieve background port forwarding.
    
   ```bash
   ~$ ssh -fN vm
   ```
