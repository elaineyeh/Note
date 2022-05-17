# Venv and Pipenv
## Venv
Venv is a virtual environment manager for Python 3 or 2.
### Install
```shell
$ pip install virtualenv
```
### Use
```shell
$ cd YOUR-PORJECT-FOLDER
```
Create virtual environment
```shell
$ python -m venv env
```
Get into venv
```shell
$ source ./env/bin/activate
```
Leave venv
```shell
$ deactivate
```

## Pipenv
The same as `venv & requirements.txt`, but pipenv can track the package automatically.
### Install
```shell
$ pip install pipenv
```
Or you can use with `brew`
```shell
$ brew install pipenv
```

### Use
#### Check installation
```shell
$ which pipenv
```
#### Check pipenv version
```shell
$ pipenv --version
```
#### Create pipenv or Get into pipenv
```shell
$ cd YOUR-PORJECT-FOLDER
```
```shell
$ pipenv shell
```
If pipenv doesn't catch pyenv version, try this:
```shell
$ pipenv --python 3.8.3 shell
```
If pipenv shell already have the same folder name, try this:
```shell
$ pipenv --rm
```
#### Leave pipenv
```shell
$ exit
```
