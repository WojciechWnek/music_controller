# Music controller

## environment

 `pip freeze` - will display requirements

 `pip freeze > requirements.txt` - will save output to file

 `pip install -r requirements.txt` - will read from file and install dependencies
  
## urls

music_controller/urls.py
- include **urls.py** from **api** folder
```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path("", include("api.urls"))
]
```

api/views.py
- will display **Hello**
```
from django.http import HttpResponse

def main(request):
    return HttpResponse("Hello")
```

api/urls.py
- will trigger **main** funcion on path **""**
```
from django.urls import path
from .views import main

urlpatterns = [
    path("", main)
]
```