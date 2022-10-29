# Technical Context of Our E-Commerce Website

## Tech Stack

Greatly simplified, we have an e-commerce website backed by a single (monolithic!) service, built on top of a PHP + SQL (relational database) stack. The e-commerce website has many separate pages including: home, landing for paid search, browse, product detail, cart, checkout, and receipt. When a user requests a page, the traffic flows to one of a fleet of generic PHP VMs, which then routes to a particular portion of code (the controller) that is attached to the specific page. This code loads data from SQL to serve the response, such as product description, price, and shipping time, and then  returns the webpage to the user.

## Server Side Rendering

An important complication is that many pages are server side rendered (SSR)--despite being built on the modern frontend Javascript framework React--meaning that HTML of the page is built with all of its details in the PHP server before being returned to the user. A more common flow for a React application would be client side rendering: when a user requests a page, the server returns the React code for the page to the user's browser, then the browser would parse and execute the Javascript, which would output the HTML of the page and display it. The reasons to SSR a page are to improve the customer experience and for search engine optimization.

The customer experience is improved by having a fast and stable page. Rendering on the server as part of the request will speed up getting fully rendered HTML displayed on the browser. SSR will increase page stability because the HTML can be created with the knowledge of certain attributes such as the size of a specific image, and so the HTML container can be built with that size in mind--this eliminates stability problems such as when a page is first rendered without knowledge of the height of an image, and then when the image is loaded and turns out to be taller than expected, the elements below that image will be suddenly shifted down.

Search Engine Optimization (SEO) in this context means that our pages must be indexed by Google to show up on search results, and this page indexing can only occur when the page is rendered on a server and returned to Google's search crawlers, as Google will not index client side rendered pages.

Note that as with anything technical, all of these reasons to SSR are disputed.

## Monolith Attachment Points

For a given website page, there are three separate main  attachment points to the monolith:

1. Traffic routing - what percentage of external traffic is routed through a monolith virtual machine (VM) versus directly routing to a non-monolith service.
2. Deployment - changes in the codebase are published to a fleet of monolith VMs that all run the same version of the code. A service that is deployed independently of the monolith will have changes pushed to that service at times that are totally decoupled from the change publication times to the monolith.
3. Data - data to inform the overall page (such as customer information, language, region) and for individual components on a page (shipping, pricing) can be loaded by making an HTTP request which is then serviced by PHP code in the monolith that loads data from SQL. 
