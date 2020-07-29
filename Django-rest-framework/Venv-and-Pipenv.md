# Venv and Pipenv
## Venv
Venv is a virtual environment manager for Python 3 or 2.
### Install
```shell
$ pip install virtualenv
```
### Use
```shell
$ cd your-project-folder
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
Or you can use with `brew
```shell
$ brew install pipenv
```

### Use
Check installation
```shell
$ which pipenv
```
Check pipenv version
```shell
$ pipenv -V
```
Create pipenv or Get into pipenv
```shell
$ cd your-project-folder
```
```shell
$ pipenv shell
```
Leave pipenv
```shell
$ exit
```
