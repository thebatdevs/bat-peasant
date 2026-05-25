# New feature workflow

To implement a new feature, follow this workflow:

1. Check for `/.ai/context` folder for related context files to understand the current business logic and code structure.
2. Create a implementation plan that outlines a clear path for how to implement the new feature containing the following sections:
   - **Objective**: A clear statement of what the new feature should accomplish.
   - **Implementation Steps**: A step-by-step plan for how to implement the feature, including any necessary refactoring or new components.
   - **Schema creation**: If the new feature requires new data models, create schema files in `/.ai/note` to define these models.
   - **Testing Plan**: A plan for how to test the new feature, including unit tests, integration tests, and any necessary test data.
3. Take user's manual approval on the implementation plan before proceeding.
4. Create a algorithmic implementation of the new feature based on the approved plan containing:
   - Workflow implementation functions by function
   - Schema implementation if needed
5. Ask for user's approval on the implementation before proceeding to testing.
6. Create tests plan and implement tests for the new feature.
7. Ask for user's approval on the tests before proceeding to documentation.
8. Implement the new feature with following standards in mind.
   - For understand project standards, refer to `/.ai/standards/code-writing.standard.md`.
9. Test the new feature and make sure all tests are passing.
10. Create a documentation for the new feature in `/.ai/context` with `/.ai/standards/context-documentation.standard.md` in mind.
