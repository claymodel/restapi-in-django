# How to Create Restful API

## Install python and environment

## Install Django
```
pip install Django
```

## Create the project api_mithu
```
django-admin startproject api_mithu
```

```
cd api_mithu/
```

## django tastypie RestAPI library install
```
pip install django-tastypie
```

```
python manage.py startapp api
```


### Edit the settings and include the api app
```
# api_mithu/settings.py
INSTALLED_APPS = [
'django.contrib.admin',
'django.contrib.auth',
'django.contrib.contenttypes',
'django.contrib.sessions',
'django.contrib.messages',
'django.contrib.staticfiles',
'api'
]
```

### creat the required model
```
Edit
Model Start
# api/models.py
class Note(models.Model):
title = models.CharField(max_length=200)
body = models.TextField()
created_at = models.DateTimeField(auto_now_add=True)
```

#### Update the model
```
Edit
# api/models.py
class Note(models.Model):
title = models.CharField(max_length=200)
body = models.TextField()
created_at = models.DateTimeField(auto_now_add=True)
def __str__(self):
return self.title
```

#### Update the model as required
```
Edit
# api/models.py
class Note(models.Model):
title = models.CharField(max_length=200)
body = models.TextField()
created_at = models.DateTimeField(auto_now_add=True)
def __str__(self):
return '%s %s' % (self.title, self.body)
```

### Create the migration
```
python manage.py makemigrations
```

### Migrate the data
```
python manage.py migrate
```

### Populate data

```
python manage.py shell
>>> from api.models import Note
>>> note = Note(title="First Note", body="This is certainly noteworthy")
>>> note.save()
>>> Note.objects.all()
<QuerySet [<Note: First Note This is certainly noteworthy>]>
>>> exit()
```

### Add the API resource point

```
Edit
# api/resources.py
from tastypie.resources import ModelResource
from api.models import Note
class NoteResource(ModelResource):
class Meta:
queryset = Note.objects.all()
resource_name = 'note'
```

### Define the API end point
```
Edit
#api_mithu//urls.py
from django.conf.urls import url, include
from django.contrib import admin
from api.resources import NoteResource
note_resource = NoteResource()
urlpatterns = [
url(r'^admin/', admin.site.urls),
url(r'^api/', include(note_resource.urls)),
]
```

### Start the RestAPI Server
```
python manage.py runserver
```

### Check the rest endpoint from other curl client
```
curl -w "\n" -X GET http://localhost:8000/api/note/1
```

#### Response
```
{
  "body": "This is certainly",
  "created_at": "2018-07-05T06:54:30.575964",
  "id": 1,
  "resource_uri": "/api/note/1/",
  "title": "First Note"
}
```

### Add authorization in the resource

```
Edit
# api/resources.py
from tastypie.resources import ModelResource
from api.models import Note
from tastypie.authorization import Authorization
class NoteResource(ModelResource):
class Meta:
queryset = Note.objects.all()
resource_name = 'note'
authorization = Authorization()
```

Try to create Delete, Put APIs similarly


### Limiting fields

```
# api/resources.py
from tastypie.resources import ModelResource
from api.models import Note
from tastypie.authorization import Authorization
class NoteResource(ModelResource):
class Meta:
queryset = Note.objects.all()
resource_name = 'note'
authorization = Authorization()
fields = ['title', 'body']
```

## Thank you
