# Unit Test Prompts

Below are the list of prompts used to generate the final state. The prompts are designed to guide the user through the process of learning how to add unit tests with GitHub Copilot. The user can choose to follow the prompts in order, or skip around as needed. The prompts are designed to be flexible and can be adapted to the specific needs of the user.

**Note:** All prompts done in Visual Studio Code. Some agents and slash commands may not be available in other IDEs.

## Prompts

## Task: Learn how to add unit tests to a Python application

> **Note:** *@workspace how do I add unit tests to this Python application?*

<details>
   <summary><b>Copilot Response:</b></summary>

   <br>

To add unit tests to the Python application in your workspace, you can follow these steps, using the existing structure and examples from the exercises/refactoring/completed/tests/ directory as a guide:

1. **Create a Test Directory (if not already present):**

   - For the Task App, you might want to create a `tests` directory inside `apps/taskapp/src/` if it doesn't already exist. This will keep your tests organized and separate from your application code.

2. **Install Testing Libraries:**

   - Ensure you have `unittest`, a built-in Python testing framework, available in your environment. Since you're working in a Python environment (`venv/`), `unittest` should already be available. For more advanced testing features, you might consider installing `pytest` by adding it to your `requirements.in` and running `pip install -r requirements.in` to update `requirements.txt`.

3. **Create Test Files:**

   - Create test files for each module you want to test. For example, if you have `app.py` and `controllers.py` in `apps/taskapp/src/`, you might create `test_app.py` and `test_controllers.py` in the tests directory. Follow the naming convention of prefixing test files with `test_` to make it clear these are test files and to help with automatic discovery by testing frameworks.

4. **Write Test Cases:**

   - Use the `unittest` framework to write test cases. Each test case class should inherit from `unittest.TestCase`. Within each class, write methods that start with `test_` to define individual tests. You can look at the examples in `exercises/refactoring/completed/tests/test_calculator.py` for guidance on structuring your tests, using `setUp` for initialization, and employing assertions like `self.assertEqual`, `self.assertAlmostEqual`, and `self.assertRaises`.

5. **Mock External Dependencies:**

   - If your application interacts with external services or has side effects, consider using `unittest.mock` to replace these dependencies with mocks during testing. This allows you to test your application logic in isolation. For example, if `controllers.py` interacts with a database, you might mock those interactions in your tests to avoid hitting an actual database.

6. **Run Your Tests:**

   - You can run your tests directly from the command line using the Python interpreter:

      ```python
      python -m unittest discover -s path/to/your/tests/directory
      ```

   - If you're using `pytest`, running your tests can be as simple as executing `pytest` in your terminal.

7. **Integrate Tests with Development Workflow:**

   - Consider integrating your tests into your development workflow using a continuous integration (CI) system. This can help you automatically run tests on each commit or pull request, ensuring that changes do not break existing functionality.

By following these steps and utilizing the examples from the `exercises/refactoring/completed/tests/` directory, you can effectively add unit tests to your Python application, improving its reliability and maintainability.

</details>

## Task: First Pass at Unit Tests

Start by asking GitHub Copilot to generate a first pass at unit tests for the `Task` model in the Task App.

> **Note:** *@workspace /tests for TaskService class in #file:services.py. Use unittest library for testing. Use unittest.mock to mock the Task model defined in #file:models.py*

This prompt:

- uses the `/tests` command to generate the unit tests. 
- uses `#file:` to specify the file to test and the file to mock.
- Is specific about the testing library to use.

<details>
   <summary><b>Copilot Response:</b></summary>

<br>

````markdown
To generate tests for the `TaskService` class in `services.py`, we'll follow these steps:

1. **Mock the Task model and db session:** Since `TaskService` interacts with the database through the `Task` model and `db.session`, we'll mock these to avoid hitting the actual database.

2. **Test `get_all_tasks` method:** Mock `Task.query.all` to return a list of tasks and assert that `get_all_tasks` returns this list.

3. **Test `get_task_by_id` method:** Mock `Task.query.get` to return a specific task when given an ID and assert that `get_task_by_id` returns the correct task.

