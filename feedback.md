
# Feedback

Hey team, I went through the codebase and wanted to share some thoughts. Overall, it looks well-structured, but I’ve noted a few areas where we can improve. Here’s my feedback!
Apologies if anything doesn’t align with the team's principles—happy to discuss and learn more!

## Documentation

I noticed that the [prerequisites](https://github.com/lightwork-blue/lightwork-microservice/blob/main/README.md#prerequisites) mention Node.js and npm/yarn/pnpm for running Turborepo commands and managing dependencies. However, the project is actually using Nx. It might be helpful to update this to avoid confusion for new developers setting up the project.

## Env config

I noticed that there are no `.env.example` files within each microservice. Adding these would help new developers quickly understand the required environment variables and avoid configuration issues. If needed, I’d be happy to help create them based on the existing `.env` files.

## Tests coverage

I noticed that there are no unit tests written in the project.  It might be helpful to establish a testing strategy. While we can introduce end-to-end (E2E) testing later, starting with unit tests will encourage writing testable code and improve overall reliability.

## Inconsistent Naming

I noticed that file naming is inconsistent— some files use kebab-case while others use camel case.
![inconsistent_naming](<Screenshot 2025-03-24 104712.png>)
This might create confusion to devs who are contributing to the project first time. Maybe mentioning a particular case which we should adhere might help. We can gradually update our naming conventions later.

## Unused Imports

I noticed that the codebase has a lot of unused imports. Removing them would improve readability and reduce potential confusion. We could also consider adding a linter rule (e.g., ESLint’s no-unused-vars rule) or a pre-commit hook to automatically clean up unused imports.

## Logger Service

Each microservice has its own copy of CustomLoggerService, leading to duplication. If any updates are needed, each service must be updated separately, increasing maintenance overhead.

Instead of defining it in every microservice, we can create a shared package and import it wherever needed.This would also make it easier to enhance logging in the future, such as integrating structured logging with Winston or Pino.

## Repository Pattern

Business logic and database logic are mixed in service files, making it harder to maintain and test. If the team ever needs to switch from one ORM to another, refactoring will be painful.Since service files directly interact with the database, mocking database calls becomes more complex. Using repository pattern will make it easier to switch database providers in the future and keep services focused on business logic. [Here](https://docs.nestjs.com/techniques/database#repository-pattern) is a helpful resource on this.

## Error handling

I noticed that some services define a CustomHttpException class named as `custom_error_class`, but it’s not used consistently across the project. To ensure a standardized approach to error handling, we could centralize this class in a shared module and use it across all services. This would also make it easier to extend in the future if we need structured error responses or integration with monitoring tools.

## Comments

Why a particular code is commented might be helpful to know. This will avoid us of any confusion whether to keep the commented code or uncomment it
![comments](image-1.png)

## Usage of `any`

There are many places in the codebases where request body type is defined as `any` overriding the type safety. It would be helpful to define a proper dto for request body and validating and reduce any runtime errors
