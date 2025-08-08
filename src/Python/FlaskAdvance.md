# Advanced Flask Topics

## 1. Using jsonify for API Responses

`jsonify` is a Flask helper function that converts Python dictionaries (or lists) to JSON responses with the correct `Content-Type` header.

### Sample Code

```python
#!/usr/bin/env python3
from flask import Flask, jsonify

app = Flask(__name__)

@app.route("/api/user")
def get_user():
    user_data = {"id": 1, "name": "Alice", "role": "admin"}
    return jsonify(user_data)

if __name__ == "__main__":
    app.run(debug=True)
```

## 2. File Upload Handling

Flask can handle file uploads using the `request.files` object.

### Sample Code

```python
#!/usr/bin/env python3
import os
from flask import Flask, request, redirect, url_for

UPLOAD_FOLDER = "./uploads"
ALLOWED_EXTENSIONS = {"txt", "pdf", "png", "jpg", "jpeg", "gif"}

app = Flask(__name__)
app.config["UPLOAD_FOLDER"] = UPLOAD_FOLDER

def allowed_file(filename):
    return "." in filename and filename.rsplit(".", 1)[1].lower() in ALLOWED_EXTENSIONS

@app.route("/upload", methods=["GET", "POST"])
def upload_file():
    if request.method == "POST":
        if "file" not in request.files:
            return "No file part"
        file = request.files["file"]
        if file.filename == "":
            return "No selected file"
        if file and allowed_file(file.filename):
            filepath = os.path.join(app.config["UPLOAD_FOLDER"], file.filename)
            file.save(filepath)
            return f"File uploaded to {filepath}"
    return '''
    <!doctype html>
    <title>Upload new File</title>
    <h1>Upload new File</h1>
    <form method=post enctype=multipart/form-data>
      <input type=file name=file>
      <input type=submit value=Upload>
    </form>
    '''

if __name__ == "__main__":
    os.makedirs(UPLOAD_FOLDER, exist_ok=True)
    app.run(debug=True)
```

## 3. File Download Handling

You can serve files for download using `send_from_directory` or `send_file`.

### Sample Code

```python
#!/usr/bin/env python3
import os
from flask import Flask, send_from_directory

DOWNLOAD_FOLDER = "./uploads"
app = Flask(__name__)

@app.route("/download/<filename>")
def download_file(filename):
    return send_from_directory(DOWNLOAD_FOLDER, filename, as_attachment=True)

if __name__ == "__main__":
    app.run(debug=True)
```

## 4. HTTP Request Handling

Flask’s `request` object gives full access to incoming HTTP request data.

### Sample Code

```python
#!/usr/bin/env python3
from flask import Flask, request

app = Flask(__name__)

@app.route("/inspect", methods=["GET", "POST"])
def inspect_request():
    return {
        "method": request.method,
        "headers": dict(request.headers),
        "args": request.args.to_dict(),
        "form": request.form.to_dict(),
        "json": request.get_json(silent=True),
        "remote_addr": request.remote_addr
    }

if __name__ == "__main__":
    app.run(debug=True)
```

## 5. Serving Files from a ./www Directory

For serving static HTML/CSS/JS files from a `./www` directory, you can use `send_from_directory`.

### Sample Code

```python
#!/usr/bin/env python3
import os
from flask import Flask, send_from_directory

WWW_FOLDER = "./www"
app = Flask(__name__, static_folder=None)  # disable default static

@app.route("/<path:filename>")
def serve_www(filename):
    return send_from_directory(WWW_FOLDER, filename)

@app.route("/")
def serve_index():
    return send_from_directory(WWW_FOLDER, "index.html")

if __name__ == "__main__":
    os.makedirs(WWW_FOLDER, exist_ok=True)
    app.run(debug=True)
```

**Note:** This setup lets you drop HTML, CSS, JS files into `./www` and serve them directly.

---

