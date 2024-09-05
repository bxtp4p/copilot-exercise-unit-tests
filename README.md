# Adding Unit Tests

This exercise focuses on writing unit tests for the Task App. You will use GitHub Copilot to help generate the necessary test cases, ensuring the application behaves as expected and that future changes do not introduce regressions.

## Overview

In this exercise, you'll use GitHub Copilot to create a suite of unit tests for the [Task App](https://github.com/bxtp4p/copilot-app-taskapp). These tests will verify the functionality of key features like task creation, modification, completion, and deletion. The goal is to ensure the app behaves correctly in different scenarios while isolating the code under test from external dependencies using mocks and stubs.

## Learning Objectives

By the end of this exercise, you will be able to:
1. **Write Comprehensive Unit Tests**: Generate unit tests that cover key scenarios for task creation, modification, completion, and deletion.
2. **Isolate Code with Mocks and Stubs**: Use mocks and stubs to ensure your tests focus on the behavior of the code being tested.
3. **Refactor for Testability**: Identify and refactor areas of the application code to make it more testable.
4. **Measure Code Coverage**: Use a coverage tool to evaluate how well your tests cover the application code.
5. **Experiment with Functional and Performance Testing**: Extend the exercise to include functional and performance tests for a broader evaluation of the app.

## Setup

To begin, clone the [Task App repository](https://github.com/bxtp4p/copilot-app-taskapp) and follow the instructions in the README to set up the project locally.

## Problem Statement

The Task App currently lacks unit tests, making it difficult to ensure the application behaves as expected and preventing future changes from being confidently tested. Writing a robust set of unit tests will improve confidence in the application’s reliability and reduce the risk of regressions.

## Task

Use GitHub Copilot to generate unit tests that cover the following scenarios:

- **Task Creation**:
  - Test that tasks can be created with a valid title and description.
  - Ensure that tasks cannot be created without a title or with an excessively long title or description.

- **Task Completion**:
  - Verify that tasks can be marked as complete or incomplete.

- **Task Retrieval**:
  - Ensure that tasks can be retrieved by their ID, and test for error handling when the task ID does not exist.

- **Task Modification**:
  - Test that existing tasks can be successfully modified.

- **Task Deletion**:
  - Verify that tasks can be deleted from the task list.

- **Task Listing**:
  - Test that a list of tasks can be retrieved.

Additionally, refactor the application code where necessary to improve its testability and to ensure all tests pass.

## Expected Outcome

Upon completion, you should have a comprehensive suite of unit tests that:
- Validate the behavior of key features of the Task App.
- Run independently and in isolation from external services using mocks and stubs.
- Ensure full test coverage for critical areas of the application.
- Include any necessary refactoring to make the code more testable.

### Bonus

- **Code Coverage**: Measure the coverage of your tests using a tool like `coverage.py` and aim for high coverage (ideally 100%).
- **Functional and Performance Testing**: Expand the exercise by writing functional tests that interact with the app as a whole, and performance tests to evaluate the app’s responsiveness under load.

## Tips

- **Iterate with Copilot**: GitHub Copilot can help generate unit test cases, but its output may need refinement. Don’t hesitate to adjust the code for accuracy and completeness.
- **Use Copilot Chat**: If you encounter any issues or need clarification on writing tests, GitHub Copilot Chat can guide you. For example, if you're unsure about how to mock an external service, you can ask, *"How do I use mocks in Python’s unittest framework?"*
- **Leverage Test Frameworks**: Use testing frameworks like `unittest` or `pytest` to structure your tests and take advantage of utilities like setup/teardown functions.
- **Explore Code Coverage**: Use tools like `coverage.py` to measure your test coverage and identify untested areas of your code.

## Example Solution

An example solution can be found in the [`copilot-exercise-unit-tests-completed`](https://github.com/bxtp4p/copilot-app-taskapp/tree/copilot-exercise-unit-tests-completed) branch of the [Task App repository](https://github.com/bxtp4p/copilot-app-taskapp). Keep in mind that GitHub Copilot’s suggestions may vary on each iteration, so feel free to experiment with different approaches. If you're stuck, refer to the [completed](https://github.com/bxtp4p/copilot-exercise-unit-tests/tree/completed) branch for the prompts and test cases used in the solution, which are located in the `/completed` directory.
