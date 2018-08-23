# Linux_Server Configuration 

This is a Linux Server configuration web application written
to demonstrate the use of SQLAlchemy, CRUD, with Flask web framework.


When the user opens the web application, a list of books categories
are presented alongside with the title, description and price of
the book. A user is required to login to be able to add by editing
or remove by deleting a book item or a book category. 

The added book item or book category gets to be validated then saved
to the database and listed in the home page. Any erros gets to be
presented in the 404 page.


### Requirements

To complete this project, you'll need a *Linux server instance*.
For this project, we will be using *Heroku toolbelt* instead of
*Amazon Lightsail*.


### Description

* Install the heroku toolbelt

* Using heroku command line login by:

`$ heroku login`

* Enter you heroku credentials i.e. your email and password.

* Clone this project:

` $ git clone https://github.com/ednasawe/item_catalog.git`

` $ cd to item_catalog

* Create your application on heroku

* Then run:

` $ git remote add heroku https://git.heroku.com/itemcatalog.git`

` $ heroku create item_catalog`

* Create a Procfile and declare your command that will be used

  to run your application. In this case use:

` web: python3 project.py`

* Declare your dependancies in the _requirements.txt_ file

* To generate the dependancies use:

` $ pip freeze > requirements.txt`

*Deploy your app now by running the commands:

` $ git add Procfile`

` $ git add requirements.txt`

` $ git commit -m "Add Procfile and requirements.txt files`

` for heroku deployment"`

` $ git push heroku master`

* Add postgres by running the command:

` $ heroku pg:info`

` $ heroku addons:create heroku-postgresql:hobby-dev`

* Now the databse is set, let's configure the app to update

  the configuration by:

` $ heroku ps:scale web=1`

* Open your deployed heroku app using:

` $ heroku open`

* Here is the link to the app hosted on heroku [app](https://mybookscatalog.herokuapp.com/)


### Resource

*Choose A License* - Helpful website for picking out a license 
    or an application.
*Udacity* - Helpful in learning contents and resources to guide.


### How to Contribute

Contributions are welcomes. If you find any typos, errors,
or additional resources. First, fork this repository.
- You can Fork Icon the project.
- Or clone the repository to make changes.

$ git clone {REPOSITORY_CLONE_URL}
$ cd to the directory

- You can also Pull Request Icon by:

Making a pull request. Once you've pushed changes to
your local repository, you can issue a pull request
by clicking on the pull request icon.


### License

The contents of this repository are covered under the **MIT License**
