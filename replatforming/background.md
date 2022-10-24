what does this post want to cover?

what is replatforming, why are we doing it, why is it hard, how does it help the business, what are the changes in culture and technology needed and organization? how does it create clearer lines of ownership and the ability to move fast? how does it slow things down? what are the risks?


We are replatforming! Hooray! It's a giant multi-year technology led initiative that is incredibly confusing. 

What is replatforming?

This isn't exactly what happened, but it is close enough.

Long ago there was a small PHP application that ran a website, and it used SQL Server relational databases, and it deployed onto some specific set of computers in datacenters. It was a relatively simple application that allowed people to purchase things online. The number of developers working concurrently in the codebase was small, and it was fairly clear who was working in what area of the codebase, i.e. ownership was clear. When developers wanted to update the production website with some change, they could simply roll out the change to the website. Sometimes a change would break something, and then the developer who rolled out that change could roll it back. 

Over time, as the scope and scale of the application and business grew, more and more developers were working in the same codebase and the codebase grew enormously. The application was asked to do more and more, to run other parts of the business, to add new features onto the existing business, to try experiments some of which failed and some suceeded. At a certain scale, limits had to be placed on who could deploy and rollback. If every developer could deploy, then you'd be doing deployments all the time and it would be hard to see what was going on, as changes stacked up on top of each other in production. If there was a problem in production, it was much harder to tell which change caused it, and if you wanted to rollback the change, how many of the changes would you have to rollback?

So an Op(eration)s team formed, to support all of the operations of the software in production, they would manage the deployment, the rollback, the scaling of resources in line with demand (because sometimes there were a lot of customers and sometimes there weren't many), and detecting and responding to problems in production. The Ops team co-evolved with the PHP application, meaning the technical, organizational, and cultural choices of the Ops team and the organization as a whole developed based on the assumptions that the centralized Ops team would manage all of this really hard infrastructure work. This allowed for scaling up the number of developers because they had to understand so much less: if you are a developer and you want to add a new endpoint, you just need to add a bit of PHP code, wait for Ops to deploy it, and then it is in production withoyu you having to understand anything else.That is an amazing experience for developers, and for managers and recruiters. We can take fairly junior developers and let them start createing features, with low friction.

At a certain scale, the whole process of deployment and rollback got to be too hard to coordinate, so he Ops team created a deploy train. If you were a developer and you had a change you wanted to get rolled out to production, you would add your changeset to the deploy train. The deploy train had a certain number of seats on it, and would depart a couple times per day. So you couldn't get your code rolled out right away, and there needed to be space for your change, which slowed down your delivery. As many different changes were bundled together into a deploy train, that increased the likelihood that any one of them would cause problems, and when the deployment was rolled back, it had to be rolled back atomically, that is, all of the changes went out to gether and had to get reverted together. So now, as a developer, you had to wait longer for your deployment to go out, and you r deployment could get reverted even if there weren't any problems in your code. Which meant longer delays in delivery. One of the ways to increase the volcity of deployments was tio increase the size of the deploy train, but the problem with that is it also increased the liklihood that there waold be a problem from any one of the indicidual changes.

as the size of the codebase grew, and as the size of the organization grew, ownership got much harder. teams would be formed, and then re-orged into other teams or out of existence. who was responsible for the code that they used to write? and code would get copy and pasted around like crazy, and then the anscestor pieces of code would change but not all of the descendents.

similarly to the difficulty of unclear ownership in code, there was also shared ownership of the data. common database tables that multiple different pieces of code, and eventually multiple separate applications would be reading and writing from and changing. which makes it very hard to understand what is happening with the data, who relies on the data, what you can change without breaking things for others, and hard to scale and tune your database--becasuse you are putting several different workloads onto the same database that each require different performance charateristics. 

what is a good example here? B2B vs B2C big small vs small big.



swiss army knife

jumble of pipes

ownership of code
ownership of data, tables
APIs









My team is part of a much larger replatforming journey, where getting off of a PHP monolith is the grand backdrop. Our specific local flavor of replatforming is informed by the fact that we are a frontend team.

Describing replatforming as a single dimension obscures the understanding needed to guide and maximize the value of our local replatforming program.

Pre-Replatforming Preparation: Decoupling Frontend Code via Webpack Migration
Frontend code had been previously built, bundled, and deployed as part of the PHP monolith. As a prepatory step to replatforming, frontend pages need to be decoupled from the monolith: split into a separate repo, built and bundled with Webpack, and deployed independently to a k8s microservice for SSR.
