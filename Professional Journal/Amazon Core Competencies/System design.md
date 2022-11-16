# [SOLID](https://www.baeldung.com/solid-principles)

1.  **S**ingle Responsibility
2.  **O**pen/Closed
3.  **L**iskov Substitution
4.  **I**nterface Segregation
5.  **D**ependency Inversion

# [Grasp](https://www.fluentcpp.com/2021/06/23/grasp-9-must-know-design-principles-for-code/)

GRASP stands for General Responsibility Assignment Software Principles.

#### 1. Information expert
#### 2. Creator
#### 3. Low coupling
#### 4. Protected variations
#### 5. Indirection
#### 6. Polymorphism
#### 7. High cohesion
#### 8. Pure fabrication
#### 9. Controller


# [Design Patterns](https://refactoring.guru/design-patterns/catalog)

## Creational

### Factory
### Builder
### Prototype

https://refactoring.guru/design-patterns/prototype
[Deep copy through serialization](https://americanairlines.udemy.com/course/design-patterns-java/learn/lecture/8556138#overview).

### Singleton

## Structural

### Adapter

## Behavioral

### [Chain of Responsibility](https://www.geeksforgeeks.org/chain-responsibility-design-pattern/)
### Command
### Iterator
### Observer

# Interview Video

![[Pasted image 20221026213338.png]]

[Amazon System Design Preparation (SIP)](https://www.youtube.com/watch?v=gNQ9-kgyHfo)

## Database

![[Pasted image 20221026164033.png]]

## API

![[Pasted image 20221026164257.png]]

## Architecture

![[Pasted image 20221026164457.png]]

## User Experience

![[Pasted image 20221026164729.png]]

# Clarifying questions:

- Who are you designing the system for and why?
- What expectations do they have in terms of functionality?
- What things would a customer just assume will be in the system but they may not think about in the forefront of their minds? (e.g. it'll be fast and secure)
- What happens if we become hyper-popular with customers? What does 2x growth look like? Or 10x? And how would that influence the design?
- Things that influence the design typically include: non-functional requirements (amount of load, load distribution, security), ways of interacting with the system (user access, scheduled processes, synchronous/asynchronous communication), and data flow.
- Explicitly mention all the assumptions that are being made.
- Spend majority of time on the Critical Requirements identified and on the core functionality.
- Justify the design choices that are made.
- Design the solution for scale, i.e. would more transactions seamlessly work.
- Research and read through best practices in the tech stack or infrastructure you would like to use. It helps to know how code and hardware can be maintained, scaled, and be made available.
-  What are the key business and technical metrics for the system? How will someone use that to identify problems?
- What are the potential bottlenecks or pain points?
- What failure points exist?
- What redundancy can we build in to reduce single points of failure?
- How does someone get logs and debug the system?
- If there are multiple ways to design a system that satisfy the requirements, describe the trade-offs of both approaches, choose one and explain why. Having a conversation with the interviewer and explaining your thought process, might help you to make a choice.
- We are looking for a high level understanding of how to design a system and deep knowledge into at least one area. If the interviewer asks you to dive deep into something you are unfamiliar with, tell them and suggest some other area you are familiar with and dive into that.
-  Domain Driven Design
-  On system design, we are talking more about different components and how those interact between each other on a high level on a product that we are building

# [N-Tier Architecture](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/n-tier)

[N-Tier Architecture](https://www.baeldung.com/cs/n-tier-architecture)

![](https://www.baeldung.com/wp-content/uploads/sites/4/2022/01/ntier_usecase.drawio.svg)

# Data Design

![](https://raw.githubusercontent.com/ethanmiller1/Node.js-and-Express-App/b543f75eff6b578af12aac39099e28731ffc149e/public/images/application/data-design.svg)
* User table
* Order Table
* Product Table
* ORM like Hibernate

# [Microservice Oriented Architecture](https://microservices.io/patterns/microservices.html)

[Are Microservices … for real?](https://www.codingblocks.net/podcast/are-microservices-for-real/)

### How to Know When to Choose the Microservice Architecture

This is actually a hard problem.

-   Choosing this path can slow down development
-   However, if you need to scale in the future, splitting apart / decomposing a monolith may be very difficult

-   Who uses Microservices?
    -   Netlifx
    -   Uber
    -   Amazon
    -   Lots of other big companies
-   Who has abandoned Microservices?
    -   Lots of small companies…seeing a pattern here?

### Pros of the Microservice Architecture

-   **Improved fault isolation**
	- example they gave is a memory leak won’t impact ALL parts of the system like in a monolithic design
-   Each service is small so it’s easier to understand and change
-   **Highly maintainable and testable** 
	- enables rapid and frequent development and deployment
-   **Loosely coupled with other services**
	- enables a team to work independently the majority of time on their service(s) without being impacted by changes to other services and without affecting other services
-   **Independently deployable**
	- enables a team to deploy their service without having to coordinate with other teams
-   **Capable of being developed by a small team**
	- essential for high productivity by avoiding the high communication head of large teams
-   Easier / faster to test as they’re smaller and less complex
-   Better deployability 
	- able to deploy each service independently of the others
-   Easier to organize development effort around smaller, autonomous teams
-   Because the code bases are smaller, the IDEs are actually better to work in
-   Applications start and run faster when they are smaller
-   Allows you to be more flexible with tech stacks 
	- you can change out small pieces rather than entire systems if necessary

### Cons of the Microservice Approach

-   Additional complexity of a distributed system
    -   Distributed debugging is hard! Requires additional tooling
    -   Additional cost (overhead of services, network traffic)
    -   Multi-system transactions are really hard
-   Implementing inter-service communication and handling of failures
-   Implementing multi-service requests is more complex
    -   Not only more complex, but you may be interfacing with multiple developer teams as well
-   Testing interactions between services is more complex
-   IDEs don’t really make distributed application development easier – more geared towards monolithic apps
-   Deployments are more complex – managing multiple services, dependencies, etc.
-   **Increased infrastructure requirements** – CPU, memory, etc.
-   Distributed debugging is hard! Requires additional tooling

# [The Twelve-Factor App](https://12factor.net/)

1. Codebase
	One codebase tracked in revision control, many deploys
2. Dependencies
	Explicitly declare and isolate dependencies
3. Config
	Store config in the environment
4. Backing services
	Treat backing services as attached resources
5. Build, release, run
	Strictly separate build and run stages
6. Processes
	Execute the app as one or more stateless processes
7. Port binding
	Export services via port binding
8. Concurrency
	Scale out via the process model
9. Disposability
	Maximize robustness with fast startup and graceful shutdown
10. Dev/prod parity
	Keep development, staging, and production as similar as possible
11. Logs
	Treat logs as event streams
12. Admin processes
	Run admin/management tasks as one-off processes

Java processes accomplish concurrency with the JVM providing one massive uberprocess that reserves a large block of system resources (CPU and memory) on startup, with concurrency managed internally via threads.