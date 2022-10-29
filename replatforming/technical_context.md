# Miniminal Technical Context

Our replatforming journey, and all pieces of it, are much more complex than can be described here, or even than any one person can fully understand, but a certain amount of technical context is needed to make any sense of the story. This section provides that minimum technical context, vastly simplifying the reality, but including enough of the complicating details to make the replatforming discussion interesting and useful.

My teams are frontend teams, so that will inform my perspective and focus. These frontend technical responsibilities primarily includes: React Javascript, CSS, GraphQL schema design, automated testing via React Testing Library (RTL) and Cypress, Buildkite for our build piplines (Continuous Integegration and Deployment, or CI/CD), and Kubernetes (k8s) for container management, deployment, and autoscaling.

## Tech Stack

We have an e-commerce website backed by a single (monolithic!) service, built on top of a PHP + SQL (relational database) stack. The e-commerce website has many separate pages including: home, landing for paid search, browse, product detail, cart, checkout, and receipt. When a user requests a page, the traffic flows to one of a fleet of generic PHP VMs, which then routes to a particular portion of code (the controller) that is attached to the specific page. This code loads data from SQL to serve the response, such as product description, price, and shipping time, and then  returns the webpage to the user.

## Server Side Rendering

An important complication is that many pages are server side rendered (SSR)--despite being built on the modern frontend Javascript framework React--meaning that HTML of the page is built with all of its details in the PHP server before being returned to the user. A more common flow for a React application would be client side rendering: when a user requests a page, the server returns the React code for the page to the user's browser, then the browser would parse and execute the Javascript, which would output the HTML of the page and display it. The reasons to SSR a page are to improve the customer experience and for search engine optimization.

The customer experience is improved by having a fast and stable page. Rendering on the server as part of the request will speed up getting fully rendered HTML displayed on the browser. SSR will increase page stability because the HTML can be created with the knowledge of certain attributes such as the size of a specific image, and so the HTML container can be built with that size in mind--this eliminates stability problems such as when a page is first rendered without knowledge of the height of an image, and then when the image is loaded and turns out to be taller than expected, the elements below that image will be suddenly shifted down.

Search Engine Optimization (SEO) in this context means that our pages must be indexed by Google to show up on search results, and this page indexing can only occur when the page is rendered on a server and returned to Google's search crawlers, as Google will not index client side rendered pages.

Note that every one of these reasons to SSR are disputed.

## Monolith Attachment Points

For a given website page, there are several separate main attachment points to the monolith:

1. Traffic routing - what percentage of external traffic is routed through a monolith virtual machine (VM) versus directly routing to a non-monolith service. 
2. Code repository - where the code lives in source control (git). The source code repo and deployment are somewhat coupled, but not completel. There are actually several PHP applications that live in the same git repository, but are independently deployed to separate services, e.g. an internally facing human resources website and the public facing e-commerce website.
3. Deployment - changes in the codebase are published to a fleet of monolith VMs that all run the same version of the code, for the e-commerce website. A service that is deployed independently of the monolith will have changes pushed to that service at times that are totally decoupled from the change publication times to the monolith.
4. Page Data - data to inform the overall page, including customer context such as customer id, language, and region.
5. Component Data - data for individual components on a page (e.g. shipping, pricing) can be loaded by making an HTTP request which is then serviced by PHP code in the monolith that loads data from SQL. 

## Pre-Replatforming Preparation: Webpack Migration of Frontend Code

Frontend code had been previously built, bundled, and deployed as part of the PHP monolith. As a prepatory step to replatforming, frontend pages have to be migrated to Webpack for bundling, split into separate code repositories from the monolith for each page, which then have their own independent build and deployment pipelines to a k8s SSR microservice.

At this stage, the specific page is coupled to the monolith at three of the above five attachment points: (1) external traffic for a page is routed to a PHP monolith VM and then the (4) initial page load data occurs in the monolith PHP controller. The page needs to be SSR-ed, so at that point the PHP calls to a separate k8s microservice for that specific page, which then runs the React code to render the HTML, fetches additional component data (5) from an arbitrary PHP monolith VM, and then returns the HTML to PHP and on to the user.

The SSR microservice is decoupled from the monolith because it has an independent (2) code repo and (3) deployment.