4. **Test `add_task` method:** Mock `db.session.add` and `db.session.commit` to ensure they are called with the correct parameters when a new task is added.

5. **Test `update_task` method:** Provide a mock task and assert that its attributes are updated correctly after calling `update_task`.

6. **Test `delete_task` method:** Mock `db.session.delete` and `db.session.commit` to ensure they are called when a task is deleted.

Here's how the tests could be implemented:

```python
import unittest
from unittest.mock import patch, MagicMock
from services import TaskService

class TestTaskService(unittest.TestCase):

    @patch('services.Task')
    def test_get_all_tasks(self, mock_Task):
        mock_Task.query.all.return_value = ['task1', 'task2']
        tasks = TaskService.get_all_tasks()
        self.assertEqual(tasks, ['task1', 'task2'])

    @patch('services.Task')
    def test_get_task_by_id(self, mock_Task):
        mock_Task.query.get.return_value = 'task'
        task = TaskService.get_task_by_id(1)
        self.assertEqual(task, 'task')

    @patch('services.db.session')
    @patch('services.Task')
    def test_add_task(self, mock_Task, mock_session):
        TaskService.add_task('title', 'description', False)
        mock_session.add.assert_called()
        mock_session.commit.assert_called()

    @patch('services.db.session')
    def test_update_task(self, mock_session):
        mock_task = MagicMock()
        updated_task = TaskService.update_task(mock_task, 'new title', 'new description', True)
        self.assertEqual(updated_task.title, 'new title')
        self.assertEqual(updated_task.description, 'new description')
        self.assertTrue(updated_task.done)
        mock_session.commit.assert_called()

    @patch('services.db.session')
    def test_delete_task(self, mock_session):
        mock_task = MagicMock()
        deleted_task = TaskService.delete_task(mock_task)
        mock_session.delete.assert_called_with(mock_task)
        mock_session.commit.assert_called()
        self.assertEqual(deleted_task, mock_task)
```

This code provides a basic structure for testing the `TaskService` class methods by mocking interactions with the database.
````

</details>

<br>

> **Prompt:** *How do I run the tests?*

<details>
   <summary><b>Copilot Response:</b></summary>

<br>

````markdown
To run the test in test_services.py using the unittest framework, you can use the command line. Here's how to do it on a Mac machine:

Open the Terminal application.
Navigate to the directory containing your test_services.py file using the cd command. For example, if your file is located in /Users/yourusername/projects/myproject/tests, you would enter:

```sh
cd /Users/yourusername/projects/myproject/tests
```

Run the test file using the Python interpreter. If you're using Python 3 (which is highly recommended), you can run the test by executing:

```sh
python3 -m unittest test_services.py
```

This command tells Python to run the unittest module as a script, passing the name of your test file as an argument. The unittest module will discover and run all tests defined in test_services.py according to the standard unittest test discovery rules.

````

<br>


</details>

## Task: Increasing Test Coverage

There is a limit to how much content can be generated in a single prompt. To generate more tests, you can start by figuring out what is missing from the existing tests.

> **Note:** *How do I check to see if I have full test cover for my class?*


<details>
   <summary><b>Copilot Response:</b></summary>

<br>
   
   ````markdown

To check if you have full test coverage for your class, you can use the `coverage` tool in combination with the `unittest` framework. Here's how you can do it:

1. **Install the `coverage` tool:**

   You can install the `coverage` tool using `pip`, which is the package installer for Python. To install `coverage`, open a terminal window and run the following command:

   ```sh
   pip install coverage
   ```

2. **Run your tests with coverage:**

   Once you have installed the `coverage` tool, you can run your tests with coverage enabled. To do this, you need to use the `coverage run` command followed by the command you would normally use to run your tests. For example, if you are using the `unittest` framework to run your tests, you would run the following command:

   ```sh
   coverage run -m unittest discover -s path/to/your/tests/directory
   ```

   Replace `path/to/your/tests/directory` with the actual path to your tests directory.

