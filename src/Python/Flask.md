# Flask Handbook

## Lesson 1: Introduction to Flask

- **Definition:** Flask is a *micro* web framework for Python, based on **Werkzeug** (WSGI toolkit) and **Jinja2** (templating engine).
- **Philosophy:** Keep the core simple, let you choose what you need.
- **Why “micro” doesn’t mean “small project only”:**
  - “Micro” means no built-in ORM, authentication, admin panel, etc.
  - You add features via *extensions* or custom code.
- **When to choose Flask:**
  - Small to medium web apps.
  - REST API backends.
  - Prototyping and rapid development.

## Lesson 2: How the Web Works (Flask’s Playground)

Before understanding Flask, understand **HTTP**:

- **Client** (browser, app) sends a *request* → **Server** responds.
- Request parts:
  - **URL** (path + query parameters).
  - **Method** (GET, POST, PUT, DELETE…).
  - **Headers** (metadata).
  - **Body** (form data, JSON, etc.).
- Response parts:
  - **Status Code** (200 OK, 404 Not Found, etc.).
  - **Headers** (content-type, cache, etc.).
  - **Body** (HTML, JSON, etc.).
- **Flask’s job:** Receive request → Run your function → Return response.

## Lesson 3: The Flask Application

- **App Instance:**
  - Created with `Flask(__name__)`.
  - `__name__` tells Flask where your app lives (helps find templates/static).
- **Single File App:** `app.py` is enough for small apps.
- **Multi-File Structure** for larger apps (later in Blueprints lesson).

### Sample Code

```python
#!/usr/bin/env python3
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello, Flask!"

if __name__ == "__main__":
    app.run(debug=True)
```

## Lesson 4: Routing

- **Definition:** Mapping a URL to a Python function.
- **Syntax:**
  `@app.route("/path", methods=["GET", "POST"])`
- **Dynamic Routing:**
  `/user/<username>` → captures `username` as a function parameter.
- **Why routing matters:** It’s the entry point for all requests.

### Sample Code

```python
#!/usr/bin/env python3
from flask import Flask

app = Flask(__name__)

@app.route("/user/<username>")
def show_user(username):
    return f"Hello, {username}!"

if __name__ == "__main__":
    app.run(debug=True)
```

## Lesson 5: Request and Response Objects

- **Request Object:** Access form, query, and JSON data.
- `flask.request`:**
  - `request.args` → query parameters.
  - `request.form` → form data.
  - `request.json` → JSON body.
  - `request.files` → uploaded files.
- **Response Object:** Return HTML, JSON, or redirects.
  - Can return:
    - Plain string → HTML response.
    - `jsonify(data)` → JSON response.
    - `redirect(url)` → HTTP redirect.
- **Headers & Status Codes:** Set custom headers or status code in the response.

### Sample Code

```python
#!/usr/bin/env python3
from flask import Flask, request, jsonify, redirect, url_for

app = Flask(__name__)

@app.route("/greet", methods=["GET"])
def greet():
    name = request.args.get("name", "Guest")
    return f"Hello, {name}!"

@app.route("/json", methods=["POST"])
def json_example():
    data = request.json
    return jsonify({"you_sent": data})

@app.route("/go-home")
def go_home():
    return redirect(url_for("greet"))

if __name__ == "__main__":
    app.run(debug=True)
```

## Lesson 6: Jinja2 Templates

- **Purpose:** Generate dynamic HTML using Python variables.
- **Syntax:**
  - `{{ variable }}` → output a variable.
  - `{% for item in list %} ... {% endfor %}` → loops.
  - `{% if condition %} ... {% endif %}` → conditions.
- **Template Inheritance:** Use `base.html` and extend it for other pages.

### Sample Code

**templates/hello.html**

```html
<!DOCTYPE html>
<html>
<body>
    <h1>Hello, {{ name }}!</h1>
</body>
</html>
```

**app.py**

```python
#!/usr/bin/env python3
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/hello/<name>")
def hello(name):
    return render_template("hello.html", name=name)

if __name__ == "__main__":
    app.run(debug=True)
```

