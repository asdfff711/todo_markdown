# Flask Tutorial

### Learning Outcomes



### Pre-requisites

#### Python 3

https://www.python.org/downloads/

#### Visual Studio Code

https://code.visualstudio.com/download

### Making a Directory

First of all, let's open up visual studio code, our code editor or an editor of your choice.

We will need to make a new folder, in order to manage our new Flask project.

For the purposes of this demo, we will name the folder `todo_flask`, now let's navigate inside it for the next step.

### Setting up a Virtual Environment

What is a virtual environment?

#### Linux / Ubuntu

Sometimes in a Ubuntu Linux system, the standard venv package is not installed by default. To add it to your system, install the python3-venv package as follows: 

`$ sudo apt-get install python3-venv`

The command that creates a virtual environment has the following structure: 

`python3 -m venv venv`

To activate

`source venv/bin/activate`

#### Windows

The command that creates a virtual environment has the following structure: 

`python3 -m venv venv`

To activate a virtual environment, in the current project directory, type in the following:

`venv\Scripts\activate`

If you're no longer using the virtual environmental or want to use another virtual environment for another project, you can type the following in the terminal:

```deactivate```

#### Flask

What is Flask?

##### Installing Flask

```
(env) $ pip install flask
```

```
(venv) pip freeze
click==7.1.2
Flask==1.1.2
itsdangerous==1.1.0
Jinja2==2.11.2
MarkupSafe==1.1.1
Werkzeug==1.0.1
```

#### Project Structure

Inside the `todo-flask` directory, we will create three files:

```
todo-flask/
  venv/
  app/
    __init__.py
    routes.py
  app.py
```

`app.py`

```python
from app import app
```

`todo-flask/app/__init__.py`

```python
from flask import Flask

app = Flask(__name__)

from app import routes
```

``todo-flask/app/routes.py`

```python
from app import app

@app.route('/')
def index():
    return "Hello, World!"
```

Congratulations! You've successfully created your first web application! How easy was that?! 😊

#### Running the Flask Website

Now let's see our website in action.

```
(venv) PS ...\projects\> cd todo-flask
(venv) PS ...\projects\todo-flask> flask run
* Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
127.0.0.1 - - [25/Jul/2020 21:00:00] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [25/Jul/2020 21:00:00] "GET /favicon.ico HTTP/1.1" 404 -
```

Now go to http://127.0.0.1:5000/ with any web browser and it should be up.

#### Routes

Let's talk a bit about routing.

If our website is called http://www.website.com/, this is the root or base URL.

Routes are different URLs that an application supports. This `routes` modules gives the ability to provide content to users who view this website. In Flask, we can handle different web page with functions. These view functions translate the URL's to return different content for the end user.

```python
# http://www.website.com/
@app.route('/')
def index():
    return "I handle the base URL"

# http://www.website.com/route
@app.route('/route')
def route_view():
    return "I handle http://www.website.com/route!"
```

So if your website may have many pages, Flask gives you the functionality to serve different web pages or content.

##### Enable Debug / Development Mode

When we are updating and developing the website, we want any changes we apply to be reflected 

For Linux, Mac, Linux Subsystem for Windows, Git Bash on Windows, etc.:

```bash
export FLASK_APP=myapp
export FLASK_ENV=development
flask run
```

For Windows CMD, use `set` instead of export:

```bash
set FLASK_ENV=development
```

For PowerShell, use `$env`:

```powershell
$env:FLASK_ENV = "development"
```

#### Templating - Serving HTML Web Pages 

Now we're well on our way to designing and developing a fully fledged website. 

Let's introduce the next building blocks: templating! 

Templating allows us to use HTML and CSS on our website, the fundamental building blocks of all modern websites.

Create a new folder in `todo_flask` and call it `templates/`:

```
todo-flask/
  venv/
  app/
    templates/
      static.html
    __init__.py
    routes.py
  app.py
