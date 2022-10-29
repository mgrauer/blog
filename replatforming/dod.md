Technical Context of Our E-Commerce Website

Greatly simplified, we have an e-commerce website backed by a single (monolithic!) service, built on top of a PHP + SQL (relational database) stack. The e-commerce website has many separate pages including: home, landing for paid search, browse, product detail, cart, checkout, and receipt. When a user requests a page, the traffic flows to one of a fleet of generic PHP VMs, which then routes to a particular portion of code (the controller) that is attached to the specific page. This code loads data from SQL to serve the response, such as product description, price, and shipping time, and then  returns the webpage to the user.

An important complication is that many pages are server side rendered (SSR)--despite being built on the modern frontend Javascript framework React--meaning that HTML of the page is built with all of its details in the PHP server before being returned to the user. A more common flow for a React application would be for a user to request a page, the server would return the React code for the page to the user's browser, then the browser would parse and execute the Javascript, which would output the HTML of the page and display it. The reasons to SSR a page are to improve the customer experience and for search engine optimization. The customer experience is improved by having a fast and stable page. Rendering on the server as part of the request will speed up getting fully rendered HTML displayed on the browser, and will increase page stability because the HTML can be created with the knowledge of certain attributes such as the size of a specific image, and 

--such as height of a given image--in the PHP server before being returned to the user.




What is the definition of done for replatforming? And what is the goal? In the ideal case, these would be aligned.

The goal is to get off the monolith. Simple enough. So lets start with a definition of done that measures how much off the monolith we are.

 



There are three main monolith attachments: traffic, deployment, and data. Traffic routing is what percentage of external traffic is routed through a monolith VM versus directly routing to a non-monolith service.

and how many components don't get their data from the monolith, but even some components that don't get their data from the monolith proxy back to the monolith. and then some components call the monolith directly, even though they aren't deployed with the monolith. how much are you not deployed with the monolith. even if you take the 5% traffic, is that 100% off the monolith? and the categories, you can also control it via url, so some brands may be off but not all brands, and you can match at the level of a regex, so some instances of aparticular domian but not all. e.g. icp, category pages.


, but that is only one of the measures, and even that isn't necessarily straightforward to answer. how much are you off the monolith? there is traffic routing, and how many components don't get their data from the monolith, but even some components that don't get their data from the monolith proxy back to the monolith. and then some components call the monolith directly, even though they aren't deployed with the monolith. how much are you not deployed with the monolith. even if you take the 5% traffic, is that 100% off the monolith? and the categories, you can also control it via url, so some brands may be off but not all brands, and you can match at the level of a regex, so some instances of aparticular domian but not all. e.g. icp, category pages.

and it goes the other way too, it's not just about how much you are still on the monolith, if you try to restrict yourself from getting off the monolith because of some particular scaling issue or other, and you don't want to take on more traffic, then even if you restrict to some percentage there may be back doors. so if the frontier between off the monolith an dno tisn't so clear, the fronteir of control for how preciesly you aren't on the monolith isn't as clear either, and so you may end up having more traffic than you can take or not be able to set the boundaries as precisely as you want.



The goal is to get off the monolith. Simple enough. But it isn't enough to leave, we must arrive somewhere, and from the hopes and dreams stamped upon this vision of a new world spring complexity. We must create a platform that embodies quality, that enables flexibility, and is built on the shiny and new.

The goal is also to decouple, not only ourselves from the monolith, but also ourselves from ourselves. Our monolith is tightly coupled, and so is our org and our business. Decoupling will allow us to regain velocity, to innovate, and to learn, all while limiting how much we must keep track of. While our software has no limits on complexity--on the accumulation of layers of busisness and technical detritrus, at first light, then slowly filtering to be bottom as sediment, and pressed upon heavily by the layers above it, until hardpacked, dense, and inert--our fragile minds have limits, and can only be effective with the small and simple. 




what is the goal? to get off the monolith. and then there all of these other goals alongside it. own quality. reusability. leverage of engineering teams by non engineers. separation of configurability from code. differentiation. have more specific dev and ops and control over dependencies and the customer experience. with so many different variations that does cause inconsistencies for your customers. so does the use of dark patterns to drive to drive customers down funnel. and so do friction down the funnel.

