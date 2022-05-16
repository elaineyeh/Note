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
### Install appoint version of Python
```shell
$ pyenv install X.X.X
```
If you got an error like the below, just follow the step.
```
./Modules/posixmodule.c:9084:15: error: implicit declaration of function 'sendfile' is invalid in C99 [-Werror,-Wimplicit-function-declaration]
ret = sendfile(in, out, offset, &sbytes, &sf, flags);

1 error generated.
make: *** [Modules/posixmodule.o] Error 1
make: *** Waiting for unfinished jobs....
1 warning generated.
```
#### Step 1. Reinstall zlib bzip2
```shell
brew reinstall zlib bzip2
```
#### Step 2. Update .zshrc or .bashrc dependent on your shell
Just put it to the last line in the file.
```shell
export PATH="$HOME/.pyenv/bin:$PATH"
export PATH="/usr/local/bin:$PATH"

eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
export LDFLAGS="-L/usr/local/opt/zlib/lib -L/usr/local/opt/bzip2/lib"
export CPPFLAGS="-I/usr/local/opt/zlib/include -I/usr/local/opt/bzip2/include"
```
#### Step 3. Tnstall python X.X.X, you can change the version
```shell
CFLAGS="-I$(brew --prefix openssl)/include -I$(brew --prefix bzip2)/include -I$(brew --prefix readline)/include -I$(xcrun --show-sdk-path)/usr/include" LDFLAGS="-L$(brew --prefix openssl)/lib -L$(brew --prefix readline)/lib -L$(brew --prefix zlib)/lib -L$(brew --prefix bzip2)/lib" pyenv install --patch X.X.X < <(curl -sSL https://github.com/python/cpython/commit/8ea6353.patch\?full_index\=1)
```
### Check how many version we have
```shell
$ pyenv versions
```
### Set the folder of Python version
```shell
$ pyenv local X.X.X
```
If it doesn't work, just try it:
```shell
eval "$(pyenv init -)"
```
### Check the version of Python
```shell
$ python -V
```

---
## Reference
[Can't install Python version](https://lifesaver.codes/answer/unable-to-install-python-3-8-0-on-macox-11-1740)

[Can't switch Python version with pyenv](https://stackoverflow.com/questions/33321312/cannot-switch-python-with-pyenv)