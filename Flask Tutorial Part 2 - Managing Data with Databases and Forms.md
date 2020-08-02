#### SQLAlchemy - Adding a Database

Even though, we can pass down data to our render_template(). We don't have a way to manage and store our data. 

How would we handle and persist the data? 

Generally websites have some form of database running, this allows them to store and manage all types of data. 

To make our lives a bit easier, we are going to use a module for flask called `flask-sqlalchemy`.

Update the `__init__.py` in the `app/` folder:

`__init__.py`:

```python
import os
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)

basedir = os.path.abspath(os.path.dirname(__file__))

app.config['SQLALCHEMY_DATABASE_URI'] =\
    'sqlite:///' + os.path.join(basedir, 'data.sqlite')
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)

from app import routes
```

This configures SQLAlchemy. (Some information about SQL Alchemy and ORMs here)

#### Install Flask-SQLAlchemy

```
(venv) PS ...\todo-flask> pip install flask-sqlalchemy
```

After typing this command in the terminal, you will see:

```
< Some "Collecting" and "Requirements already satisfied" lines>
Installing collected packages: SQLAlchemy, flask-sqlalchemy
Successfully installed SQLAlchemy-1.3.18 flask-sqlalchemy-2.4.4
```

#### Defining Models

Let's create a new folder inside `app/` folder, and call it Models. Inside this models we will create a `models.py` file

```
todo-flask/
  venv/
  app/
    models/
      models.py
    templates/
      index.html
    __init__.py
    routes.py
  app.py
```

`models.py`

```python
from app import db

# Models
class Task(db.Model):
    __tablename__ = 'tasks'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64))
    description = db.Column(db.String(512))

    def __repr__(self):
        return '<Task %r - %r>' % self.name, self.description
```

What we've done here is defined the model. (Explanation of what are models)

After we've defined the models we are using in our database, its time to build the tables and add some starting data.

Let's create a `setup_db.py` python file, to help use create some new tasks and initialize the database.

```python
from app import app, db
from app.models import Task

db.drop_all()
db.create_all()

tasks = [
    'Eat some food',
    'Listen to some music',
    'Read a book',
]

for task in tasks:
    new_task = Task(name=task, description='')
    db.session.add(new_task)
db.session.commit()
```

Now all we have to do is run this file to create these three tasks. 

We also need to navigate to our `routes.py` and make some changes so it knows about the database model we've created.

```python
from flask import render_template
from app import app, models

Task = models.Task

@app.route('/')
def index():
    title = 'My Personal Todo Application!'
    tasks = Task.query.all()
    return render_template("index.html", title=title, tasks=tasks)
```

#### Adding Tasks

The next step is to give ourselves the ability to add tasks. We can do this on the frontend side with HTML and Bootstrap. Usually when users send data or information, a common way that is being used in HTML is forms. They encapsulate information in a request in order to deliver it to the backend, which is our Flask server. 

This is what we will end with:

![Add tasks using modals](C:/Users/jerry/Documents/Projects/FlaskExample/Notes/todo_images/modal.png)

For this to work, we need to add some code to both the HTML file, `index.html` and our `routes.py` file

`routes.py`

```python
from flask import render_template, request, redirect, url_for
from app import app, models, db

Task = models.Task

@app.route('/')
def index():
    title = 'My Personal Todo Application!'
    tasks = Task.query.all()
    return render_template("index.html", title=title, tasks=tasks)

# POST (Forms)
@app.route('/item/', methods=['POST'])
def add_item():
    # Get form data from
    taskName = request.form.get('itemName')
    taskDescription = request.form.get('itemDescription')
    new_item = Task(name=taskName, description=taskDescription)
    db.session.add(new_item)
    db.session.commit()
    return redirect(url_for('index'))
```

Notice how we have`@app.route('/item/', methods=['POST'])`. 

This will be a different endpoint to handle receiving data from the user where the user *POSTS* the data back to our server.  This is done through 

