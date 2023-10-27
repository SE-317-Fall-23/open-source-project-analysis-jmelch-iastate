# Assignment Submission

## Project Selected: Flask

## I. Introduction
- Flask is a simple way to setup a web application using Python
- The two tests that I decided to talk about today both are related to error handling.  One is a test to make sure custom error handlers work and the other allows for testing built in exceptions which work in a similar way when you run into errors but instead return an exception.

## II. Test Case 1: test_error_handling
### A. Description
- This makes sure error handlers work and return the correct data and HTTP status code when an error occurs
### B. Test Steps
- Set the app testing mode to off
- Set up error handler for certain error codes
  - Error handler 404 returns the data string "not found" and the int 404 as the http status code
  - Error handler 500 returns the data string "internal server error" and the int 500 as the http status code
  - Error handler Forbidden returns the data string "forbidden" and the int 403 as the http status code
- Certain page routes are assigned errors to have the error handlers respond
  - Route ```/``` gets assigned a flask abort 404 to have it respond with the 404 error handler
  - Route ```/error``` gets assigned 1 // 0 to have it respond with the 500 error handler
  - Route ```/forbidden``` gets assigned flask abort 403 to have it respond with the 403 error handler
- A variable rv gets assigned with the data of getting those pages one by one
- assert is used to make sure the corect http status code and text is returned for each page
### C. Code Segments Under Test
```path
~\flask\tests\test_basic.py
```

I've gone ahead and commented the segemnts of the code getting tested below

```python
def test_error_handling(app, client):
    app.testing = False

    @app.errorhandler(404)  #Error handler being tested
    def not_found(e):
        return "not found", 404

    @app.errorhandler(500)  #Error handler being tested
    def internal_server_error(e):
        return "internal server error", 500

    @app.errorhandler(Forbidden)  #Error handler being tested
    def forbidden(e):
        return "forbidden", 403

    @app.route("/")  #Route certain error handler is being assigned to
    def index():
        flask.abort(404)

    @app.route("/error")  #Route certain error handler is being assigned to
    def error():
        1 // 0

    @app.route("/forbidden") #Route certain error handler is being assigned to
    def error2():
        flask.abort(403)

    rv = client.get("/")  #Gets the route
    assert rv.status_code == 404  #Tests the http code returned by the route
    assert rv.data == b"not found"  #Tests the data string returned by the route
    rv = client.get("/error")  #Gets the route
    assert rv.status_code == 500  #Tests the http code returned by the route
    assert b"internal server error" == rv.data  #Tests the data string returned by the route
    rv = client.get("/forbidden")  #Gets the route
    assert rv.status_code == 403  #Tests the http code returned by the route
    assert b"forbidden" == rv.data  #Tests the data string returned by the route
```
### D. Initial State
- There is an app and client object at the start of the test supplied to the test, these need to be assigned in the test with routes leading to errors so we know that error handlers work.
### E. Transition
- Error handlers are setup and then assigned to a route
### F. Expected State
- Each route that has been assigned an error handler returns the correct http status code and data telling you what the error code means
  
## III. Test Case 2: test_trapping_of_all_http_exceptions
### A. Description
- Makes sure when a client calls for something that has an error that an exception is raised
### B. Test Steps
- Change the app's config so that TRAP_HTTP_EXCEPTIONS is true
- Makes sure the webpage at the route ```/fail``` aborts with a 404
- Checks to see whether client.get raises a 404 Not found exception
### C. Code Segments Under Test
```path
~\flask\tests\test_basic.py
```

I commented the code with what is getting tested

```Python
def test_trapping_of_all_http_exceptions(app, client):
    app.config["TRAP_HTTP_EXCEPTIONS"] = True #This is what we are wanting to test to make sure exceptions are raised when enabling this

    @app.route("/fail") # This sets up an error for a certain page
    def fail():
        flask.abort(404)

    @app.route("/cant") #This is my code to test how this works
    def fail2():
        flask.abort(403)

    with pytest.raises(NotFound): #This makes sure that an exception was raised when you try to use the client to get the page
        client.get("/fail")

    with pytest.raises(Forbidden): #This is my code to test h ow this works
        client.get("/cant")
```
### D. Initial State
- There is a client and app supplied to the test.  The app needs to be configered in the test so that exceptions are thrown when the client runs into an error.
### E. Transition
- TRAP_HTTP_EXCEPTIONS is enabled in the app config and then a route is assigned with an error
### F. Expected State
- The client get request raises an exception matching the error that the website route was assigned to. 
