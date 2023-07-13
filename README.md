# Music controller

## environment

`python -m venv music-env` - will create virtual environment

On Windows, run:\
`music-env\Scripts\activate.bat` - will run virtual environment\
`music-env\Scripts\activate.ps1` - with powershell

On Unix or MacOS, run:\
`source music-env/bin/activate` - will run virtual environment

`deactivate` - will stop virtual environment

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

## rest_framework

create model of a database table
```
class Room(models.Model):
    code = models.CharField(max_length=8, default="", unique=True)
    host = models.CharField(max_length=50, unique=True)
    guest_can_pause = models.BooleanField(null=False, default=False)
    votes_to_skip = models.IntegerField(null=False, default=2)
    created_at = models.DateTimeField(auto_now_add=True)
```

**serialize** model using **rest_framework**
```
from rest_framework import serializers
from .models import Room

class RoomSerializer(serializers.ModelSerializer):
    class Meta:
        model = Room
        fields = ('id', "code", "host", "guest_can_pause",
                  "votes_to_skip", 'created_at')
    
```
where:
- **fields** will be json keys
\
\
create **class view** using rest_framework, 

to do this, utilize **serializer** from above
```
from rest_framework import generics
from .models import Room
from .serializers import RoomSerializer

class RoomView(generics.ListAPIView):
    queryset = Room.objects.all()
    serializer_class = RoomSerializer
```


update urls utlizing **class view**
```
from .views import RoomView

urlpatterns = [
    path("room", RoomView.as_view())
]
```