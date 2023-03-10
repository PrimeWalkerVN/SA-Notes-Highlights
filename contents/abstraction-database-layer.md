<h1 align="center">Abstraction database layer - Middleware Adapter</h1>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#introduction">Introduction</a>
    </li>
    <li>
      <a href="#abstraction-layer">Abstraction Layer</a>
    </li>
    <li>
      <a href="#microservices">Microservices</a>
    </li>
    <li><a href="#clean-architecture">Clean architecture</a></li>
    <li><a href="#pros-and-cons">Pros and cons</a></li>
  </ol>
</details>
<br>

## Introduction
The decision to implement an adapter abstraction layer in your codebase can be a difficult one. On the one hand, an adapter abstraction layer can improve flexibility and maintainability by providing a consistent interface for interacting with different databases or data models. On the other hand, it can add additional complexity and potential sources of errors, and may impact performance.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Abstraction Layer

An adapter abstraction layer provides a consistent interface for interacting with different databases or data models. This can make it easier to switch between different data sources, or to add support for new databases or data models. However, implementing an adapter abstraction layer can add additional complexity and potential sources of errors. Depending on the implementation, it may also impact performance.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Microservices

Microservices can improve scalability and maintainability by breaking up large applications into smaller, independent services. When using an abstraction layer in a microservices architecture, it's important to carefully consider the boundaries between services. The abstraction layer should be designed to allow services to communicate efficiently and reliably. If services have different data needs, it may be necessary to create multiple adapters to support different data sources.An adapter abstraction layer provides a consistent interface for interacting with different databases or data models. This can make it easier to switch between different data sources, or to add support for new databases or data models. However, implementing an adapter abstraction layer can add additional complexity and potential sources of errors. Depending on the implementation, it may also impact performance.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Clean architecture

Clean architecture can improve maintainability by separating business logic from implementation details. When using an abstraction layer in a clean architecture, it's important to carefully consider the dependencies between different layers. The abstraction layer should be designed to allow data to flow smoothly between layers, while minimizing dependencies. This can help to ensure that changes to one layer do not have unintended consequences in other layers.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Pros and cons

### Pros

* Increased flexibility: An adapter layer can provide a unified interface for working with multiple databases and data models. This can increase the flexibility of your application, as you can easily switch between different databases or data models without having to modify your application code.

* Improved compatibility: By using an adapter layer, you can ensure that your application is compatible with multiple databases and data models. This can help you avoid vendor lock-in and ensure that your application can work with a wide range of data storage options.

* Easier migration: An adapter layer can make it easier to migrate your data between different databases or data models. Because the adapter layer provides a common interface, you can simply switch out the underlying database or data model without having to modify your application code.

* Simplified development: By providing a unified interface for working with different databases and data models, an adapter layer can simplify the development process. You can write application code that works with the adapter layer, rather than having to write custom code for each individual database or data model.

* Better maintainability: Because an adapter layer provides a single point of entry for interacting with the database, it can be easier to maintain and update over time. This can help reduce the overall maintenance overhead of your application.

* Improved performance: In some cases, an adapter layer can actually improve performance by providing a more efficient interface for working with the database. For example, an adapter layer may be able to optimize queries or minimize data transfer between the application and the database.

* Overall, an adapter abstraction layer can provide many benefits in terms of flexibility, compatibility, and ease of development. By abstracting away the underlying database or data model, you can simplify your application code and make it easier to work with a wide range of data storage options.

### Cons
While using an adapter abstraction layer can provide many benefits, there are also some potential downsides to consider:

* Complexity: Introducing an adapter layer between your application and the database can add complexity to your architecture. It requires additional code to be written and maintained, and can make debugging and testing more difficult.

* Performance: The additional abstraction layer can add a performance penalty, as it adds another layer of indirection between the application and the database. This can be especially problematic for high-performance applications that need to process large amounts of data quickly.

