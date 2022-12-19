<h1 align="center">Scalability</h1>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#monolithic-architecture">Monolithic architecture</a>
    </li>
    <li>
      <a href="#microservices-architecture">Microservices architecture</a>
    </li>
        <li>
      <a href="#approaches">Approaches</a>
    </li>
  </ol>
</details>
<br>


## Monolithic architecture

### Pros
    Simplicity
### Cons
    1. Continuous Deployment
    2. Regression Testing
    3. Single points of failure
    4. Scalability Issues (Have to scale entire the system)
    5. Cannot Leverage Heterogeneous Technologies (Cuz use one single code base)
    6. Stateful (Not cloud ready and hold state) 

*When should you pick a monolithic architecture*
- Expected a limited amount of traffic. Etc: Tax app or similar open public tool
- Not use for the exponential growth, traffic over time and based on user 
- Start with a monolithic after that scale out into a distributed microservices


## Microservices architecture
### Pros
    1. No single points of Failure
    2. Leverage the Heterogeneous technologies ( can apply several, multiform and various tech stack or polygon persistence)
    3. Independent and Continuos Deployments
    4. A loosely coupled architecture
### Cons
    1. Complexities in management => Need to certain resources and dedicated team for maintaining every service because separation services have different the business logic and the operation, even the implement of them.
    2. Need to setup additional components to manage microservices such as a node manager like zookeeper, a distributed tracing service for monitoring the nodes
    3. No strong consistency (reference transaction in microservice)

*When should you pick a monolithic architecture*
- For complex cases and for apps which expect traffic to increase exponentially in the future.
- Each component keeping the Single Responsibility and Separation of Concerns principle 
- Writing every feature in a single code base would take no time in becoming a messy
  
## Approaches
When started we can pick: 
  * Picking the monolithic or microservice follow your use-case
  * Starting with monolithic after that scale out into the microservice => So we need to keep it evolving the code interactively for easy moving the codebase when understand requirement thoroughly

<p align="right">(<a href="#readme-top">back to top</a>)</p>
