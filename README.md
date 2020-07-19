<h3 align="center">
<img alt="Logo" src="img/in-develop.png" width="75%">
</h3>

<h1 align="center">
:small_red_triangle_down:  Django Basics
</h1>

<p align="center">
  <img src="https://img.shields.io/static/v1?label=PRs&message=welcome&color=7159c1&labelColor=000000" alt="PRs welcome!" />

  <img alt="License" src="https://img.shields.io/static/v1?label=license&message=MIT&color=7159c1&labelColor=000000">
</p>

<p>
This repository was created to show the main commands needed to create an application in Django framework.
</p>

## :computer: Commands

1. Create a directory and a sub-directory to install a Virtual Env:
  
    `$ mkdir my_directory`
    `$ cd my_directory`
    `$ mkdir venv`

2. Create and activate a Virtual Enviroment (Virtual Env):
  
    `$ cd venv`
    `$ python -m venv name_venv`
    `$ cd name_venv/Scripts`

    - On PowerShell or cmd:
        `$ activate`

    -On GitBash:
        `$ source activate`

3. Install Django:

    `$ cd \`  
    `$ cd my_directory`  
    `$ pip install django`

4. Create a project in Django:
  
    `$ django-admin startproject my_project`

5. Create an app in Django:
  
    `$ cd my_project`  
    `$ django-admin startapp my_app`

6. Set Python interpreter to point to 'Python.exe' inside venv created in the folder 'Scripts'.

7. Include de new app in Settings:

```Python  
    INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'my_app',
]
```
8. File 'urls.py:

```Python
        from django.contrib import admin
        from django.urls import path
        from core import views
        from django.views.generic import RedirectView

        urlpatterns = [
            path('admin/', admin.site.urls),
            path('home/', views.home),
            path('eventos/', views.lista_eventos),
            path('', RedirectView.as_view(url='/home/')), # Redirecionando o index diretamente
        ]
```

9. File views.py:

```Python
    from django.contrib.auth.models import User
    from django.shortcuts import render, redirect
    from core.models import Evento
    from django.contrib.auth.decorators import login_required
    from django.contrib.auth import authenticate, login, logout
    from django.contrib import messages
    from datetime import datetime, timedelta
    from django.http.response import Http404, JsonResponse

    def login_user(request):
            return render(request, 'login.html')

    def submit_login(request):
            if request.POST:
                    username = request.POST.get('username')
                    password = request.POST.get('password')
                    usuario = authenticate(username=username, password=password)
                    if usuario is not None:
                            login(request, usuario)
                            return redirect('/')
                    else:
                            messages.error(request, 'Usuário ou Senha inválidos')
            return  redirect('/')

    # Lista os eventos atuais a partir de duas horas atrás
    @login_required(login_url='/login/') # Precisa colocar a barra no início pra não concatenar
    def lista_eventos(request):
            usuario = request.user
            data_atual = datetime.now() - timedelta(days=1) # Pegando não atrasados de de até 1 hora atrás
            evento = Evento.objects.filter(usuario=usuario,
                                        data_evento__gt=data_atual).order_by('data_evento', 'titulo') # '__lt' para datas menores
            dados = {'eventos':evento}
            return render(request, 'home.html', dados)

```

10. Run server:

    `$ python manage.py runserver`

## :small_orange_diamond: Sharing

1. To understand a little more about migration from Sqlite to Postgres check the link bellow:

    [https://www.youtube.com/watch?v=ZgRkGfoy2nE](https://www.youtube.com/watch?v=ZgRkGfoy2nE)

2. To better understando how to deploy an Django Python application in Heroku Cloud check the link bellow:

    [https://www.youtube.com/watch?v=wsi0xpHUM00](https://www.youtube.com/watch?v=wsi0xpHUM00)

## :small_orange_diamond: LIcense

    This project is undet MIT license. Open file [LICENSE](LICENSE.md) to details. The images in this project were maded by repo's owner or taken from another repo in the web with the right authorization of use.
