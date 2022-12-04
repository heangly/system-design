# Solutions

### 1. Gathering System Requirements

As with any system design interview question, the first thing that we want to do is to gather system requirments; we need to figure out what system we're building exactly.

From the answers we were given to our clarifying questins. We're building the core AlgoExpert user flow, which includes users landing on the website, accessing questions, marking them as complete, writing code, running code, and having their code saved.

We don't need to worry about payments or authentication, and we don't need to go too deep into the code-execution engine.

We'are building this platform for a global audience, with an emphasis on U.S. and India users, and we don't need to overly optimize our system's availability. We probably dontt need more than two or three nines, because we're not building a health or security system, and this gets use somewhere between **8 hours and 3 days of downtime per year**, which is reasonable. All in all, this means that we don't need to worry too much about availability.

We care about latency and throughput within reason, but apart from the code-execution engine, this doesn't seem like a particulary difficult aspect of our system.

<br/>

### 2. Comming Up With A Plan

It's important to organize ourselves and to lay out a clear plan regarding how we're going to tackle our design. What are the major, distinguishable components for our system?

On the one hand, AlgoExpert has a lot of static content; the entire homepage, for instance, is static, and it has a lot of images. On the other hand, AlgoExpert isn't just a static website; it clearly has a lot of dynamic content that users themselves can generate(code that they can write, for example). So we'll need to have a robust API backing our UI, and given that user content gets saved on the website, we'll also need a database backing our API.

We can divide our system into 3 core components:

- Static UI content
- Accessing and interacting with questions (question completion status, saving solutions, etc.)
- Ability to run code

Note that the second bullet point will likey get further divided.

<br>
 
### 3. Static UI content

For the UI static content, we can put public assets like images and JavaScript bundles in a blob store: **S3 or Google Cloud Storage**. Since we're catering to a global audience and we care about having a responsive website (especially the home page of the website), we might want to use a **Content Delivery Network** (CDN) to server that content. This is especially important for a better mobile experience because of the slow connections that phones use.
