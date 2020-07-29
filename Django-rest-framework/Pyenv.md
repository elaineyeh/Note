# Pyenv
Can easily switch between multiple versions of Python.
## Install
**Step 1. Install**
```shell
$ brew install pyenv
```
**Step 2. Set environment variables**
Paste the command below into `~/.zshrc`(zsh) or `~/.bashrc`(bash)
```shell
PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```
**Step 3.** Restart Terminal
```
command + Q
```
**Tips:** If you restart terminal have the error `no such command 'virtualenv-list'`, just annotation the environment variables `eval "$(pyenv virtualenv-init -)"`

## Use
Install appoint version of Python
```shell
$ pyenv install X.X.X
```
Check how many version we have
```shell
$ pyenv versions
```
Set the folder of Python version
```shell
$ pyenv local X.X.X
```
Check the version of Python
```shell
$ python -V
