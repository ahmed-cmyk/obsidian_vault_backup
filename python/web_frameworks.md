# Python Web Frameworks (Basics)

Python is a popular choice for web development, thanks to its readability, extensive libraries, and a rich ecosystem of web frameworks. Web frameworks provide a structured way to build web applications, handling common tasks like routing, request/response handling, database interaction, and templating.

This note will provide a basic introduction to two of the most popular Python web frameworks: Flask and Django.

## 1. Flask (Microframework)

Flask is a lightweight and flexible microframework. It provides the essentials for web development without imposing a rigid structure or requiring specific tools. This makes it ideal for smaller applications, APIs, or when you want more control over the components you use.

### Key Characteristics:
-   **Microframework:** Provides core functionalities, leaving other choices (like database ORM, form validation) to the developer.
-   **Flexible:** Allows developers to choose their own tools and libraries.
-   **Simple and Easy to Get Started:** Low barrier to entry.
-   **Good for APIs and Smaller Apps:** Often used for RESTful APIs, simple websites, or prototypes.

### Installation

```bash
pip install Flask
```

### Basic Flask Application (`app.py`)

```python
from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__) # Create a Flask application instance

# Route for the home page
@app.route('/')
def index():
    return "Hello, Flask! This is the home page."

# Route with a variable part
@app.route('/user/<username>')
def show_user_profile(username):
    return f'User: {username}'

# Route that accepts different HTTP methods
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        if username == 'admin' and password == 'password': # Simple check
            return redirect(url_for('dashboard'))
        else:
            return "Invalid credentials"
    return '''
        <form method="post">
            <p><input type=text name=username></p>
            <p><input type=password name=password></p>
            <p><input type=submit value=Login></p>
        </form>
    '''

@app.route('/dashboard')
def dashboard():
    return "Welcome to the Dashboard!"

# Route that renders a template
@app.route('/hello/<name>')
def hello_template(name):
    # Flask looks for templates in a 'templates' folder by default
    return render_template('hello.html', name=name)

# To run the application
if __name__ == '__main__':
    app.run(debug=True) # debug=True enables debugger and auto-reloader
```

### Basic Template (`templates/hello.html`)

```html
<!DOCTYPE html>
<html>
<head>
    <title>Hello Page</title>
</head>
<body>
    <h1>Hello, {{ name }}!</h1>
    <p>This is a template rendered by Flask.</p>
</body>
</html>
```

### Running Flask

1.  Save the Python code as `app.py`.
2.  Create a `templates` folder in the same directory and save `hello.html` inside it.
3.  Set the `FLASK_APP` environment variable:
    ```bash
    export FLASK_APP=app.py # On Linux/macOS
    set FLASK_APP=app.py   # On Windows CMD
    $env:FLASK_APP="app.py" # On Windows PowerShell
    ```
4.  Run the development server:
    ```bash
    flask run
    ```
    Or, if you have `if __name__ == '__main__': app.run(debug=True)` in your `app.py`:
    ```bash
    python app.py
    ```

## 2. Django (Full-Stack Framework)

Django is a high-level, full-stack web framework that encourages rapid development and clean, pragmatic design. It follows the "batteries-included" philosophy, providing many built-in features and tools for common web development tasks.

### Key Characteristics:
-   **Full-Stack:** Provides almost everything you need out-of-the-box (ORM, admin panel, authentication, templating, etc.).
-   **Opinionated:** Has a strong philosophy on how things should be done.
-   **Scalable:** Used by many large websites.
-   **DRY (Don't Repeat Yourself):** Aims to reduce repetitive code.
-   **MTV (Model-Template-View) Architecture:** Similar to MVC, but with a slightly different naming convention.

### Installation

```bash
pip install Django
```

### Basic Django Project

1.  **Start a project:**
    ```bash
    django-admin startproject myproject
    cd myproject
    ```
    This creates a project structure:
    ```
    myproject/
    ├── manage.py
    └── myproject/
        ├── __init__.py
        ├── asgi.py
        ├── settings.py
        ├── urls.py
        └── wsgi.py
    ```

2.  **Create an app:** Django projects are composed of reusable "apps".
    ```bash
    python manage.py startapp myapp
    ```
    This creates `myapp/` directory:
    ```
    myapp/
    ├── migrations/
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── tests.py
    └── views.py
    ```

3.  **Register the app:** Add `'myapp'` to `INSTALLED_APPS` in `myproject/settings.py`.

4.  **Define a View (`myapp/views.py`):**
    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django! This is my app.")
    ```

5.  **Map URLs (`myapp/urls.py`):**
    ```python
    from django.urls import path
    from . import views

    urlpatterns = [
        path('', views.home, name='home'),
    ]
    ```

6.  **Include app URLs in project URLs (`myproject/urls.py`):**
    ```python
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('myapp/', include('myapp.urls')), # Include your app's URLs
    ]
    ```

7.  **Run migrations (for database setup):**
    ```bash
    python manage.py migrate
    ```

8.  **Run the development server:**
    ```bash
    python manage.py runserver
    ```
    Visit `http://127.0.0.1:8000/myapp/` in your browser.

### Django ORM (Object-Relational Mapper)

Django includes a powerful ORM that allows you to interact with your database using Python objects instead of raw SQL.

```python
# myapp/models.py
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    pub_date = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```

After defining models, run:

```bash
python manage.py makemigrations
python manage.py migrate
```

Then you can interact with it in the Django shell:

```bash
python manage.py shell
>>> from myapp.models import Post
>>> Post.objects.create(title="My First Post", content="Hello world!")
>>> Post.objects.all()
>>> Post.objects.get(title="My First Post")
```

## Choosing a Framework

-   **Flask:** Choose Flask if you need a lightweight solution, prefer to pick your own components, or are building a small API.
-   **Django:** Choose Django if you need a full-featured framework, prefer convention over configuration, or are building a complex, data-driven web application.

Both frameworks are excellent choices, and the best one depends on your project's specific needs and your team's preferences.
