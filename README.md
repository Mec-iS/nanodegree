# nanodegree-fullstack-final
### Project 3: Full-Stack Foundations Final
Designing and implementing a web-application to C.R.U.D. with a database, using `MySql Lite` and `Python2.7`.
There are also endpoints for serving data as REST service.

### PROJECT 5: set up the server

```
# ADD a user
$> adduser grader

# ADD the user to the sudo group
$> adduser grader sudo

# UPDATE all packages
$> apt-get update
```


### Running the app:

Git clone this repository:
```
$ git clone https://github.com/Mec-iS/nanodegree-fullstack-final
```

Install the necessary python libraries:
```
$ pip install -r requirements.txt
```

Add your GitHub OAUTH tokens in `libs\secret.py`.<br>
Change your database username and password in `libs\database_setup.py`.


Change directory to repository directory and:.

* Run the `lotofmenus.py` script in `tests`, to create the database and tables, and load some mock data.

* Run the server by running the `finalproject.py` script

Visit `127.0.0.1:5000` on your browser.



<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Licenza Creative Commons" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>