```

Let's add create a new file `static.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- Bootstrap -->
    <link
      rel="stylesheet"
      href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css"
      integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk"
      crossorigin="anonymous"
    />
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Simple Todo App</title>
  </head>
  <body>
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

    <div class="container">
      <table class="table table-striped">
        <thead>
          <tr>
            <th scope="col">#</th>
            <th scope="col">Task</th>
            <th scope="col">Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <th scope="row">1</th>
            <td>Eat</td>
            <td>
              <a href="#"
                ><button type="button" class="btn btn-primary">View</button></a
              >
              <!-- Button trigger modal -->
              <button type="button" class="btn btn-danger">
                Delete
              </button>
            </td>
          </tr>
          <tr>
            <th scope="row">2</th>
            <td>Jog</td>
            <td>
              <a href="#"
                ><button type="button" class="btn btn-primary">View</button></a
              >
              <!-- Button trigger modal -->
              <button type="button" class="btn btn-danger">
                Delete
              </button>
            </td>
          </tr>
          <tr>
            <th scope="row">3</th>
            <td>Sleep</td>
            <td>
              <a href="#"
                ><button type="button" class="btn btn-primary">View</button></a
              >
              <!-- Button trigger modal -->
              <button type="button" class="btn btn-danger">
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Javascript support for bootstrap -->
    <script
      src="https://code.jquery.com/jquery-3.4.1.slim.min.js"
      integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n"
      crossorigin="anonymous"
    ></script>
    <script
      src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js"
      integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo"
      crossorigin="anonymous"
    ></script>
    <script
      src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js"
      integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6"
      crossorigin="anonymous"
    ></script>
  </body>
</html>
```

We will need to update `app.py`

```python
from flask import render_template
from app import app

# http://www.website.com/
@app.route('/')
def index():
    return render_template("static.html")
```

Let's try accessing our website again!

![Our new website](./todo_images/static_html.png)

You may have noticed, the content that we are serving to our users is static and the data won't change.
The buttons don't seem to work either. In the next few sections we will make sure to tackle these areas.

##### Dynamic Rendering and Passing Data

Often in websites, we want the content of a website to reflect the state or data being stored in it. So if we finish a task such as eating, we want it to only display the tasks remaining. 

Let's navigate to the `routes.py` file and make some changes.

`routes.py`

```python
from flask import render_template
from app import app

# http://www.website.com/
@app.route('/')
def index():
    title = 'My Personal Todo Application!'
    tasks = [
        {'name': 'Walk the dog'},
        {'name': 'Buy some groceries'},
        {'name': 'Listen to some music'},
        {'name': 'Tidy up my room'}
    ]
    return render_template("index.html", title=title, tasks=tasks)
```

Now we need to tell the function `render_template()` how to handle the tasks we are giving to it. We do this by passing `render_template` some variables, these can then be accessed inside the function and be used to declaratively how we want the variables we pass to look like.

Flask comes with Jinja2, a powerful template engine, `render_function()` integrates this with our application.

Let's make a copy of `static.html` and call it `index.html`. Alternatively, you can rename `static.html` to `index.html`.

We will make a few modifications to have it display the `title` and `task` that we passed into `render_template()`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- Bootstrap -->
    <link
      rel="stylesheet"
      href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css"
      integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk"
      crossorigin="anonymous"
    />
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{{title}}</title>
```

Replace line 13 with

```
FROM
	<title>Simple Todo App</title>
TO
	<title>{{title}}</title>
```

`{{ title }}` is our special way of telling Jinja2 that we would like it to reference a variable, it's a special placeholder that tells Jinja2 to replace `{{ title }}` with what we passed it from `render_template()`. This would be `My Personal Todo Application!` in our case.

The `{{ expression }}` construct handles a variety of different data types. It can handle other things such as lists, dictionaries as well as objects.

Now let's also tell it how we want our tasks list to be rendered.

Replace lines 38 to 76 with the following:

```html
{% for task in tasks %}
<tr>
    <th scope="row">{{loop.index}}</th>
    <td>{{task.name}}</td>
    <td>
        <a href="#"><button type="button" class="btn btn-primary" >View</button></a>
        <button
                type="button"
                class="btn btn-danger"
                >
            Delete
        </button>
    </td>
</tr>
{% endfor %}
```

`{% for task in tasks %}` tells us to loop through the task list

Fr every task in the list, we will produce a row in HTML to display it. We can access the contents of the task with `{{ task.name }}`

Now that we've made the changes, let's go back to http://127.0.0.1:5000/ and see our changes

![Passing down the data](./todo_images/pass_data.png)

Success!

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

![Add tasks using modals](./todo_images/modal.png)

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

### Deleting Tasks

You've made it all the way here though to this section. Great work!
You'll now learn another important part of managing data which is deleting items. 

We will look at how we can: 

* Create requests on our webpages to delete tasks using the Fetch
* Create and use a JavaScript function to complete the DELETE request
* How to handle DELETE methods in Flask

#### Update the Delete Button

First of all, let's modify the delete button in `index.html` to support the deletion of a task.

```html
<!--- REPLACE ME (LINE 46) --->
<button type="button" class="btn btn-danger">
    Delete
</button>

<!--- WITH THIS --->
<!-- Button trigger modal -->
<button
  type="button"
  data-item-id="{{task.id}}"
  data-toggle="modal"
  data-target="#deleteModal"
  class="btn btn-danger"
  onclick="{setDeleteId(this)}"
>
  Delete
</button>

```

