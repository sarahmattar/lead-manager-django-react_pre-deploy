# lead-manager-django-react - Pre Deploy

This is an in depth tutorial application build from Brad Traversy's Youtube series [Fullstack React & Django](https://www.youtube.com/playlist?list=PLillGF-RfqbbRA-CIUxlxkUpbq0IFkX60). This repo contains the app just before it is re-organized for deployment - all the files are added for Heroku's deployment (`runtime.txt`, `Procfile`, `Pipfile`, etc.) but I have not re-organized the Django application structure in the repo yet. To see how the repo gets restructured for Heroku deployment, check out the [lead_manager_django_react](https://github.com/sarahmattar/lead-manager-django-react) repo.

### Heroku-Specific Deploy Files

1. `runtime.txt` - this specifies the version of Python for Heroku to install.
2. `Procfile` - a Procfile tells Heroku that it should install the Python application as a WSGI app, and to migrate the database models to the production database once Django installs. 
3. `Pipfile` - this contains a list of the Python dependences for running the virtual environment (note that it is different from the `requirements.txt` list inside the Django app, which is referenced farther down).

### Why I Did This

Once I began to get comfortable with Django and creating template based frontends, I began to wonder: how would React fit on the front end? People use Django on the back end and React on the front end, but how? The answer to this was remembering that React components, while we are used to seeing them as UI "building blocks" in our application, eventually compile down to a single Javascript file when built for production. That Javascript file can be served as a static file along with any CSS files, and will be referenced inside `index.html`. 

### What I Learned from This

How to allow the backend of the application in Django to communicate with a non native front end in React.  Serializers via the Django Rest Framework convert the querysets and models from SQL to JSON based data for the front end, and Viewsets to manage permissions for each Model. 

How to configure Webpack for monitoring React component builds in development. 

Reinforcement of Redux knowledge for the front end state management. 

### Installation Instructions

Installation requires Python 3.8 and Django 3 to be installed on your local machine. 

1. Clone the project repository.

2. Use Pipenv to create a virtual environment, open the shell, and install Pipfile dependencies:
   1. `pip3 install pipenv`
   2. `pipenv shell`
   3. `pipenv install`
   
3. Install dependencies for the `lead_manager` Django application that are listed in `requirements.txt`:

   1. `cd lead_manager`
   2. `pip install -r requirements.txt`

4. Create a `.env` file for local settings, and place it in the `lead_manager` folder (not at the root of the project):

   ```.env
   SECRET_KEY=""
   DEBUG=True
   ```

   (Hint: You can auto-generate a unique secret key at [https://djcrety.ir](https:/djcrety.ir).)

5. Install NPM dependencies for Webpack to compile the React components:

   1. `cd ..` - our `package.json` file is at the root of the repository, not in the Django project folder, so we need to go up one level to get back to the root of the repository.
   2. `npm install`

6. Starting your Django server & run Webpack concurrently - this will require two Terminal windows/tabs: 

   | Terminal Tab 1               | Terminal Tab 2                                               |
   | ---------------------------- | ------------------------------------------------------------ |
   | `pipenv shell`               | `pipenv shell` (in case the second tab does not <br />have your virtual environment active) |
   | `cd lead_manager`            | `npm run dev`                                                |
   | `python manage.py runserver` |                                                              |

7. This will launch the application on `localhost:8000`, and Webpack will "watch" the `index.js` file for changes. Use `Ctrl-C` to stop the Django server at any time in the first window, and `Ctrl-C` to quit the Webpack compiling in the second window. 

8. You may get a notification in the Python terminal window that migrations have to be made to your local SQLite3 database, and possibly a Internal Server Error (500) in the browser. If this is the case, you can do this by running `python manage.py migrate`. Quit the server and Webpack, and restart both. Navigate to `http://localhost:8000`, and the Lead Manager homepage should load and redirect to `http://localhost:8000/#/login`. 

