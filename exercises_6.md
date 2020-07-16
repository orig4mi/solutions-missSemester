[Home](README.md)

# Lecture 6: Version Control (Git) 

1. > If you don't have any past experience with Git, either try reading the first
   > couple chapters of [Pro Git](https://git-scm.com/book/en/v2) or go through a
   > tutorial like [Learn Git Branching](https://learngitbranching.js.org/). As
   > you're working through it, relate Git commands to the data model.
1. > Clone the [repository for the
> class website](https://github.com/missing-semester/missing-semester).
>
```bash
~$ git clone https://github.com/missing-semester/missing-semester missing-semester
```
    1. > Explore the version history by visualizing it as a graph.
>
```bash
~$ git log --graph --all --decorate
```
    1. > Who was the last person to modify `README.md`? (Hint: use `git log` with
       > an argument)
>
```bash
~$ git log README.md
```
    1. > What was the commit message associated with the last modification to the
       > `collections:` line of `_config.yml`? (Hint: use `git blame` and `git
       > show`)
>
```bash
~$ git blame _config.yml
~$ git show a88b4
```
1. > One common mistake when learning Git is to commit large files that should
  >  not be managed by Git or adding sensitive information. Try adding a file to
  >  a repository, making some commits and then deleting that file from history
  >  (you may want to look at
  >  [this](https://help.github.com/articles/removing-sensitive-data-from-a-repository/)).
>
```bash
~$ git filter-branch --force --index-filter \
    "git rm --cached --ignore-unmatch testfile" --prune-empty --tag-name-filter cat -- --all
```
1.>  Clone some repository from GitHub, and modify one of its existing files.
  >  What happens when you do `git stash`? What do you see when running `git log
  >  --all --oneline`? Run `git stash pop` to undo what you did with `git stash`.
  >  In what scenario might this be useful?
>
`git stash` saves modifications and reverts the working directory. `git log --all --oneline` shows the stash in the log history with the place in which itwas stored (`(refs/stash)`), the name of the branch in which it was done and a message of the associated commit. It is useful to momentarily interrupt a current workflow and store away modifications, when an inmediate return to the original branch is required for a quick work on something else.
1.>  Like many command line tools, Git provides a configuration file (or dotfile)
  >  called `~/.gitconfig`. Create an alias in `~/.gitconfig` so that when you
  >  run `git graph`, you get the output of `git log --all --graph --decorate
  >  --oneline`.
>
```bash
~$ git config --global alias.graph 'got log --all --graph --decorate --oneline
```
1.>  You can define global ignore patterns in `~/.gitignore_global` after running
  >  `git config --global core.excludesfile ~/.gitignore_global`. Do this, and
  >  set up your global gitignore file to ignore OS-specific or editor-specific
  >  temporary files, like `.DS_Store`.
>
```bash
~$ git config --global core.excludesfile ~/.gitignore_global
~$ echo .DS_Store > ~/.gitignore_global
```
1.>  Fork the [repository for the class
  >  website](https://github.com/missing-semester/missing-semester), find a typo
  >  or some other improvement you can make, and submit a pull request on GitHub.
>
On GitHub, go to the class repository and click "Fork". Next:
```bash
~$ git clone https://github.com/orig4mi/missing-semester
~$ cd missing-semester
~$ git checkout -b new_branch
~$ git remote add upstream https://github.com/missing-semester/missing-semester
...(make improvements and commit)...
~$ git push -u origin new_branch
```
Go to GitHub, click on "Compare & pull request", fill the form and click "Create pull request".
