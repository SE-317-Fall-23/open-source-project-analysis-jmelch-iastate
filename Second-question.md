## Project Selected: <Enter project name>

## I. Introduction
- Provide an overview of the analysis, indicating the goal of describing the types of testing used in the selected open-source project and explaining the rationale behind their choice.

## II. Types of Testing in the Project
### A. Unit Testing
- Describe the role and purpose of unit testing in the project.
- Identify specific components or modules that are subjected to unit testing.
- Mention how unit tests are executed and the tools or frameworks used.
  - There are a ton of things that can be run individualy before everything is connected together in this project.  Unit testing allows for each individual part that could have an issue to be seperated from the rest of the program to make sure it works.
  - One such area that has unit testing building configs from different file types.  These tests are all seperated from the rest of the program so you can test these config builders seperately from the rest of the program
  - These tests are all run automatically by pytest

### B. Integration Testing
- Explain the importance of integration testing in the context of the project.
- Identify the interactions between different modules, services, or components that are tested.
- Highlight the methods used for conducting integration tests and tools employed.
  - When designing an application that creates a website using python there are a lot of different things that need to interact.  One of those big things would be how a web browser would reeact to what flask is hosting.
  - Because I don't have the time to dive into how that process of python to HTML works I'm instead going to look into the tests dealing with the flask application and the logger.  When something goes wrong you would want to be able to find a place to look to see what that issue is.  That's why there is a logger interacting with the other components to collect that data to troubleshoot.
  - One method used to see how the logger interacts with everything else is test_log_view_exception.  It purposely raises an exception so that it can be tested whether or not the logger was able to store that information.

### C. UI (User Interface) Testing
- Describe the significance of UI testing in the project, especially if it's a web application or software with a graphical user interface.
- Explain which aspects of the user interface are tested (e.g., functionality, usability, responsiveness).
- Mention the testing frameworks, tools, or automation scripts used for UI testing.

- UI testing would be important for anything created by this tool.  It doesn't really have a UI of it's own.  Due to that ui really isn't tested for this application.

- If this was a webapp it would be a good idea to use a testing tool like Cypress to navigate through everyhting to make sure it works how it's supposed to.  It would also be a good idea to have users testing the application to see how user friendly the UI is to use.

### D. Other Types of Testing (if applicable)
- There is regression testing to make sure a certain bug doesn't get reintroduced, more specifically there is only one test.

## III. Reasons for Choosing These Testing Types
- There are a lot of individual pieces that need to interact when creating a piece of software like this.  The unit testing allows for the developers to have a bit more granularity on what's going on.  If there is something going wrong with integration testing this can help narrow down the issue depending on whether or not it's a piece of in the application having the issue.
- Integration testing is more broad but makes sure everything that was written individually works together with everything else.
- The regression testing will just help to make sure a bug that has been found before doesn't get reintroduced.

## 2. Test Data Generation
### A. Static Test Data
- Explain if and how static test data is used in the project.
- Provide examples of scenarios where static test data is employed.

- Static test data is used for most of these tests.  An example being this:
```Python
def test_baseexception_error_handling(app, client):
    app.testing = False

    @app.route("/")
    def broken_func():
        raise KeyboardInterrupt()

    with pytest.raises(KeyboardInterrupt):
        client.get("/")
```
The data is set and then never changes after that
### B. Dynamic Test Data
- Explain if and how dynamic test data is used in the project.
- Provide examples of scenarios where dynamic test data is generated.

- While looking through the tests for this project I stil haven't run accross any tests where the data is really being changed during the test.  A constant is set at the begining of the test and never changed again.

## 3. Test Doubles
- Identify and explain the use of test doubles (e.g., mocks, stubs, fakes) in the project.
- Specify the specific components, functions, or cases where test doubles are applied.
- Discuss the purpose of using these test doubles in the testing strategy.

- Stubs are used a lot for triggering errors in some of these tests for error handling.  It really doesn't matter how an error is tiggered as long as there is an error to test.  That's where the flask.abort() call comes in handy.  You can make a route have an error by sepcifying flask.abort(<http-status-code-with-error>).  This makes it easy to test this error handling.

## 4. Discussion on Testing Practices
<!-- 
To find discussions on testing strategy in a GitHub repository, you can follow these steps:

Visit the GitHub repository for the project you're interested in.
Look for the "Issues" tab on the repository's page.
Use the search bar within the Issues tab to search for terms related to testing, such as "testing strategy," "test cases," or "test automation."
-->
- Investigate any discussions related to testing practices within the project.
- Give a link to the Github PR or issue
- Summarize key points or insights from these discussions.
- Offer your interpretation of how the project's testing practices align with industry standards or best practices.

- https://github.com/pallets/flask/issues/1200
  - When this issue was opened there was somebody who was wondering how something worked in the code.  Eventually this was explained by pointing that user to a test case where they could experiment for themselves what they were wondering about
  - Not only do these test cases get used to make sure everything is working, they also can be a good way to allow somebody who wants to get their hands dirty and learn the code a good way to experiment with things they might not totally understand.
  - Somebody came back on later and found what more than likely was a bug.  Other develpoers promptly got to working fixing the issue when it was disclosed.
