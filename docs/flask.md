# Flask

```py
from flask import Flask, request, jsonify
from flask_restful import Resource, Api, reqparse
from flask_jwt import JWT, jwt_required
from flask_sqlalchemy import SQLAlchemy


app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///data.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
app.secret_key = 'secret' # this must be secured
api = Api(app)

db = SQLAlchemy()


@app.before_first_request
def create_tables():
    db.create_all()

# MODELS

class UserModel(db.Model):
    __tablename__ = 'users'

    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80))
    password = db.Column(db.String(80))

    # store_id = db.Column(db.Integer, db.ForeignKey('stores.id'))
    # store = db.relationship('StoreModel')

    # items = db.relationship('ItemModel', lazy='dynamic')
    # self.items.all()

    def __init__(self, username, password):
        self.username = username
        self.password = password

    @classmethod
    def find_by_username(cls, username, password):
        # return User object if username and password are found in db
        return cls.query.filter_by(username=username).first()

    @classmethod
    def find_by_id(cls, _id):
        # return User object if id is found in db
        ...

    def upsert(self):
        db.session.add(self)
        db.session.commit()

    def delete(self):
        db.session.delete(self)
        db.session.commit()

# JWT
# request access token from /auth with username and password payload
# use access token for endpoints requiring JWT
# add Authentication header with value "JWT <token>"

def authenticate(username, password):
    user = User.find_by_username(username)
    if user and password == user.password:
        return user


def identity(payload):
    return User.find_by_id(payload['identity'])


jwt = JWT(app, authenticate, identity)


@app.route("/protected", methods=["GET"])
@jwt_required()
def protected():
    ...

# RESOURCE
# Resource automatically jsonify responses
# http methods are implemented as methods
# method parameters must be consistent

class Student(Resource):
    # reqparse enforces payload keys and values
    # it ignores keys not registered as argument
    parser = reqparse.RequestParser()
    parser.add_argument(
        'username',
        type=str,
        required=True,
        help='Username'
    )

    def get(self, name):
        return {'name': name}

    def post(self, name):
        data = Student.parser.parse_args()
        # create resource here
        return {'status': 'Success'}, 201


api.add_resource(Student, '/student/<string:name>')

if __name__ == '__main__':
    db.init_app(app)
    app.run(debug=True, port=5000)
```

## Setup
Install
```sh
poetry add flask
```

Basic Route
```py
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```

Run
```sh
export FLASK_APP=hello # hello.py
export FLASK_ENV=development # full dev features
flask run
```

As a shortcut, if the file is named `app.py` or `wsgi.py`, you don’t have to set the `FLASK_APP` environment variable.  
The default return content type is HTML.

## HTML Escaping

```py
from markupsafe import escape

@app.route("/<name>")
def hello(name):
    return f"Hello, {escape(name)}!"
```

## Variable Rules

```py
from markupsafe import escape

@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    return f'User {escape(username)}'

@app.route('/post/<int:post_id>')
def show_post(post_id):
    # show the post with the given id, the id is an integer
    return f'Post {post_id}'

@app.route('/path/<path:subpath>')
def show_subpath(subpath):
    # show the subpath after /path/
    return f'Subpath {escape(subpath)}'
```

**Converter Types**  
`string` -- (default) accepts any text without a slash  
`int` -- accepts positive integers  
`float` -- accepts positive floating point values  
`path` -- like string but also accepts slashes  
`uuid` -- accepts UUID strings  

## URL Building

```py
from flask import url_for

@app.route('/')
def index():
    return 'index'

@app.route('/login')
def login():
    return 'login'

@app.route('/user/<username>')
def profile(username):
    return f'{username}\'s profile'

with app.test_request_context():
    print(url_for('index'))                             # /
    print(url_for('login'))                             # /login
    print(url_for('login', next='/'))                   # /login?next=/
    print(url_for('profile', username='John Doe'))      # /user/John%20Doe
```

## HTTP Methods

```py
from flask import request

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        return do_the_login()
    else:
        return show_the_login_form()
```

## Static Files

```py
url_for('static', filename='style.css')
```

The file has to be stored on the filesystem as `static/style.css`.

## Rendering Templates

```py
from flask import render_template

@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    return render_template('hello.html', name=name)

# /app.py
# /templates
#     /hello.html
```

