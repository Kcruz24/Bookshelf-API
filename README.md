# The Great Bookshelf of Udacity

### Description
This is one of the Udacity projects to practice API Development and 
documentation. It follows the REST arquitectural style and performs all CRUD 
operations. It implements basic error handling and testing with Python unittests. 
All errors are formatted to be returned as JSON objects instead of HTML 
and all routes also return JSON objects to connect with the frontend. Lastly, the 
database is implemented with PostgreSQL.

### Code Style
The backend is built with Python utilizing the Flask micro framework 
and follows the PEP8 code style guidelines.

## Getting Started

### Local Development
The instructions below will guide you through the process of running the application 
locally on your machine.

#### Prerequisites

- The Latest version of Python, pip and node should already be installed on your machine.
- **Start a virtual environment** from the backend folder. If you don't know how to
start your own virtual environment, below are the instructions to do so:
  
  ```
  # Mac users
  python -m venv venv 
  source venv/bin/activate
  
  # Windows users on Git Bash, not CMD
  > py -m venv venv
  > venv/Scripts/activate
  ```

If you're using the PyCharm IDE you can start a virtual environment following 
these [instructions](https://www.jetbrains.com/help/pycharm/creating-virtual-environment.html#python_create_virtual_env).
From the [PyCharm official docs](https://www.jetbrains.com/help/pycharm/quick-start-guide.html) website.

- **Install dependencies**. From the backend folder run: \
  ```
  pip install -r requirements.txt
  ```

### Step 0: Check that you have PostgreSQL installed.
First you must verify that you have postgres installed on your machine, to verify
run the commands below:
```
which postgres
postgres --version 
```
The `which postgres` command should output something like this:
``` 
/k/PostgreSQL/13/bin/postgres
```
That's the path in which postgres is installed on your machine.

The `postgres --version` command should output the postgres version you have installed on your machine.
Make sure is the latest to avoid any problem with outdated versions.

### Step 1: Start/Stop the PostgreSQL server.
Mac users can follow the commands below:
```
pg_ctl -D /usr/local/var/postgres start
pg_ctl -D /usr/local/var/postgres stop
```

Windows users can follow the commands below:
- Find the database directory, it could be something like this: `C:\Program File\PostgreSQL\13.3\data`
the path depends on where you installed postgres on your machine.
- Then, in the command line (Git Bash), execute the following command:
  ```
  # Start the server
  pg_ctl -D "C:\Program File\PostgreSQL\13.3\data" start
  # Stop the server
  pg_ctl -D "C:\Program File\PostgreSQL\13.3\data" stop
  # Restart the server
  pg_ctl -D "C:\Program File\PostgreSQL\13.3\data" restart
  ```

if it shows that the *port already occupied* error, run:
```
sudo su -
ps -ef | grep postmaster | awk '{print $2}'
kill <PID>
```

### Step 2 - Create and Populate the database

1. **Verify that the database**  
   Verify that the database user in the `/backend/books.psql`, `/backend/models.py`, and `/backend/test_flaskr.py` files match
   your database username.
2. **Create the database**
   In your terminal, navigate to the `/backend` directory path and run the following command:
   ```
   # Connect to the PostgreSQL
   psql <your database username>
   
   # View all databases
   \l
   
   # Create the database
   \i setup.sql
   
   # Exit the PostgreSQL prompt
   \q
   ```
3. **Create tables** 
   Once your database is created, you can create tables (`bookshelf`) and apply constraints.
   
   ```
   # Mac & Windows users
   psql -f books.psql -U <Your database username> -d bookshelf
   
   # Linux users
   su - postgres bash -c "psql bookshelf < /path/to/backend/books.psql"
   ```
   
### Start the backend server
From the `/backend/` directory run:
```
# Mac users
export FLASK_APP=flaskr
export FLASK_ENV=development
flask run

# Windows users on CMD
set FLASK_APP=flaskr
set FLASK_ENV=development
flask run
```
These commands put the application in development and directs our application to 
use the `__init__.py` file in our *flaskr* folder.

The application will run on `http://127.0.0.1:5000` by default and is set as
a proxy in the frontend configuration. 

#### Authentication
- The current version of the application does not require authentication or 
API keys.
  
### Step 4: Start the frontend
(You can start the frontend before the backend is up if you want) From the `/frontend`
folder, run the following commands to start the client:
```
npm install // Only once to install dependencies
npm upgrade // To upgrade any outdated dependencies
npm start
```
By default, the frontend will run on `localhost:3000`. You can close the 
terminal if you wish to stop the frontend server.

### Runnning Tests
If any route needs testing, navigate to the `/backend` folder and
run the following commands:
```
psql bookshelf_test < books.psql
python test_flaskr.py
```
Also, make sure to change the database from `bookshelf` to `bookshelf_test` 
on this files `/backend/models.py` and `/backend/test_flaskr.py`.

---
## API Reference

### Getting Started
- Base URL: At present this app can only be run locally and is not hosted as a base URL. The backend app is hosted at the default, `http://127.0.0.1:5000/`, which is set as a proxy in the frontend configuration. 
- Authentication: This version of the application does not require authentication or API keys. 

### Error Handling
Errors are returned as JSON objects in the following format:
```
{
    "success": False, 
    "error": 400,
    "message": "bad request"
}
```
The API will return three error types when requests fail:
- 400: Bad Request
- 404: Resource Not Found
- 405: Method not allowed
- 422: Not Processable

### Endpoints 
#### GET /books
- General:
    - Returns a list of book objects, success value, and total number of books
    - Results are paginated in groups of 8. Include a request argument to choose page number, starting from 1. 
- Sample: `curl http://127.0.0.1:5000/books`

``` {
  "books": [
    {
      "author": "Stephen King",
      "id": 1,
      "rating": 5,
      "title": "The Outsider: A Novel"
    },
    {
      "author": "Lisa Halliday",
      "id": 2,
      "rating": 5,
      "title": "Asymmetry: A Novel"
    },
    {
      "author": "Kristin Hannah",
      "id": 3,
      "rating": 5,
      "title": "The Great Alone"
    },
    {
      "author": "Tara Westover",
      "id": 4,
      "rating": 5,
      "title": "Educated: A Memoir"
    },
    {
      "author": "Jojo Moyes",
      "id": 5,
      "rating": 5,
      "title": "Still Me: A Novel"
    },
    {
      "author": "Leila Slimani",
      "id": 6,
      "rating": 5,
      "title": "Lullaby"
    },
    {
      "author": "Amitava Kumar",
      "id": 7,
      "rating": 5,
      "title": "Immigrant, Montana"
    },
    {
      "author": "Madeline Miller",
      "id": 8,
      "rating": 5,
      "title": "CIRCE"
    }
  ],
"success": true,
"total_books": 18
}
```

#### POST /books
- General:
    - Creates a new book using the submitted title, author and rating. Returns the id of the created book, success value, total books, and book list based on current page number to update the frontend. 
- `curl http://127.0.0.1:5000/books?page=3 -X POST -H "Content-Type: application/json" -d '{"title":"Neverwhere", "author":"Neil Gaiman", "rating":"5"}'`
```
{
  "books": [
    {
      "author": "Neil Gaiman",
      "id": 24,
      "rating": 5,
      "title": "Neverwhere"
    }
  ],
  "created": 24,
  "success": true,
  "total_books": 17
}
```
#### DELETE /books/{book_id}
- General:
    - Deletes the book of the given ID if it exists. Returns the id of the deleted book, success value, total books, and book list based on current page number to update the frontend. 
- `curl -X DELETE http://127.0.0.1:5000/books/16?page=2`
```
{
  "books": [
    {
      "author": "Gina Apostol",
      "id": 9,
      "rating": 5,
      "title": "Insurrecto: A Novel"
    },
    {
      "author": "Tayari Jones",
      "id": 10,
      "rating": 5,
      "title": "An American Marriage"
    },
    {
      "author": "Jordan B. Peterson",
      "id": 11,
      "rating": 5,
      "title": "12 Rules for Life: An Antidote to Chaos"
    },
    {
      "author": "Kiese Laymon",
      "id": 12,
      "rating": 1,
      "title": "Heavy: An American Memoir"
    },
    {
      "author": "Emily Giffin",
      "id": 13,
      "rating": 4,
      "title": "All We Ever Wanted"
    },
    {
      "author": "Jose Andres",
      "id": 14,
      "rating": 4,
      "title": "We Fed an Island"
    },
    {
      "author": "Rachel Kushner",
      "id": 15,
      "rating": 1,
      "title": "The Mars Room"
    }
  ],
  "deleted": 16,
  "success": true,
  "total_books": 15
}
```
#### PATCH /books/{book_id}
- General:
    - If provided, updates the rating of the specified book. Returns the success value and id of the modified book. 
- `curl http://127.0.0.1:5000/books/15 -X PATCH -H "Content-Type: application/json" -d '{"rating":"1"}'`
```
{
  "id": 15,
  "success": true
}
```