3. **Generate a coverage report:**

   After running your tests with coverage enabled, you can generate a coverage report using the `coverage report` command. This will show you the percentage of code that is covered by your tests. To generate a coverage report, run the following command:

   ```sh
   coverage report
   ```

   The coverage report will show you the percentage of code that is covered by your tests, as well as the lines of code that are not covered. This can help you identify areas of your code that are not being tested and may need additional test cases.

4. **View the coverage report:**

   Once you have generated a coverage report, you can view it in your terminal window. The coverage report will show you the percentage of code that is covered by your tests, as well as the lines of code that are not covered. You can use this information to identify areas of your code that are not being tested and may need additional test cases.

By following these steps, you can check if you have full test coverage for your class and identify areas of your code that may need additional test cases.

   ````

<br>

</details>

<br>

Coverage results example:

```txt
Name                     Stmts   Miss  Cover
--------------------------------------------
extensions.py                2      0   100%
models.py                    8      1    88%
services.py                 27      0   100%
--------------------------------------------
TOTAL                       37      1    97%
```

<br>

> **Prompt:** *According to the coverage tool, I seem to be missing tests for #file:models.py. There are 8 statements with 1 missed coverage. Can you help address this?*

<details>
   <summary><b>Copilot Response:</b></summary>

<br>

Given the context of models.py and the information about missed coverage, it seems likely that the __repr__ method of the Task class is not being tested. To address this, you can add a unit test specifically for the __repr__ method to ensure full coverage. Here's an example of how you might write such a test:

```python
import unittest
from models import Task

class TestTaskModel(unittest.TestCase):
    def test_repr_method(self):
        # Create a Task instance with specific attributes
        task = Task(id=1, title='Test Task', description='Test Description', done=False)
        
        # Expected representation
        expected_repr = "<Task Test Task>"
        
        # Assert that the __repr__ method returns the expected string
        self.assertEqual(repr(task), expected_repr)

if __name__ == '__main__':
    unittest.main()
```

This test creates an instance of the Task class, sets its attributes, and then checks if the __repr__ method returns the expected string representation. By adding this test, you should be able to cover the missed statement in your coverage report.

</details>

<br>

After re-running the coverage report again, here are the results:

```txt
Name            Stmts   Miss  Cover
-----------------------------------
extensions.py       2      0   100%
models.py           8      0   100%
services.py        27      0   100%
-----------------------------------
TOTAL              37      0   100%
```

## Task: Test-Driven Development

### Task: Write Failing Tests

In `test_services.py`:

> **Prompt:** *# Unit test for adding a task without a title*

<details>
   <summary><b>Copilot Response:</b></summary>

<br>

```python
 @patch('services.db.session')
    def test_add_task_no_title(self, mock_session):
        with self.assertRaises(ValueError) as context:
            TaskService.add_task('', 'description', False)
        self.assertEqual(str(context.exception), 'Task title is required')
        mock_session.add.assert_not_called()
        mock_session.commit.assert_not_called()
```

</details>

<br>

> **Prompt:** *# Unit test for adding a task with a title that is too long (over 255 characters)*

<details>
   <summary><b>Copilot Response:</b></summary>

<br>

```python
@patch('services.db.session')
def test_add_task_title_too_long(self, mock_session):
   with self.assertRaises(ValueError) as context:
      TaskService.add_task('a' * 256, 'description', False)
   self.assertEqual(str(context.exception), 'Task title must be 255 characters or less')
   mock_session.add.assert_not_called()
   mock_session.commit.assert_not_called()

```

</details>

<br>

> **Prompt:** *# Unit test for a task with a description that is too long (over 1000 characters)*

<details>
   <summary><b>Copilot Response:</b></summary>

```python
    @patch('services.db.session')
    def test_add_task_description_too_long(self, mock_session):
        with self.assertRaises(ValueError) as context:
            TaskService.add_task('title', 'a' * 1001, False)
        self.assertEqual(str(context.exception), 'Task description must be 1000 characters or less')
        mock_session.add.assert_not_called()
        mock_session.commit.assert_not_called()
```

