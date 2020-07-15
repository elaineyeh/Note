# Requests and Responses
## Request objects
Similar to request.POST, but more useful for working with Web APIs.
```python
request.POST  # Only handles form data.  Only works for 'POST' method.
request.data  # Handles arbitrary data.  Works for 'POST', 'PUT' and 'PATCH' methods.
```
## Response objects
A type of TemplateResponse that takes unrendered content and uses content negotiation to determine the correct content type to return to the client.
```python
return Response(data)  # Renders to content type as requested by the client.
```
## Status codes
REST framework provides more explicit identifiers for each status code, such as HTTP_400_BAD_REQUEST in the status module.
## Wrapping API views
REST framework provides two wrappers you can use to write API views.  

1.The `@api_view` decorator for working with function based views.  
2.The `APIView` class for working with class-based views.
## Pulling it all together
```python
from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer


@api_view(['GET', 'POST'])
def snippet_list(request):
    """
    List all code snippets, or create a new snippet.
    """
    if request.method == 'GET':
        snippets = Snippet.objects.all()
        serializer = SnippetSerializer(snippets, many=True)
        return Response(serializer.data)

    elif request.method == 'POST':
        serializer = SnippetSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```
Use pk
```python
@api_view(['GET', 'PUT', 'DELETE'])
def snippet_detail(request, pk):
    """
    Retrieve, update or delete a code snippet.
    """
    try:
        snippet = Snippet.objects.get(pk=pk)
    except Snippet.DoesNotExist:
        return Response(status=status.HTTP_404_NOT_FOUND)

    if request.method == 'GET':
        serializer = SnippetSerializer(snippet)
        return Response(serializer.data)

    elif request.method == 'PUT':
        serializer = SnippetSerializer(snippet, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    elif request.method == 'DELETE':
        snippet.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```
| Action | HTTP method=APIView | ViewSet method |
|:------:|:-------------------:|:--------------:|
|新增一個 |      POST           |     Create     |
|刪除一個 |      DELETE         |     Destroy    |
|修改一個 |      PUT/PATCH      |     Update     |
|查詢一個 |      GET            |    Retrieve    |
|查詢一堆 |      GET            |    List        |
## Adding optional format suffixes to our URLs
Start by adding a format keyword argument to both of the views.
```python
def snippet_list(request, format=None):
```
```python
def snippet_detail(request, pk, format=None):
```
snippets/urls.py
```python
from django.urls import path
from rest_framework.urlpatterns import format_suffix_patterns
from snippets import views

urlpatterns = [
    path('snippets/', views.snippet_list),
    path('snippets/<int:pk>', views.snippet_detail),
]

urlpatterns = format_suffix_patterns(urlpatterns)
```