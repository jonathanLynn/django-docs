# Django Authentication

This Django application uses Django's built-in authentication system to authenticate users. The authentication process is handled by three main views: `LoginView`, `HomePageView`, and `LogoutView`.

### /yourapp/views.py

```
from django.contrib.auth import authenticate, login, logout
from django.views.generic import TemplateView
from django.shortcuts import redirect

class LoginView(TemplateView):
    template_name = 'login.html'

    def post(self, request, *args, **kwargs):
        username = request.POST.get('username')
        password = request.POST.get('password')
        user = authenticate(request, username=username, password=password)
        if user is not None:
            login(request, user)
            return redirect('home')
        else:
            return redirect('login')
    
class HomePageView(TemplateView):
    template_name = 'home.html'

    def dispatch(self, request, *args, **kwargs):
        if not request.user.is_authenticated:
            return redirect('login')
        return super().dispatch(request, *args, **kwargs)

class LogoutView(TemplateView):
    def dispatch(self, request, *args, **kwargs):
        logout(request)
        return redirect('login')
```

## LoginView

The `LoginView` is responsible for rendering the login page and handling the login process. When a `POST` request is made to the login page, the `LoginView` retrieves the username and password from the request, authenticates the user, and logs the user in if the authentication was successful. If the authentication fails, the user is redirected back to the login page.

```
class LoginView(TemplateView):
    template_name = 'login.html'

    def post(self, request, *args, **kwargs):
        username = request.POST.get('username')
        password = request.POST.get('password')
        user = authenticate(request, username=username, password=password)
        if user is not None:
            login(request, user)
            return redirect('home')
        else:
            return redirect('login')
```

The login page is defined in the `login.html` file. This file includes a form with fields for the username and password, and a submit button. The form's `action` attribute is set to the URL of the login view, and the `method` attribute is set to "post".

## HomePageView

The `HomePageView` is responsible for rendering the home page. Before the home page is rendered, the `HomePageView` checks if the user is authenticated. If the user is not authenticated, they are redirected to the login page.

```
class HomePageView(TemplateView):
    template_name = 'home.html'

    def dispatch(self, request, *args, **kwargs):
        if not request.user.is_authenticated:
            return redirect('login')
        return super().dispatch(request, *args, **kwargs)
```

The home page is defined in the `home.html` file. This file includes a welcome message and a sign out button. The sign out button is a form that makes a `POST` request to the logout view.

## LogoutView

The `LogoutView` is responsible for logging out the user. When a request is made to the logout view, the `LogoutView` logs out the user and redirects them to the login page.

```
class LogoutView(TemplateView):
    def dispatch(self, request, *args, **kwargs):
        logout(request)
        return redirect('login')
```

## URLs

The URLs for the login, home, and logout views are defined in the `urls.py` file. The `path` function is used to map a URL pattern to a view. The `name` argument is used to name the URL pattern, which allows it to be referred to in other parts of the Django application.

Here are the URL patterns for the authentication views:

- `login/` maps to `LoginView`
- `home/` maps to `HomePageView`
- `logout/` maps to `LogoutView`

### /yourapp/urls.py

```
from .views import LoginView, HomePageView, LogoutView

urlpatterns = [
    path('login/', LoginView.as_view(), name='login'),
    path('home/', HomePageView.as_view(), name='home'),
    path('logout/', LogoutView.as_view(), name='logout'),
]
```

## Maintaining User Authentication throughout your app

If you wish to have other pages require an authenticated user than using the `@login_required` decorator is recommended.

### /yourapp/views.py
```
from django.contrib.auth.decorators import login_required
from django.shortcuts import render

@login_required
def profile(request):
    return render(request, 'profile.html')
```