</details>

<br>

The new unit tests should fail with a similar output:



```sh
$ coverage run -m unittest discover -s tests

.FFF.....
======================================================================
FAIL: test_add_task_description_too_long (test_services.TestTaskService.test_add_task_description_too_long)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/opt/homebrew/Cellar/python@3.12/3.12.4/Frameworks/Python.framework/Versions/3.12/lib/python3.12/unittest/mock.py", line 1390, in patched
    return func(*newargs, **newkeywargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/bxtp4p/Documents/copilot-exercises/copilot-app-taskapp/tests/test_services.py", line 66, in test_add_task_description_too_long
    with self.assertRaises(ValueError) as context:
AssertionError: ValueError not raised

======================================================================
FAIL: test_add_task_no_title (test_services.TestTaskService.test_add_task_no_title)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/opt/homebrew/Cellar/python@3.12/3.12.4/Frameworks/Python.framework/Versions/3.12/lib/python3.12/unittest/mock.py", line 1390, in patched
    return func(*newargs, **newkeywargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/bxtp4p/Documents/copilot-exercises/copilot-app-taskapp/tests/test_services.py", line 48, in test_add_task_no_title
    with self.assertRaises(ValueError) as context:
AssertionError: ValueError not raised

======================================================================
FAIL: test_add_task_title_too_long (test_services.TestTaskService.test_add_task_title_too_long)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/opt/homebrew/Cellar/python@3.12/3.12.4/Frameworks/Python.framework/Versions/3.12/lib/python3.12/unittest/mock.py", line 1390, in patched
    return func(*newargs, **newkeywargs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/bxtp4p/Documents/copilot-exercises/copilot-app-taskapp/tests/test_services.py", line 57, in test_add_task_title_too_long
    with self.assertRaises(ValueError) as context:
AssertionError: ValueError not raised

----------------------------------------------------------------------
Ran 9 tests in 0.010s

```

### Task: Make the Tests Pass

It makes sense to encapsulate the validation logic for tasks in a separate class `TaskValidation` and to use this class in the `TaskService` class to validate tasks before adding them to the database. This will help keep the `TaskService` class clean and focused on interacting with the database. The `TaskValidation` could be reused by other parts of the application that need to validate tasks.

> **Prompt:** *@workspace Add a TaskValidation class that the TaskService can use to validate if a description is longer than 1000 characters and if a title is present and longer than 255 characters. #file:services.py #file:test_services.py*

<details>
   <summary><b>Copilot Response:</b></summary>

````markdown
To add a `TaskValidation` class to the `services.py` file and integrate it with the `TaskService` class for validating task descriptions and titles, follow these steps:

### Step 1: Define the `TaskValidation` Class in `services.py`

```python
class TaskService:
    @staticmethod
    def add_task(title, description, done):
        TaskValidation.validate_title(title)
        TaskValidation.validate_description(description)
        new_task = Task(
                title=title,
                description=description,
                done=done
        )
        db.session.add(new_task)
        db.session.commit()
        return new_task
```

### Step 2: Integrate `TaskValidation` with `TaskService` in `services.py`

Modify the `add_task` method in the `TaskService` class to use `TaskValidation` for validating the title and description before creating a new task.

```python
class TestTaskValidation(unittest.TestCase):
    def test_validate_title_with_no_title(self):
        with self.assertRaises(ValueError) as context:
            TaskValidation.validate_title('')
        self.assertEqual(str(context.exception), 'Task title is required')

    def test_validate_title_with_long_title(self):
        with self.assertRaises(ValueError) as context:
            TaskValidation.validate_title('a' * 256)
        self.assertEqual(str(context.exception), 'Task title must be 255 characters or less')

    def test_validate_description_with_long_description(self):
        with self.assertRaises(ValueError) as context:
            TaskValidation.validate_description('a' * 1001)
        self.assertEqual(str(context.exception), 'Task description must be 1000 characters or less')
```

### Step 3: Update Tests in `test_services.py`