Inside templates you also have access to the `config`, `request`, `session` and `g` objects as well as the `url_for()` and `get_flashed_messages()` functions.

## Template Inheritance

Base
```html
{% raw %}
<!doctype html>
<html>
  <head>
    {% block head %}
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
    <title>{% block title %}{% endblock %} - My Webpage</title>
    {% endblock %}
  </head>
  <body>
    <div id="content">{% block content %}{% endblock %}</div>
    <div id="footer">
      {% block footer %}
      &copy; Copyright 2010 by <a href="http://domain.invalid/">you</a>.
      {% endblock %}
    </div>
  </body>
</html>
{% endraw %}
```

Child
```html
{% raw %}
{% extends "layout.html" %}
{% block title %}Index{% endblock %}
{% block head %}
  {{ super() }}
  <style type="text/css">
    .important { color: #336699; }
  </style>
{% endblock %}
{% block content %}
  <h1>Index</h1>
  <p class="important">
    Welcome on my awesome homepage.
{% endblock %}
{% endraw %}
```

## Test

```py
from flask import request

with app.test_request_context('/hello', method='POST'):
    # now you can do something with the request until the
    # end of the with block, such as basic assertions:
    assert request.path == '/hello'
    assert request.method == 'POST'
```

## Request

Form data and query string
```py
from flask import request

@app.route('/login', methods=['POST', 'GET'])
def login():
    error = None
    if request.method == 'POST':
        if valid_login(request.form['username'],
                       request.form['password']):
            return log_the_user_in(request.form['username'])
        else:
            error = 'Invalid username/password'
    # the code below is executed if the request method
    # was GET or the credentials were invalid
    return render_template('login.html', error=error)

# query string
# searchword = request.args.get('key', '')

# json payload
# payload = request.get_json()
```

## File upload
```py
from werkzeug.utils import secure_filename

@app.route('/upload', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        file = request.files['the_file']
        file.save(f"/var/www/uploads/{secure_filename(file.filename)}")
    ...
```

## Cookies

Read
```py
from flask import request

@app.route('/')
def index():
    username = request.cookies.get('username')
    # use cookies.get(key) instead of cookies[key] to not get a
    # KeyError if the cookie is missing.
```

Set
```py
from flask import make_response

@app.route('/')
def index():
    resp = make_response(render_template(...))
    resp.set_cookie('username', 'the username')
    return resp
```

Use sessions instead of cookies for better security.

## Redirects and Errors

```py
from flask import abort, redirect, url_for

@app.route('/')
def index():
    return redirect(url_for('login'))

@app.route('/login')
def login():
    abort(401)
    this_is_never_executed()
```

## Error handler
```py
from flask import render_template

@app.errorhandler(404)
def page_not_found(error):
    return render_template('page_not_found.html'), 404
```

## APIs with JSON

Dictionary as JSON
```py
@app.route("/me")
def me_api():
    user = get_current_user()
    return {
        "username": user.username,
        "theme": user.theme,
        "image": url_for("user_image", filename=user.image),
    }
```

JSONify other data types
```py
from flask import jsonify

@app.route("/users")
def users_api():
    users = get_all_users()
    return jsonify([user.to_json() for user in users])
```

## Sessions
```py
from flask import session

# Set the secret key to some random bytes. Keep this really secret!
app.secret_key = b'_5#y2L"F4Q8z\n\xec]/'

@app.route('/')
def index():
    if 'username' in session:
        return f'Logged in as {session["username"]}'
    return 'You are not logged in'

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        session['username'] = request.form['username']
        return redirect(url_for('index'))
    return '''
        <form method="post">
            <p><input type=text name=username>
            <p><input type=submit value=Login>
        </form>
    '''

@app.route('/logout')
def logout():
    # remove the username from the session if it's there
    session.pop('username', None)
    return redirect(url_for('index'))
```

Flask will serialize the values in the session object into a cookie. Take caution on cookie size.

For server-side sessions, use Flask extensions.

## Message Flashing

```py
flash('Invalid password provided', 'error')

messages = get_flashed_messages()
```

[Example](https://flask.palletsprojects.com/en/2.0.x/patterns/flashing/)

## Logging
```py
app.logger.debug('A value for debugging')
app.logger.warning('A warning occurred (%d apples)', 42)
app.logger.error('An error occurred')
```

## Deployment
[Reference](https://flask.palletsprojects.com/en/2.0.x/deploying/)