and then what is the actual definition of done of replatforming? in one sense it is very technical. when you are on certain set of technology substrates that we consider to be our platform of the future. all of your data from federated graphql instead of monolith. direct external routing instead of routing in through monolith vms and then internally routed to ssr render farms. other things are more squishy: how much of a set of reusable components are you using, or extending? how much are you adhering to rfcs that call out certain patterns? 

and along the way there are many choices about how to tradeoff speed for feature parity, and how do you even decide on feature parity? many dimensions there.

typical of migrations, you want to get there as soon as you can, but also it is impossible to remain the same by defintion, so which are the dimensions you try to restrict? and because you have a goal in the first place, there was some limiting condition in the old world that you wanted to throw off.

how much do you allow others to program your page via a cms and configuration and server driven UI? which are the thigns you allow to be configured? things that used to be hard are now easy, so almost immediately you are heading off in different directions than you were before. so why bother to get to the exact same place as quickly as possible if you are just going to diverge anyway?

how much do you go greenfield vs incremental? what is the level of granularity for making change, and for building change without testing it in production? because sometimes to test it in production would be to incur operational or developmental costs. and how do you even test? it's so big and so many axes of change you couldn't A/B test it if you ever wanted to get there.

the biggest concern is how quickly you can replatform, but that means you should be primarily focused on accelation. identifying bottlenecks and overcoming them. reshaping hills as you go along. for every process, how quickly can you iterate and speed it up? what are the things you need to learn?

replatforming a bunch of services. we want to start with the one that takes the longest so that it can finish sooner, it is a long pole. but if you start with that one you can't push a marble all the way through, so also pick one that can finish the soonest.

how can you incentivize your partners to help you on the paths? you need to find compromise projects so that they can advance their goals while you advance yours. really this is the place where leadership needs to come in.

product needs to understand the value so it can explain it, advocate for it, and push back when things threaten it. engineering needs to take the straight and narrow route and not get cuaght in shiny object syndrome. which by definition this is a minefield for. even to have gotten this far, with an actual replatforming plan, you already had to sort through a maze of technology choices, and then along the way, there are so many other choices to be made as you rehash arguments, discover more. and some of those decisions were 49/51 anyway, so which do you retract vs just try to get done sooner? and is momentum the right arugment to defend a course when you reliaize it is problematic.

so you want to accellerate, you need to thinka bout all of the things you are trying to do and which of those is critical to the goal and which can be sacraficied to go faster to the goal. 

replaforming is also the golden chance to rewrite. to hold onto the door jamb and say you are not pulling me through in this form. if i am going to migrate with all of my baggage, at least i want a chance to take things to the curb and decide which of these things i actually wanted.





---


The goal is to get off the monolith, but any goal of sufficient gravity and resource allotment will attract organizational barnacles. I can't say what these look like outside the single replatforming I've been a part of. Or is that true? I've been part of midas to girder, girder to girder 4 replatforming, replatforming from midas into retrograde, replatforming from girder into stumpf. With those projects, the replatforming end was generally driven by solving specific problems in the case of retrograde and stumpf. in the case midas to girder and girder to girder 4, what was driving it? Those were both changes in tech stacks, driven by irritation with the current platform. did they give a clearer mission? did they make solving business problems easier? did they give a more enjoyable developer experience? did they allow for developer experssivenesess?

for our replatforming, i had thought it was about differentiation. but rather than differentiation it was about customization and quality. but what does it mean to customize? and who has an easier time in customizing? since the system has an evolutionary architecture, it's optimized to do different things, and there's a tradeoff. 

what does getting everyone off the monolith as soon as possible give you? it gives you highly localized control, splitering you. but then that is too much divergence and so people want to bring it back together into a monorepo, because some things have become too differtiated. so many ironies! the ops and dev experience and dependencies have been differentiated, as has the developer experience, the patterns. but the ability to differentiate hasn't been given in any of these things. 

how is the replatforming program itself coupled? try to have it in a gantt chart, which means the who organization can agree it will have certain syncrhonization points. and what can we expect at those synchronization points, we can define them with a time point but what else? certain expectations in terms of the definition of done.

