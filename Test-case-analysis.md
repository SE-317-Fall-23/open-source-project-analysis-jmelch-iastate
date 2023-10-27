# Assignment Submission

## Project Selected: Flask

## I. Introduction
- Briefly introduce the purpose of this section, which is to provide a detailed description of two test cases in the selected open-source project.

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
### E. Initial State
- There is a blank app and client object at the start of the test, these need to be assigned in the test.
### F. Transition
- Error handlers are setup and then assigned to a route
### G. Expected State
- Each route that has been assigned an error handler returns the correct http status code and data telling you what the error code means

## III. Test Case 2: [Name of Test Case]
### A. Description
- Provide a concise overview of the purpose of the second test case.
### B. Gherkin Syntax (if applicable)
- If you choose to use Gherkin syntax, write the Gherkin scenario for this test case.
### C. Test Steps
- Enumerate the sequence of steps or actions involved in this test case.
### D. Code Segments Under Test
- Identify specific code segments or functions that are being tested in this case.
```Java
// Enter code
```
### E. Initial State
- Describe the initial state or conditions of the system or component before executing the test.
### F. Transition
- Explain the actions or events that trigger a change in the system's state.
### G. Expected State
- Clearly outline the expected state or outcome after the test steps have been completed.
