Prashnat
1. Enable Two Factor Authentication and setup an App Password in Gmail.
2. set code editor(pycharm,Vs code,etc). and  startproject
```bash
python -m django startproject mysite
```
Open startapp in project
```bash
cd ./mysite
```
```bash
python -m django startapp subscribe
```
Set Email Configurations
```bash
#### settings.py
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_USE_TLS = True
EMAIL_PORT = 587
EMAIL_HOST_USER = 'your_account@gmail.com'
EMAIL_HOST_PASSWORD = 'your app password'
```
Make simple forms.py in app
```bash
####forms.py
from django import forms


class Subscribe(forms.Form):
    Email = forms.EmailField()

    def __str__(self):
        return self.Email
```
Make a simple View.py to handle sending Emails
```bash
####views.py
from django.conf.global_settings import EMAIL_HOST_USER
from django.shortcuts import render
from . import forms
from django.core.mail import send_mail


def subscribe(request):
    sub = forms.Subscribe()
    if request.method == 'POST':
        sub = forms.Subscribe(request.POST)
        subject = 'Welcome to DataFlair'
        message = 'Hope you are enjoying your Django Tutorials'
        recepient = str(sub['Email'].value())
        send_mail(subject,
                  message, EMAIL_HOST_USER, [recepient], fail_silently=False)
        return render(request, 'subscribe/success.html', {'recepient': recepient})
    return render(request, 'subscribe/index.html', {'form': sub})
```
The template(subscribe/templates/subscribe/index.html)
```bash
####index.html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/html" xmlns="http://www.w3.org/1999/html">
<head>
         <title>DataFlair send email</title>
         <link rel="stylesheet"                 href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
</head>
<body>
    <center>
        <h1 class = 'alert alert-success'>Subscribe Form</h1>
        <h3 class="p-3 mb-2 bg-primary text-white" style = 'font-size: 50px;'>DataFlair Django Tutorials
    </h3>
    <form method="POST">
        <!-- Very Important csrf Token -->
        {% csrf_token %}
        <div class = "form-group">
            <p><h3>{{ form.as_p }}</h3>
            <br>
            <input type="submit" name="Subscribe" class = 'btn btn-primary btn-lg'>
                </div>
            </form>
        </center>
</body>
</html>
```
The template(subscribe/templates/subscribe/success.html)
```bash
####success.html
<!DOCTYPE html>
<html>
<head>
    <title>Success</title>
</head>
<body>
    <h1>email sent successfully to: {{ recepient
    }}</h1>
</body>
</html>
```
Mysite/urls.py
```bash
####mysite/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('subscribe/', include('subscribe.urls')),
    path('admin/', admin.site.urls),
]
```
Subscribe/urls.py
```bash
####subscribe/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.subscribe, name='subscribe')
]
```
Some changes in settings.py
```bash
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'subscribe',
]
```
```bash
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': ['templates'],
        'APP_DIRS': True,
```