Notice that we've added a few more attributes to the button HTML element. These properties include *data-item-id*, *data-toggle*, *data-target*, and *onClick*. We have explained *data-target* and *data-toggle* earlier, if you need to refresh your knowledge. Please refer to Adding a Task (Link here).  Setting`data-toggle="modal"`, allows us to set up a modal which targets `data-target="#deleteModal"` which will be a pop up modal that we will create shortly.

In HTML 5, we have a way to store additional information in an HTML element. We can do this with data-attributes. The way we define them is with `data-<our data name here>`, where we replace the spaces with additional dashes `-`s. For example, `data-example-item`.

We do this to attach the id, which is the unique identifier of the task in the element, that way when we delete it, we will know which task we will be referring to and will be able to send this back to our Flask server.

For more information on data attributes:

https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes

#### Creating the Delete Task Modal

Often for delete buttons, there is a confirmation window to make sure the user wanted to delete. Ocassionally, we misclick and delete something we don't intend to. So let us create a confirmation pop up window to prevent this from happening. 

This is what it will look like:

![Delete confirmation modal](./todo_images/are_you_sure.png)

We will append this after the first modal `<div>` or above the first `<script>` tag.

`index.html`

```html
</div>

<!-- INSERT ME HERE! Line (106) -->

<!-- Javascript support for bootstrap -->
    <script
        src="https://code.jquery.com/jquery-3.4.1.slim.min.js"
        integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n"
        crossorigin="anonymous"
    ></script>
```

Lets put the code below in index.html.

```html
<!-- Modal for Deleting a Task -->
<div
  class="modal fade"
  id="deleteModal"
  tabindex="-1"
  role="dialog"
  aria-labelledby="exampleModalLabel"
  aria-hidden="true"
>
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="deleteModalLabel">Modal title</h5>
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
        Are you sure you want to delete this task?
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">
          Cancel
        </button>
        <button
          type="button"
          class="btn btn-danger"
          onClick="deleteTask(deleteId)"
        >
          Delete
        </button>
      </div>
    </div>
  </div>
</div>
```

Now, when we click the delete task on an item row, a confirmation window will pop up asking if the user is sure that they want to delete the task. If they are sure, they will need to click the delete button.

```html
<button
  type="button"
  class="btn btn-danger"
  onClick="deleteTask(deleteId)"
>
```

Clicking on that delete button will call a function called `deleteTask()` which we will define in the next section.

#### Using JavaScript functions and Fetch to Delete the Item

Below the three JavaScript `<script>` tags, we will add the following:

```html
<script>
  var deleteId = 0;
  function setDeleteId(event) {
    deleteId = event.dataset.itemId;
  }
  function deleteTask(id) {
    console.log(deleteId);
    fetch("/item/" + id, {
      method: "DELETE",
    })
      .then(() => window.location.reload())
      .catch((error) => console.log("Something went wrong: " + error));
  }
</script>

```

In the first line, we declared a variable called deleteId. This is updated with `setDeleteId(event)` function. This is a callback that is called when we click on the delete button. The parameter being passed through is an object with information about the event that has just occurred such as what element was pressed, what type of event as well as lots of other information. Here, we are interested in the data attributes, which we can access via the "data-set" property which is a map that we can use to access any data variables we have assigned to the HTML element earlier. 

The function deleteTask is called when the confirmation popup to delete the task appears and the user clicks on the modal. When the user confirms `deleteTask(id)` is called, what it does is send a request to our server saying to delete the task. Once it completes successfully, the `.then()` clause runs and reloads our page with `window.location.reload()`.

For more information about events and onClick handling:

https://developer.mozilla.org/en-US/docs/Web/API/Event

https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers/onclick

For more about Promises (then, catch):

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

#### Handling the Delete Request on our Flask Server

Now we've done all we need from the user perspective with our web page by making appropriate changes. However, currently our server does not know what to do to handle it. We will add the following to our `routes.py`

```python
# DELETE (Delete a specific task id)
@app.route('/task/<int:id>', methods=['DELETE'])
def delete_task(id):
    task = Task.query.filter_by(id=id).first()
    
    # Check if Task exists
    if (task != None):
        msg = {
            'message': 'Delete successful'
        }
        db.session.delete(item)
        db.session.commit()
        return jsonify(msg), 200
	
    # Task does not exist
    msg = {
        'message': 'Task not found'
    }
    return jsonify(msg), 204
```

The function `delete_task(id)` takes in a parameter from the url, and finds the task from the database. If the id we receive is valid and the task exists in the database, that is given to us in ` task = Task.query.filter_by(id=id).first()`, we will delete it and commit in the database.





TODO: View + Update

TODO: Cloud Deployment.