For more information:

https://www.w3schools.com/html/html_forms.asp

More links

(Explanation of add_item()  here)

###### Modifying index.html

We will take a two step process for this. 

`index.html`

```python
<nav class="navbar navbar-light bg-light">
    <a class="navbar-brand" href="/">Simple Todo App</a>
    <button
        type="button"
        class="btn btn-primary"
        data-toggle="modal"
        data-target="#newItemModal"
    >
    Add New Item
    </button>
 </nav>
```

Notice that in our navigation bar `<nav>` tag, that we have enclosed a button called `Add New Item`. Inside the tag, there are a few properties such as 

| Property    | Explanation                                                  |
| ----------- | ------------------------------------------------------------ |
| type        | Identifies what kind of element it is                        |
| class       | Names you can give your tags to identify them, they are typically used by CSS later to define the look and feel of your element. In our case, bootstrap is looking at this property. "btn" tells it's a button. "btn-primary", it's a primary button and should be coloured as blue. |
| data-toggle | A data attribute property, that bootstrap uses to identify an element such as a button, that will be used to "control" or display a modal. |
| data-target | A data attribute property, that says, look for this specific element with id `newItemModal`. <br />`data-target*="#newItemModal"`. When the `Add New Item` is pressed, it will make the element with `id  = newItemModal` to be visible or hidden. |

For more information about modals:

https://getbootstrap.com/docs/4.0/components/modal/

##### Adding the Modal - The Popup Form to add new Tasks

We are now going to add the modal itself to index.html. Copy paste this under the last `</div> ` tag or at the top of the first `<script>` tag. This will be roughly line 55 in `index.html`. Please don't get intimidated by the amount of code below, we'll break it down in the next section and at any point you will never be expected to memorise any of this. 

This is Bootstrap's way of representing modals in an accessible way to tools such as screen readers. If you're not confident in pasting this. We have a copy set up for you here: `<Link to commit index.html>`

```html
<div
  class="modal fade"
  id="newItemModal"
  data-backdrop="static"
  tabindex="-1"
  role="dialog"
  aria-labelledby="newItemModalLabel"
  aria-hidden="true"
>
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="newItemModalLabel">Add New Item</h5>
        <button
          type="button"
          class="close"
          data-dismiss="modal"
          aria-label="Close"
        >
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        <form action="/item/" method="post">
          <div class="form-group">
            <label for="itemName">Task Name</label>
            <input
              name="itemName"
              type="text"
              class="form-control"
              id="itemName"
              placeholder="Item Name"
            />
          </div>
          <div class="form-group">
            <label for="itemDescription">Task Description</label>
            <textarea
              name="itemDescription"
              class="form-control"
              id="itemDescription"
              rows="3"
            ></textarea>
          </div>
          <button type="submit" class="btn btn-primary">Submit</button>
        </form>
      </div>
    </div>
  </div>
</div>
```

The part we are interested in is the following:

```html
<form action="/item/" method="post">
  <div class="form-group">
    <label for="itemName">Task Name</label>
    <input
      name="itemName"
      type="text"
      class="form-control"
      id="itemName"
      placeholder="Item Name"
    />
  </div>
  <div class="form-group">
    <label for="itemDescription">Task Description</label>
    <textarea
      name="itemDescription"
      class="form-control"
      id="itemDescription"
      rows="3"
    ></textarea>
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

Here, we are wrapping a `<form>` around three form elements. Specifically, a text input form element, a text area form element for longer paragraphs and a submit button.

```html
<form action="/item/" method="post">
    <input name="itemName" type="text"></input>
	<textarea name="itemDescription" id="itemDescription"></textarea>
	<button type="submit" class="btn btn-primary"></button>
</form>
```

When the button is pressed, the form is submitted and sent to http://www.ourwebsite.com/item. As a POST request, a post request is a special request that allows us to attach data to the server. For more information refer to the REST API section of our MSA workshop notes.

Congratulations! We can now add items.