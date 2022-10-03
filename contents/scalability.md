<h1 align="center">Scalability</h1>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#approach">Approach Vertical & Horizontal Scaling</a>
    </li>
    <li>
      <a href="#concepts">Concepts</a>
    </li>
    <li>
      <a href="#at-the-level">At the level</a>
    </li>
    <li><a href="#tunning">Tunning</a></li>
    <li><a href="#testing-the-scalability">Testing the scalability</a></li>
  </ol>
</details>
<br>


## Approach Vertical & Horizontal Scaling

1. Vertical do not touch the code, and simpler for configuration, less monitoring, administrative, management effort
2. Horizontal is not availability risk, higher available which opposed vertical when major downside as servers going down.
<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Concepts
1. Use a persistent memory like a key-value store to hold the data and to
intercept the state/static variable from the class &rarr; This is why functional
programming got so popular with distributed systems. The functions don’t
retain any state. 
2. Make no limit for hardware capacity. Data is replicated across different geographical regions as nodes & data centres which are set up across the globe &rarr; Build and deploy it on the cloud and have **horizontal scalability in mind** right from the start

3. Make wise use of database partitioning, sharding and use multiple database server to make sure the module efficient
4. A common architectural mistakes is not using asynchronous processes and modules where ever required rather all processes are scheduled sequentially
5. Using caching exhaustively throughout the application to speed up things significantly
6. Using too many or too few load balancers will impact the latency of our application
7. Please do not add business logic to the database

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## At the level
_Top frequently mistakes:_
1. Using unnecessary loops, nested loops
2. Writing tightly coupled code
3. Not paying attention to the big O complexity while during writing the code &rarr; production issues :))

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Tunning
1. Profiling
2. Caching
3. CDN
4. Data Compression
5. Avoid unnecessary RPS and QPS
<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Testing the scalability

1. CPU usage
2. Network bandwidth consumption
3. Throughput
4. The number of requests processed within stipulate time
5. Latency
6. memory usage of the program
7. End user experience when system under heavy load

<p align="right">(<a href="#readme-top">back to top</a>)</p>