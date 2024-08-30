# Adding Unit Tests

This exercise focuses on unit tests. Using the [Task App](https://github.com/bxtp4p/copilot-app-taskapp), add unit tests to the app to ensure that it behaves as expected.

## Problem

The [Task App](https://github.com/bxtp4p/copilot-app-taskapp) has no unit tests. This makes it difficult to ensure that the app behaves as expected and that future changes do not introduce regressions.

## Task

Add unit tests to the Task App to ensure that it behaves as expected. The tests should cover the following scenarios:

- **Task Creation:**

  - Ensure that a task can be created with a title and description.
  - Ensure that a task cannot be created without a title.
  - Ensure that a task cannot be created with a title that is too long.
  - Ensure that a task cannot be created with a description that is too long.

- **Task Completion:**

  - Ensure that a task can be marked as complete.
  - Ensure that a task can be marked as incomplete.

- **Task Retrieval:**

  - Ensure that a task can be retrieved by ID.
  - Ensure that a task cannot be retrieved if it is not in the list of tasks.

- **Task Modification:**

  - Ensure that a task can be modified.

- **Task Deletion:**

  - Ensure that a task can be deleted.

- **Task Listing:**

  - Ensure that a list of tasks can be retrieved.

## Expected Outcome

After completing this exercise, you should have a suite of unit tests that cover the scenarios listed above. The tests should be able to run in isolation and should not depend on any external services or resources. The application code should be refactored as needed to make it more testable and in order to pass the tests.

- **Use a Testing Framework:**
  - Use a testing framework like `unittest` or `pytest` to write your tests. These frameworks provide utilities for setting up and tearing down test fixtures, as well as for running tests and reporting results.

- **Use Mocks and Stubs:**
  - Use mocks and stubs to isolate the code under test from its dependencies. This will make your tests more reliable and easier to maintain.

## Bonus Points

- **Code Coverage:**
  - Use a code coverage tool like `coverage.py` to measure the code coverage of your tests. Aim for 100% code coverage.

- **Functional Tests:**
  - Write functional tests that exercise the app as a whole. These tests should cover the same scenarios as the unit tests, but should do so by interacting with the app and/or the API.

- **Performance Tests:**
  - Write performance tests that measure the performance of the app under various conditions. These tests should help you identify bottlenecks and other performance issues.

## Tips

- Don't expect GitHub Copilot to produce perfect results, all of the time. It's a tool to help you write code, but it's up to you to decide what to do with its suggestions. Feel free to modify the code as you see fit.

- If you don't understand how to do something, or if you get stuck, don't hesitate to ask GitHub Copilot Chat for help. Think of it as a seasoned expert in the thing you're trying to do who can help guide you in the right direction. Don't be afraid to ask it questions! For example, if you don't know what kinds of diagrams Mermaid supports, you could ask "What kinds of diagrams does Mermaid support?" and it will give you a list of the supported diagram types.

- Use the `/help` command in GitHub Copilot Chat to get a list of commands you can use to interact with.

An example result of this exercise can be found in the [`copilot-exercise-unit-tests-completed`](https://github.com/bxtp4p/copilot-app-taskapp/tree/copilot-exercise-unit-tests-completed) branch of the [Task App](https://github.com/bxtp4p/copilot-app-taskapp). Note that this is just one possible solution as GitHub Copilot is generative and the results may be different on each iteration. Feel free to experiment and try different approaches. If you're still stuck, you can refer to the [completed](https://github.com/bxtp4p/copilot-exercise-unit-tests/tree/completed) branch in this repo for the prompts used to generate the results. Prompts will be located in the `/completed` directory.