you would think it would be how much off the monolith are you, but that is only one of the measures, and even that isn't necessarily straightforward to answer. how much are you off the monolith? there is traffic routing, and how many components don't get their data from the monolith, but even some components that don't get their data from the monolith proxy back to the monolith. and then some components call the monolith directly, even though they aren't deployed with the monolith. how much are you not deployed with the monolith. even if you take the 5% traffic, is that 100% off the monolith? and the categories, you can also control it via url, so some brands may be off but not all brands, and you can match at the level of a regex, so some instances of aparticular domian but not all. e.g. icp, category pages.

and it goes the other way too, it's not just about how much you are still on the monolith, if you try to restrict yourself from getting off the monolith because of some particular scaling issue or other, and you don't want to take on more traffic, then even if you restrict to some percentage there may be back doors. so if the frontier between off the monolith an dno tisn't so clear, the fronteir of control for how preciesly you aren't on the monolith isn't as clear either, and so you may end up having more traffic than you can take or not be able to set the boundaries as precisely as you want.

what is the goal? to get off the monolith. and then there all of these other goals alongside it. own quality. reusability. leverage of engineering teams by non engineers. separation of configurability from code. differentiation. have more specific dev and ops and control over dependencies and the customer experience. with so many different variations that does cause inconsistencies for your customers. so does the use of dark patterns to drive to drive customers down funnel. and so do friction down the funnel.

and then what is the actual definition of done of replatforming? in one sense it is very technical. when you are on certain set of technology substrates that we consider to be our platform of the future. all of your data from federated graphql instead of monolith. direct external routing instead of routing in through monolith vms and then internally routed to ssr render farms. other things are more squishy: how much of a set of reusable components are you using, or extending? how much are you adhering to rfcs that call out certain patterns? 

and along the way there are many choices about how to tradeoff speed for feature parity, and how do you even decide on feature parity? many dimensions there.

typical of migrations, you want to get there as soon as you can, but also it is impossible to remain the same by defintion, so which are the dimensions you try to restrict? and because you have a goal in the first place, there was some limiting condition in the old world that you wanted to throw off.

how much do you allow others to program your page via a cms and configuration and server driven UI? which are the thigns you allow to be configured? things that used to be hard are now easy, so almost immediately you are heading off in different directions than you were before. so why bother to get to the exact same place as quickly as possible if you are just going to diverge anyway?

how much do you go greenfield vs incremental? what is the level of granularity for making change, and for building change without testing it in production? because sometimes to test it in production would be to incur operational or developmental costs. and how do you even test? it's so big and so many axes of change you couldn't A/B test it if you ever wanted to get there.

the biggest concern is how quickly you can replatform, but that means you should be primarily focused on accelation. identifying bottlenecks and overcoming them. reshaping hills as you go along. for every process, how quickly can you iterate and speed it up? what are the things you need to learn?

replatforming a bunch of services. we want to start with the one that takes the longest so that it can finish sooner, it is a long pole. but if you start with that one you can't push a marble all the way through, so also pick one that can finish the soonest.

how can you incentivize your partners to help you on the paths? you need to find compromise projects so that they can advance their goals while you advance yours. really this is the place where leadership needs to come in.

product needs to understand the value so it can explain it, advocate for it, and push back when things threaten it. engineering needs to take the straight and narrow route and not get cuaght in shiny object syndrome. which by definition this is a minefield for. even to have gotten this far, with an actual replatforming plan, you already had to sort through a maze of technology choices, and then along the way, there are so many other choices to be made as you rehash arguments, discover more. and some of those decisions were 49/51 anyway, so which do you retract vs just try to get done sooner? and is momentum the right arugment to defend a course when you reliaize it is problematic.

so you want to accellerate, you need to thinka bout all of the things you are trying to do and which of those is critical to the goal and which can be sacraficied to go faster to the goal. 

replaforming is also the golden chance to rewrite. to hold onto the door jamb and say you are not pulling me through in this form. if i am going to migrate with all of my baggage, at least i want a chance to take things to the curb and decide which of these things i actually wanted.