Since the validation logic is now encapsulated within the `TaskValidation` class, you might want to add tests specifically for this class to ensure its correctness. However, since the validation methods are static and raise exceptions for invalid inputs, your existing tests for `TaskService.add_task` that check for `ValueError` should still be valid. You may want to add direct tests for `TaskValidation` to explicitly test the validation logic.

```python
class TestTaskValidation(unittest.TestCase):
    def test_validate_title_with_no_title(self):
        with self.assertRaises(ValueError) as context:
            TaskValidation.validate_title('')
        self.assertEqual(str(context.exception), 'Task title is required')

    def test_validate_title_with_long_title(self):
        with self.assertRaises(ValueError) as context:
            TaskValidation.validate_title('a' * 256)
        self.assertEqual(str(context.exception), 'Task title must be 255 characters or less')

    def test_validate_description_with_long_description(self):
        with self.assertRaises(ValueError) as context:
            TaskValidation.validate_description('a' * 1001)
        self.assertEqual(str(context.exception), 'Task description must be 1000 characters or less')
```

These changes will help you centralize and manage the validation logic for tasks more effectively, making the `TaskService` class cleaner and more maintainable.
````

</details>

<br>

## Task: Reuse the TaskValidation Class

### Task: Add Unit Tests for Editing Tasks

> **Prompt:** *# Unit test for updating a task with a title that is too long (over 255 characters)*

<details>
   <summary><b>Copilot Response:</b></summary>

<br>

```python
@patch('services.db.session')
def test_update_task_title_too_long(self, mock_session):
   mock_task = MagicMock()
   with self.assertRaises(ValueError) as context:
      TaskService.update_task(mock_task, 'a' * 256, 'description', False)
   self.assertEqual(str(context.exception), 'Task title must be 255 characters or less')
   mock_session.commit.assert_not_called()
```

</details>

<br>

> **Prompt:** *# Unit test for updating a task with a description that is too long (over 1000 characters)*

<details>
   <summary><b>Copilot Response:</b></summary>

<br>

```python
@patch('services.db.session')
def test_update_task_description_too_long(self, mock_session):
   mock_task = MagicMock()
   with self.assertRaises(ValueError) as context:
      TaskService.update_task(mock_task, 'title', 'a' * 1001, False)
   self.assertEqual(str(context.exception), 'Task description must be 1000 characters or less')
   mock_session.commit.assert_not_called()
```

</details>

<br>

> **Prompt:** *# Unit test for updating a task with an empty title*

<details>
   <summary><b>Copilot Response:</b></summary>

<br>

```python
@patch('services.db.session')
def test_update_task_no_title(self, mock_session):
   mock_task = MagicMock()
   with self.assertRaises(ValueError) as context:
      TaskService.update_task(mock_task, '', 'description', False)
   self.assertEqual(str(context.exception), 'Task title is required')
   mock_session.commit.assert_not_called()
```

</details>

<br>

### Task: Refactor `update_task` in `TaskService` to pass the new tests

> **Prompt:** *@workspace Update the `update_task` method in the `TaskService` class to use the `TaskValidation` class for validating the title and description before updating a task. #file:services.py #file:test_services.py*

<details>
   <summary><b>Copilot Response:</b></summary>

<br>

```python
class TaskService:
@staticmethod
def update_task(task, title, description, done):
   """
   Update an existing task.

   Parameters:
   - task: The Task instance to update.
   - title: The new title for the task.
   - description: The new description for the task.
   - done: Boolean indicating the new done status.

   Returns:
   - The updated Task instance.
   """
   TaskValidation.validate_title(title)
   TaskValidation.validate_description(description)
   
   task.title = title
   task.description = description
   task.done = done
   db.session.commit()
   return task
```

</details>


## Task: Functional Testing

> **Note:** *@workspace what Python libraries and tools can I use to do functional testing?*

<br>

<details>
   <summary><b>Copilot Response:</b></summary>

<br>

````markdown
To add functional tests to your Flask application, you can follow these steps, leveraging the Flask testing tools and the structure of your existing tests. Since you're already using `unittest` for unit testing, you can continue with this framework for functional testing. Functional tests will interact with your application's endpoints to ensure they behave as expected when served.

