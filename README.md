# Assignment 3 Python
## Installation:
  ```bash
  pip install BeautifulSoup
  ```
  [Beautiful Soup Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
  
  Download **flask_server.py** file from **src/** to your project folder
## Usage
  Firstly, you need to change the following row based on your postgresql settings
  ```bash
  app.config['SQLALCHEMY_DATABASE_URI'] = "postgresql://<username>:<password>@localhost:<port>/<db_name>"
  ```
  You can run **flask_server.py** file with IDE or on command line(```python flask_server.py```).
  
  Then you have to open your browser or requests making program and make requests. It has 3 ways:
  
  - /sign_up (post and get)
  - /login (post and get)
  - /protected (use token as parameter)
  
  The second option is a database managing without running a local server.
  ```bash
  >>> from flask_server import db
  >>> from task.flask_server import Users
  ```
  
  Look at the Users model below, which has id, username, password and token fields. Username and token fields are unique. The constructor accepts only username and password.
  ```bash
  class Users(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    password = db.Column(db.String(120), nullable=False)
    token = db.Column(db.String(200), unique=True, nullable=True)

    def __init__(self, username, password):
        self.username = username
        self.password = password

    def __repr__(self):
        return '<User %r>' % self.username
  ```
## Example
```bash
  >>> db.create_all()   # creates tables in psql based on models

>>> username = "example_user" 
>>> password = "123" 

>>> user = Users(username, password) # creates a user 

>>> db.session.add(user) 
>>> db.session.commit()   # adds this row to psql users table

>>> Users.query.all()      
[<User 'asdfg'>, <User 'asdfwer'>, <User 'asdf'>, <User 'asdgas'>, <User 'example_user'>]

>>> Users.query.filter_by(username = "example_user").first()
<User 'example_user'>

>>> check_user = Users.query.filter_by(username = "example_user").first()
>>> print ("username: " + check_user.username + "\npassword: " + check_user.password) 
username: example_user
password: 123


```
 