## Lesson 7: Static Files

- **Static Directory:** `static/` folder holds CSS, JS, images.
- **URL to static:** `url_for('static', filename='style.css')`.
- **Best Practice:** Keep all non-changing assets in `/static`.

### Sample Code

Directory structure:

```
/static/style.css
/templates/index.html
app.py
```

**static/style.css**

```css
h1 { color: blue; }
```

**templates/index.html**

```html
<link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
<h1>Styled Page</h1>
```

**app.py**

```python
#!/usr/bin/env python3
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def index():
    return render_template("index.html")

if __name__ == "__main__":
    app.run(debug=True)
```

## Lesson 8: Contexts in Flask

- **Application Context:**
  - Holds `current_app` (reference to the app object).
  - Exists outside of an HTTP request (CLI commands, background jobs).
- **Request Context:**
  - Holds `request`, `session`, `g` (global per request).
  - Exists only during an HTTP request.
- **Why important:** Avoids passing variables everywhere manually.

### Sample Code

```python
#!/usr/bin/env python3
from flask import Flask, request, g

app = Flask(__name__)

@app.before_request
def before():
    g.user = request.args.get("user", "Guest")

@app.route("/")
def index():
    return f"Hello, {g.user}!"

if __name__ == "__main__":
    app.run(debug=True)
```

## Lesson 9: Blueprints

- **Problem:** Big apps have too many routes in one file.
- **Solution:** Blueprints group related routes/templates/static files.
- **Example:**
  - `auth` blueprint for login/logout routes.
  - `api` blueprint for REST endpoints.
- **Benefits:** Cleaner structure, reusable modules.

### Sample Code

**auth.py**

```python
#!/usr/bin/env python3
from flask import Blueprint

auth_bp = Blueprint("auth", __name__)

@auth_bp.route("/login")
def login():
    return "Login Page"
```

**app.py**

```python
#!/usr/bin/env python3
from flask import Flask
from auth import auth_bp

app = Flask(__name__)
app.register_blueprint(auth_bp, url_prefix="/auth")

if __name__ == "__main__":
    app.run(debug=True)
```

## Lesson 10: Middleware & Hooks

- **Before Request Hooks:** `@app.before_request` → runs before each request.
- **After Request Hooks:** `@app.after_request` → modify response before sending.
- **Teardown Functions:** Cleanup after request is processed.
- **Uses:** Logging, authentication checks, caching, request timing.

### Sample Code

```python
#!/usr/bin/env python3
from flask import Flask, request

app = Flask(__name__)

@app.before_request
def log_request():
    print(f"Request: {request.method} {request.path}")

@app.after_request
def add_header(response):
    response.headers["X-App-Name"] = "FlaskDemo"
    return response

@app.route("/")
def home():
    return "Check console log."

if __name__ == "__main__":
    app.run(debug=True)
```

## Lesson 11: Error Handling

- **Custom Error Pages:**
  `@app.errorhandler(404)` → return a custom 404 page.
- **Centralized Error Logic:** All exceptions can be caught and handled.
- **Security Consideration:** Avoid leaking internal error details in production.

### Sample Code

```python
#!/usr/bin/env python3
from flask import Flask

app = Flask(__name__)

@app.errorhandler(404)
def page_not_found(e):
    return "Custom 404 Page", 404

@app.route("/")
def home():
    return "Welcome!"

if __name__ == "__main__":
    app.run(debug=True)
```

## Lesson 12: Configuration Management

- **Sources of config:**
  - Python files (`config.py`).
  - Environment variables.
  - Dicts or objects.
- **Access:** `app.config['KEY']`.
- **Examples:** Database URLs, debug mode, secret keys.
- **Best Practice:** Keep sensitive data outside version control.

### Sample Code

**config.py**

```python
DEBUG = True
SECRET_KEY = "mysecret"
```

**app.py**

