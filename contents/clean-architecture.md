<h1 align="center"> Clean Architecture </h1>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#clean-architecture">Clean Architecture</a>
    </li>
    <li>
      <a href="#benefits">Benefits</a>
    </li>
    <li>
      <a href="#layers">Layers</a>
    </li>
    <li>
      <a href="#interfaces-and-dependency-injection">Interfaces and Dependency Injection</a>
    </li>
    <li>
      <a href="#separation-of-concerns">Separation of Concerns</a>
    </li>
    <li>
      <a href="#testability-and-scalability">Testability and Scalability</a>
    </li>
    <li>
      <a href="#example-clean-architecture-implementation-in-nestjs">Example Clean Architecture implementation in NestJS</a>
    </li>
    <li>
      <a href="#entities">Entities</a>
    </li>
    <li>
      <a href="#use-cases">Use Cases</a>
    </li>
    <li>
      <a href="#repositories">Repositories</a>
    </li>
    <li>
      <a href="#modules">Modules</a>
    </li>
  </ol>
</details>
<br>
Clean Architecture is a software architecture pattern that provides a way to organize code into separate, independent, and interchangeable modules. The pattern is designed to make code easier to maintain, test, and extend over time.

## Benefits

- Decoupled code: Clean Architecture helps to decouple the different parts of an application, meaning changes in one part of the application do not affect other parts.
- Easier testing: Clean Architecture makes testing easier by allowing business logic to be tested independently of infrastructure.
- Reusable code: Clean Architecture makes it easier to reuse code by separating the different parts of an application into modules.

## Layers

Clean Architecture typically consists of three or more layers, including entities, use cases, repositories, infrastructure, and presentation. Each layer is responsible for a specific concern and minimizes cross-layer dependencies.

## Interfaces and Dependency Injection

Clean Architecture makes use of interfaces to define contracts between different parts of the application, and uses Dependency Injection to manage dependencies between these parts.

## Separation of Concerns

Clean Architecture promotes the separation of concerns, with each part of an application responsible for a specific concern. This makes code easier to maintain and extend over time.

## Testability and Scalability

Clean Architecture makes it easier to write automated tests and scale an application as it grows. By dividing code into separate and independent modules, it is easier to add new features and functionality without having to make major changes to the existing codebase.

In conclusion, Clean Architecture is a powerful tool for organizing and structuring code to make it easier to maintain, test, and extend over time. It requires discipline and experience to implement effectively, but can lead to code that is more robust, scalable, and easier to maintain in the long term.

# Example Clean Architecture implementation in NestJS

## Entities
```
export class Task {
  id: number;
  title: string;
  description: string;
  status: TaskStatus;
}

export enum TaskStatus {
  OPEN = 'OPEN',
  IN_PROGRESS = 'IN_PROGRESS',
  DONE = 'DONE',
}

```

## Use Cases

```
export class CreateTaskUseCase {
  constructor(private tasksRepository: TasksRepository) {}

  async execute(title: string, description: string): Promise<Task> {
    const task = new Task();
    task.title = title;
    task.description = description;
    task.status = TaskStatus.OPEN;
    return await this.tasksRepository.save(task);
  }
}
```

## Repositories
```
export interface TasksRepository {
  save(task: Task): Promise<Task>;
}

export class InMemoryTasksRepository implements TasksRepository {
  private tasks: Task[] = [];

  async save(task: Task): Promise<Task> {
    this.tasks.push(task);
    return task;
  }
}
```

## Modules

```
@Module({
  imports: [],
  controllers: [TasksController],
  providers: [
    { provide: TasksRepository, useClass: InMemoryTasksRepository },
    CreateTaskUseCase,
  ],
})
export class TasksModule {}

```