1. **Create a New Test File**: Create a new test file in your `tests` directory. You might name it `test_app.py` to indicate it contains tests for the application's routes and overall behavior.
    
2. **Setup Test Configuration**: In your test file, import the necessary modules and configure your Flask application for testing. This often involves creating a new instance of your Flask application configured to use a different database (e.g., an in-memory SQLite database) to avoid interfering with your production or development database.
    
3. **Write Test Cases**: Write test functions for each endpoint you wish to test. Use the Flask test client (`app.test_client()`) to make requests to your application and assert the responses. You can test for correct response codes, response data, and database state after requests.
    

Here's an example of how you might structure `test_app.py`:

```python
import unittest
from flask import json
from app import app
from extensions import db

class FunctionalTest(unittest.TestCase):
    def setUp(self):
        self.app = app.test_client()
        self.app.testing = True
        with app.app_context():
            # Setup a test database, if necessary
            db.create_all()

    def tearDown(self):
        with app.app_context():
            # Teardown the test database, if necessary
            db.session.remove()
            db.drop_all()

    def test_get_tasks(self):
        response = self.app.get('/tasks/')
        self.assertEqual(response.status_code, 200)
        # Additional assertions on the response data can be made here

    def test_add_task(self):
        response = self.app.post('/tasks/add', data=dict(title="Test Task", description="Test Description"), follow_redirects=True)
        self.assertEqual(response.status_code, 200)
        # Verify the task was added to the database

    # Add more tests as needed for other endpoints

if __name__ == '__main__':
    unittest.main()
```

4. **Run Your Tests**: Run your functional tests using the same command or setup you use for your unit tests. If you're using `pytest`, you can simply run `pytest` in your terminal. If you're using Python's built-in `unittest` framework, you can run your tests with `python -m unittest discover tests`.

Remember to add any additional dependencies you might need for testing (e.g., for handling test databases) to your `requirements.in` file and update your environment accordingly.
````

</details>

<br>


## Task: Performance Testing

> **Prompt:** *@workspace I want to add performance testing to my API in #file:resources.py. What do you recommend?*

<details>
   <summary><b>Copilot Response:</b></summary>

<br>

````markdown
To add performance testing to your API in [`resources.py`](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "#file:resources.py-context"), you can use a combination of tools and approaches. Here's a recommended strategy:

1. **Locust**: An open-source load testing tool. It allows you to write test scenarios in Python and simulate millions of simultaneous users. This is particularly useful for testing how your API handles high traffic and identifying bottlenecks.
    
2. **Apache JMeter**: A popular open-source tool designed for load testing and measuring performance. It can be used to test performance on static and dynamic resources, and it provides a variety of graphical analyses of performance reports.
    
3. **PyTest-Benchmark**: If you're already using PyTest for unit testing, PyTest-Benchmark can be integrated to add benchmark tests for your API endpoints. This allows you to keep performance tests close to your unit tests and use a familiar framework.
    
4. **Profiling Middleware**: For Flask applications, you can integrate middleware that profiles your API requests and responses. This can help identify slow parts of your API under normal conditions. Flask has extensions like Flask-Profiler or Flask-MonitoringDashboard for this purpose.
    
5. **Custom Load Testing Scripts**: Using `requests` or `httpx` libraries in Python, you can write custom scripts to simulate API requests. This approach gives you full control over the test scenarios and allows you to tailor the load testing to your specific needs.
    

Here's a simple example using Locust to test the performance of fetching all tasks from your API:

```python
from locust import HttpUser, task, between

class ApiUser(HttpUser):
    wait_time = between(1, 5)

    @task
    def get_tasks(self):
        self.client.get("/tasks")
```

This script defines a Locust user that makes GET requests to the `/tasks` endpoint. You can run this test with Locust to simulate multiple users accessing your API simultaneously and measure its performance.

Remember to run these tests in a controlled environment (not on your live API) to avoid impacting real users.
````

</details>