```python
#!/usr/bin/env python3
from flask import Flask

app = Flask(__name__)
app.config.from_pyfile("config.py")

@app.route("/")
def home():
    return f"Debug Mode: {app.config['DEBUG']}"

if __name__ == "__main__":
    app.run()
```

## Lesson 13: Flask Extensions

- **Popular Extensions:**
  - **Flask-SQLAlchemy** → ORM for databases.
  - **Flask-Login** → User sessions/authentication.
  - **Flask-Migrate** → DB migrations.
  - **Flask-WTF** → Form handling.
  - **Flask-RESTful** → REST APIs.
- **Why extensions matter:** Reuse instead of reinventing.

### Sample Code

```python
#!/usr/bin/env python3
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///test.db"
db = SQLAlchemy(app)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50))

@app.route("/add/<name>")
def add_user(name):
    db.session.add(User(name=name))
    db.session.commit()
    return "User added!"

if __name__ == "__main__":
    db.create_all()
    app.run(debug=True)
```

## Lesson 14: REST API with Flask

- **REST Basics:** Stateless, resource-oriented, JSON responses.
- **Flask Tools:**
  - `jsonify()` for responses.
  - `request.json` for input.
- **CORS:** Allowing cross-domain requests (`flask-cors`).
- **Versioning:** `/api/v1/...`.

### Sample Code

```python
#!/usr/bin/env python3
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route("/api/data", methods=["POST"])
def api_data():
    data = request.json
    return jsonify({"received": data})

if __name__ == "__main__":
    app.run(debug=True)
```

## Lesson 15: Testing Flask Apps

- **Test Client:** `app.test_client()` for simulating requests.
- **Unit vs Integration Tests:** Test functions individually or as a whole.
- **Best Practice:** Test before deployment.

### Sample Code

```python
#!/usr/bin/env python3
import unittest
from app import app

class FlaskTest(unittest.TestCase):
    def setUp(self):
        self.app = app.test_client()

    def test_home(self):
        response = self.app.get("/")
        self.assertEqual(response.status_code, 200)

if __name__ == "__main__":
    unittest.main()
```

## Lesson 16: Deployment Concepts

- **Why Not Flask’s Dev Server in Production:** It’s single-threaded & insecure.
- **Production Setup:**
  - WSGI servers: Gunicorn, uWSGI.
  - Reverse proxy: Nginx, Apache.
- **Scaling:** Multiple worker processes or containers.

Example Gunicorn command:

```bash
gunicorn -w 4 app:app
```

## Lesson 17: Security in Flask

- **Common Risks:**
  - CSRF → Prevent with Flask-WTF or tokens.
  - XSS → Avoid unsafe HTML, auto-escape templates.
  - SQL Injection → Always use parameterized queries or ORM.
- **Session Security:** Use `SECRET_KEY`.

Enable CSRF protection with Flask-WTF:

```python
from flask_wtf import FlaskForm, CSRFProtect

csrf = CSRFProtect(app)
```

## Lesson 18: Performance Optimization

- **Techniques:**
  - Caching (`flask-caching`).
  - Minifying static files.
  - Avoid blocking calls in request cycle.
- **Monitoring:** Use middleware or APM tools.

Example: Simple caching with flask-caching

```python
from flask_caching import Cache

cache = Cache(app, config={"CACHE_TYPE": "simple"})

@app.route("/")
@cache.cached(timeout=60)
def index():
    return "Cached for 60 seconds"
```

## Lesson 19: Flask in a Larger Architecture

- **Microservices:** Flask as an API service.
- **With Message Queues:** Celery for background jobs.
- **With Databases:** SQLAlchemy or external DB clients.
- **With Frontend:** Works with React, Vue, Angular, or plain HTML.

Example Celery task integration:

```python
from celery import Celery

celery = Celery(broker="redis://localhost:6379/0")
```

## Lesson 20: Summary & Mental Map

By now you should mentally see Flask as:

```
WSGI Server (Werkzeug)
   ↓
Routing System
   ↓
Request Context
   ↓
View Function
   ↓
Response Object
   ↓
Client
```
---

