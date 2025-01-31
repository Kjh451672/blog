# Digital Storefront: Seeds n Shrubs

Hello again, and for this month's blog post, I figured I'd talk about my most recent Full-Stack Development Project. From November to the middle of December, I've been working on my own digital storefront that I dubbed ["Seeds n Shrubs"](http://http://127.0.0.1:5000/), which you can check out by clicking on the name. Seeds n Shrubs is a digital storefront that I created, which would have allowed users to purchase plant seeds, shrubs, gardening tools, plant pots, and etc. Now, as to how I created this whole thing and how it all works,let's start from the beginning.

## Creating Our Database

<img src="/blog/images/database_structure.png" height="435">

Each of us created our structures/frameworks for our databases in PHPMyAdmin. Each table/subset of data represents a different page/directory. Each one has a unique ID for that table, and to interact with the other "primary keys" with other tables. This way, each table can connect with each other, which then gets incorporated into the SQL, Flask, Python, HTML, Bootstrap and CSS we use later on in Visual Studio Code. We had to structure our databases to account for six things: The customer, the products, the user's cart, the checkout page, the Sale of the product, and the Sale ID of all of the orders. These six tables all work together to store and display information, to create a fully functional digital storefront.

## Python, Jinja, Flask & SQL

We start by creating a python file. This is where all of our "behind the scenes" work happens, such as connecting to our database, managing user login credentials and authentication, etc. Creating our routes for our website is actually very simple.


```python
#We use @app.route to define it as a webpage/directory
@app.route("/browse")
def product_browse():
   query = request.args.get('query')


#Establishes a connection to the database
   conn = connect_db()


   cursor = conn.cursor()
#SQL that works with the information from the database
   if query is None:
       cursor.execute(f"SELECT * FROM `Product`;")
   else:
       cursor.execute(f"SELECT * FROM `Product` WHERE `product` LIKE '%{query}%' OR `description` LIKE '%{query}%';")


   results = cursor.fetchall()
#Closes the connection, and returns the information to the jinja file respective of it
   cursor.close()
   conn.close()


   return render_template("browse.html.jinja", products = results, query = query)
```

We use "@app.route" to estables the website route, and create the class to handle all of the back end for that web page. The "conn" variable and "cursor" variable allow us to establish a connection to the database. The "cursor.executes" run SQL code to the database, pulling the information from it. With SQL, we have to be specific when calling from it, stating what table and what column from that table we need to link/access. Once all of that is said and done, we need to close the connection to our database, and return the "render_template" to our "html.jinja" page, which will handle all of the frontend development.

## Importing Flask & User Management
```python
#Importing tools from Flask
from flask import Flask, render_template, request, redirect, flash, abort
import pymysql
from dynaconf import Dynaconf
import flask_login


app = Flask(__name__)


conf = Dynaconf(
   settings_file = ["settings.toml"]
)


app.secret_key = conf.secret_key


#Establishing login_manager and creating user class
login_manager = flask_login.LoginManager()
login_manager.init_app(app)
login_manager.login_view = '/sign_in'


class User:
   is_authenticated = True
   is_anonymous = False
   is_active = True


   def __init__(self, user_id, username, email, first_name, last_name):
       self.id = user_id
       self.username = username
       self.email = email
       self.first_name = first_name
       self.last_name = last_name


   def get_id(self):
       return str(self.id)
```

To make things easier and more professional, we create a class called "User", that checks if they are active, not anonymous, and authenticated. Then, we use the same variables that are in our customer table, so we can utilize the information stored in the database whenever a user logs in or creates an account. With "login_manager" we are able to toggle access to users who are logged in or not, so that we prevent them from running into any errors by accessing pages they should no longer be needing to see.

```python
@app.route("/sign_up", methods =["POST", "GET"])
def sign_up_page():
   if flask_login.current_user.is_authenticated:
       return redirect("/")
   else:
       if request.method == 'POST':


           first_name = request.form["first_name"]
           last_name = request.form["last_name"]


           email = request.form["email"]
           address = request.form["address"]


           username = request.form["user_name"]
           password = request.form["password"]
           comfirm_password = request.form["verify_password"]


           conn = connect_db()


           cursor = conn.cursor()


           if comfirm_password != password:
               flash("Sorry, those passwords do not match")
           elif len(password) < 12:
               flash("Sorry, that password is too short")
           else:
               try:
                   cursor.execute(f"""
                       INSERT INTO `Customer`
                           (`first_name`, `last_name`, `username`, `password`, `email`, `address` )
                       VALUES
                           ( '{first_name}', '{last_name}', '{username}', '{password}', '{email}', '{address}' );
                   """)
               except pymysql.err.IntegrityError:
                   flash("Sorry, that username/email is already in use")
               else:
                   return redirect("/sign_in")
               finally:
                   cursor.close()
                   conn.close()




       return render_template("sign_up.html.jinja")
```
For example, if you were to take a look at the very first if-statement, it checks if the user is already logged in or not. If they aren't then it lets them create an account, by linking the database to a form in the html.jinja file, that stores the user information in the Customer table, which it pulls from whenever the user logs back in. The same thing applies to the cart and the checkout page, as if someone isn't logged in, they cannot access it. To be able to use all this stuff like render_template, redirect, flash, abort, and request, we have to import flask from Flask. From there, we can set up an "app" to work our ".route". Tools like flash and abort work alongside our login_manager, flashing error messages to the user whenever they attempt to access a non-accessible page.

## Back to Front(end): HTML & Jinja

Once we have all of our backend complete, we can start working on the frontend of our website. When working with "jinja.html", we have to put our html code inside of "{%%}". After that, it's a combination of for-loops, simple paragraphs and incorporations of bootstrap elements, we now have a fully functional digital storefront. This was one of the more in-depth projects that I've had to work on, and it's definitely safe to say that I've learned a lot. Our next and final project for the year will be our cohort project, where we utilize all of the schools that we've learned over the past two years. Until then, happy coding everyone.

