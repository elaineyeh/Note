# Serialization
## Project set up
### Create a virtual environment and use it
```shell=
$ python3 -m venv env
$ source env/bin/active
```
virtual environment will keep our package configuration isolated from the others project.

### Install Django and Django REST framework
```shell=
$ pip install django
$ pip install djangorestframework
$ pip install pygments
```

### Set up a new project and application
```shell=
$ django-admin startproject tutorial . # <- There have a dot
$ cd tutorial
$ django-admin startapp quickstart
$ cd ..
```
If you don't want to create directory name the same, don't use the dot. Otherwise use it.

### Create an app
```shell=
$ python3 manage.ph startapp snippets
```

### Add the new app and rest_framework to INSTALLED_APPS in the setting.py
Edit in `setting.py`
```shell=
INSTALLED_APPS = [
    ...
    'rest_framework',
    'snippets',
]
```
If you didn't put app in `INSTALLED_APPS`, migration and migrate will display nothing change.

### Creating a model to work with
Edit in `model.py`
```python
from django.db import models
from pygments.lexers import get_all_lexers
from pygments.styles import get_all_styles

LEXERS = [item for item in get_all_lexers() if item[1]]
LANGUAGE_CHOICES = sorted([(item[1][0], item[0]) for item in LEXERS])
STYLE_CHOICES = sorted([(item, item) for item in get_all_styles()])


class Snippet(models.Model):
    created = models.DateTimeField(auto_now_add=True)
    title = models.CharField(max_length=100, blank=True, default='')
    code = models.TextField()
    linenos = models.BooleanField(default=False)
    language = models.CharField(choices=LANGUAGE_CHOICES, default='python', max_length=100)
    style = models.CharField(choices=STYLE_CHOICES, default='friendly', max_length=100)

    class Meta:
        ordering = ['created']
```
Then migration and migrate
```shell=
$ python3 manage.py migration
$ python3 manage.py migrate
```
## Creating a Serializer class
Provide a way of serializing and deserializing the snippet instances into representations such as json. 
Edit `serializers.py`
```python
from rest_framework import serializers
from snippets.models import Snippet, LANGUAGE_CHOICES, STYLE_CHOICES


class SnippetSerializer(serializers.Serializer):
    id = serializers.IntegerField(read_only=True)
    title = serializers.CharField(required=False, allow_blank=True, max_length=100)
    code = serializers.CharField(style={'base_template': 'textarea.html'})
    linenos = serializers.BooleanField(required=False)
    language = serializers.ChoiceField(choices=LANGUAGE_CHOICES, default='python')
    style = serializers.ChoiceField(choices=STYLE_CHOICES, default='friendly')

    def create(self, validated_data):
        """
        Create and return a new `Snippet` instance, given the validated data.
        """
        return Snippet.objects.create(**validated_data)

    def update(self, instance, validated_data):
        """
        Update and return an existing `Snippet` instance, given the validated data.
        """
        instance.title = validated_data.get('title', instance.title)
        instance.code = validated_data.get('code', instance.code)
        instance.linenos = validated_data.get('linenos', instance.linenos)
        instance.language = validated_data.get('language', instance.language)
        instance.style = validated_data.get('style', instance.style)
        instance.save()
        return instance
```
A serializer class is very similar to a Django Form class
## Working with Serializers
```shell=
$ python manage.py shell
```
```python
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer
from rest_framework.renderers import JSONRenderer
from rest_framework.parsers import JSONParser

snippet = Snippet(code='foo = "bar"\n')
snippet.save()

snippet = Snippet(code='print("hello, world")\n')
snippet.save()
```
```shell=
$ serializer = SnippetSerializer(snippet)
$ serializer.data
# {'id': 2, 'title': '', 'code': 'print("hello, world")\n', 'linenos': False, 'language': 'python', 'style': 'friendly'}
```