* Limited functionality: While an adapter layer can help you work with multiple databases and data models, it may not provide access to all the features and functionality of each database. This can limit your ability to take advantage of advanced features, and may require you to write custom code to work around these limitations.

* Learning curve: Adopting an adapter layer requires learning new concepts and tools, which can be time-consuming and difficult. It may also require changes to your existing codebase, which can be disruptive to ongoing development efforts.

* Vendor lock-in: Depending on the adapter layer you choose, you may become locked into a specific vendor or technology stack. This can limit your ability to switch to different databases or tools in the future, and may require significant rework to adapt to new technologies.
* Increased development time: Implementing an adapter layer can require significant development time, especially if you're working with multiple databases or data models. This can delay the release of your application and increase development costs.
  
* Maintenance overhead: Adding an adapter layer introduces an additional layer of code that needs to be maintained and updated over time. This can increase the overall maintenance overhead of your application, especially if you need to support multiple databases or data models.

Ultimately, the decision to use an adapter layer depends on your specific needs and requirements. While there are certainly potential downsides to consider, an adapter layer can also provide significant benefits in terms of flexibility, compatibility, and ease of development.Overall, while an adapter layer can provide many benefits in terms of flexibility and compatibility, it's important to carefully weigh the trade-offs and choose an approach that best meets your specific needs and constraints.
<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Implementation
We can use NestJS and TypeScript with support for both MySQL and MongoDB:

```
import { Injectable } from '@nestjs/common';
import { createConnection, Connection, Repository } from 'typeorm';
import { MongoClient } from 'mongodb';
import { ConfigService } from '@nestjs/config';

interface IDatabaseAdapter {
  connect(): Promise<void>;
  disconnect(): Promise<void>;
  getProductRepository(): Repository<Product>;
  getCategoryRepository(): Repository<Category>;
}

@Injectable()
export class DatabaseAdapter implements IDatabaseAdapter {
  private connection: Connection;
  private productRepository: Repository<Product>;
  private categoryRepository: Repository<Category>;

  constructor(private readonly configService: ConfigService) {}

  async connect(): Promise<void> {
    const dbType = this.configService.get<string>('DB_TYPE');
    const dbHost = this.configService.get<string>('DB_HOST');
    const dbPort = this.configService.get<number>('DB_PORT');
    const dbName = this.configService.get<string>('DB_NAME');

    if (dbType === 'mysql') {
      this.connection = await createConnection({
        type: 'mysql',
        host: dbHost,
        port: dbPort,
        username: this.configService.get<string>('DB_USERNAME'),
        password: this.configService.get<string>('DB_PASSWORD'),
        database: dbName,
        entities: [Product, Category],
      });

      this.productRepository = this.connection.getRepository(Product);
      this.categoryRepository = this.connection.getRepository(Category);
    } else if (dbType === 'mongo') {
      const mongoUrl = `mongodb://${dbHost}:${dbPort}/${dbName}`;

      const client = await MongoClient.connect(mongoUrl);
      const db = client.db(dbName);

      this.productRepository = db.collection<Product>('product');
      this.categoryRepository = db.collection<Category>('category');
    } else {
      throw new Error(`Unsupported database type: ${dbType}`);
    }
  }

  async disconnect(): Promise<void> {
    await this.connection.close();
  }

  getProductRepository(): Repository<Product> {
    return this.productRepository;
  }

  getCategoryRepository(): Repository<Category> {
    return this.categoryRepository;
  }
}

```
* In this example, we define the IDatabaseAdapter interface and implement it in the DatabaseAdapter class. The connect() method sets up the database connection based on the configuration values, and the disconnect() method closes the connection.

* We also define getProductRepository() and getCategoryRepository() methods that return the corresponding repository objects for each entity.

* This implementation uses TypeORM for MySQL and the MongoDB driver for MongoDB. It leverages the NestJS ConfigService to retrieve the configuration values for the database connection.

