# replatforming.
## background, or, how we got to a monolith and why we want to get off of it now

### how we got to a monolith


the ops team co-evolved over many many years. the ops team and the codebase were symbiotic, they each worked well with the other. the ops team had the knowledge to provision new hardward, to scale services, to monitor and diagnose and debug and take corrective action. all of that knowledge was centralized. and the culture of service ownership was also centralized there. 

they owned a service which is a live thing, and needed constant maintenance and tuning. they could also see and coordinate from their central location. so when someone put something in that broke the overall service, because of some interaction between pieces, the ops team would notice this, find the teams responsible, roll back the train, and tell the offenders to fix their stuff.

this worked great, and it allowed the company to scale up very quickly the number of developers who could create features and even other business services. since we already had the infrastructure and team to run a web application, why not add on a new application to deal with supplier data, or one for managing human resources? 

people on feature teams didn't have to know about alerting, or scaling, or writing performant code, or noticing when things went wrong. they didn't have a culture of service ownership, which made sense for the business at the time. so these teams thought about how many features they could get done, and they would launch a feature and move on to the next one. they didn't have to set aside time for maintaining the service, or making sure the service performed effeciently, as the demands on the service scaled up, or that they service was reliable, or that it could handle a certain amount of load.

Long ago there was a small PHP application that ran a website, and it used SQL Server relational databases, and it deployed onto some specific set of computers in datacenters. It was a relatively simple application that allowed people to purchase things online. The number of developers working concurrently in the codebase was small, and it was fairly clear who was working in what area of the codebase, i.e. ownership was clear. When developers wanted to update the production website with some change, they could simply roll out the change to the website. Sometimes a change would break something, and then the developer who rolled out that change could roll it back. 

Over time, as the scope and scale of the application and business grew, more and more developers were working in the same codebase and the codebase grew enormously. The application was asked to do more and more, to run other parts of the business, to add new features onto the existing business, to try experiments some of which failed and some suceeded. At a certain scale, limits had to be placed on who could deploy and rollback. If every developer could deploy, then you'd be doing deployments all the time and it would be hard to see what was going on, as changes stacked up on top of each other in production. If there was a problem in production, it was much harder to tell which change caused it, and if you wanted to rollback the change, how many of the changes would you have to rollback?

So an Op(eration)s team formed, to support all of the operations of the software in production, they would manage the deployment, the rollback, the scaling of resources in line with demand (because sometimes there were a lot of customers and sometimes there weren't many), and detecting and responding to problems in production. The Ops team co-evolved with the PHP application, meaning the technical, organizational, and cultural choices of the Ops team and the organization as a whole developed based on the assumptions that the centralized Ops team would manage all of this really hard infrastructure work. This allowed for scaling up the number of developers because they had to understand so much less: if you are a developer and you want to add a new endpoint, you just need to add a bit of PHP code, wait for Ops to deploy it, and then it is in production withoyu you having to understand anything else.That is an amazing experience for developers, and for managers and recruiters. We can take fairly junior developers and let them start createing features, with low friction.









### why was the monolith good and how did monolith come to limit the thing it was designed to enable

At a certain scale, the whole process of deployment and rollback got to be too hard to coordinate, so he Ops team created a deploy train. If you were a developer and you had a change you wanted to get rolled out to production, you would add your changeset to the deploy train. The deploy train had a certain number of seats on it, and would depart a couple times per day. So you couldn't get your code rolled out right away, and there needed to be space for your change, which slowed down your delivery. As many different changes were bundled together into a deploy train, that increased the likelihood that any one of them would cause problems, and when the deployment was rolled back, it had to be rolled back atomically, that is, all of the changes went out to gether and had to get reverted together. So now, as a developer, you had to wait longer for your deployment to go out, and you r deployment could get reverted even if there weren't any problems in your code. Which meant longer delays in delivery. One of the ways to increase the volcity of deployments was tio increase the size of the deploy train, but the problem with that is it also increased the liklihood that there waold be a problem from any one of the indicidual changes.

as the size of the codebase grew, and as the size of the organization grew, ownership got much harder. teams would be formed, and then re-orged into other teams or out of existence. who was responsible for the code that they used to write? and code would get copy and pasted around like crazy, and then the anscestor pieces of code would change but not all of the descendents.

similarly to the difficulty of unclear ownership in code, there was also shared ownership of the data. common database tables that multiple different pieces of code, and eventually multiple separate applications would be reading and writing from and changing. which makes it very hard to understand what is happening with the data, who relies on the data, what you can change without breaking things for others, and hard to scale and tune your database--becasuse you are putting several different workloads onto the same database that each require different performance charateristics.




another problem was that as the number of changes on the train grew, and because they rolled the train back atomically, it prevented teams from taking ownership even if they wanted to. if you are a team and some feature from your team or another team causes problems for your area, it's harder to identify because there are larger changesets, and you have to roll the entire thing back at once. what if the problem is subtle, and only occurs over time, or occurs as the result of interactions between several different changes that interact in unexpected ways. it's harder to notice that because the background of changes is so much larger. and if you want to test, does some fix actually help, you have to wait for the integrator train, and wait for that to be deployed, and when it is deployed since it is going along with so many other changes, was the fix even if t was fixed, was that the result of your change or some other change?

so even if teams wanted to have ownership over their area, it made it harder.

one analogy is a knife. you start out with a very specific need, and you have a swiss army knife that only has a blade. then you see the utility of the thing, well, i'm already carrying it around, why don't i add a corkscrew, and a file, and a scissors. before long, your small specific tool that did one thing well is now a giant hunk of metal, it's bloated and awkard, and even using the knife on it is hard.

now imagine that something has gone wrong, and you need to trace it down. there's a bunch of code that no one owns

i lived in an old house. it started out as a beautiful grand victorian house in 1897. over time it was split into a number of apartments, each of which needed their own electicity, gas, water, and heat. and then several of those apartments were merged into two apartments by the time i lived there. in the basement, there was a maze of piping, and it was nearly impossible to tell what went where, what was still active, or what was no longer in use. any time someone came out for maintenance, they couldn't make any sense of it, so they would lay in some additional piece, or else make the smallest possible change to the existing mess. no one was thinking about the overall whole, and cared about making it easy to maintain, buecause that would be a lot of owork to figure out which pipe went where, and remove many ofthe old ones, or at least label them. so people came in and did their one change, and thechanges stacked up over time. this is like the code in the monolith. but also, notice that it worked! the apartments got the services they needed, eveni f it took longer to fix anything when it brok or make any changes.



### why do we want to get off of it now

move fast
it also clarifies ownership
hiring, new tech, engineers. more attractive to new engineers, and easier to staff.


### why is this hard

talk about service ownership, as a culture, everyone needst obe aware, and roll things back, or try things, but can try things and try things quickly. but this is a culture change for people, they need to learn to own their area, and hold others working in their area accountable. that is a new responsibility, to tell people that you own this area nd they cannot do what they want, or they need to back off, or even the timeframes that they can do things.

and this technology transformation doesn't happen everywhere uniformly at the same time. it takes time for different parts of the organization to adapt these changes, which means you can be operating across a culture divide. one team has already taken on the culture of service ownership, and wants their service to be reliable, and expects others working in their area to also have that ownership mindset, but they don't. they don't know to monitor their changes, they don't know to look if theyve added more errors to the logs with their changes, and they may not like it when the service owning team tells them they need to have their features roolled back because it breaks some availability metric of the service. they are trying to do their job and their job is to launcrate new features, and now they have this team standing in their way.or on their roadmpa, they don't have time allocated to the mainteance of their service and the fact that the service is now a living thing that neededs maintenencae. so this is also a culture change that product management needs to understand ad adapt to. they need t o buffer for this adaitional capacity on their roadmaps.

rebuilding the plane in flight. engine vs airframe, retrain crew in the process.

coupled/decoupled organization
constraints, everything is changing
barnacles

it also clarifies ownership, which is part of the idea of separating out domain boundaries. and apis. not just between code, but between different groups in the organization. conways law, and many correlaries. ownership is unclear, so people try to track down who is responsible, and everyone tries to push off this responsibility on others until someone is left holding the bag.

in theory this should be good for the business to, so it can see clearer ownership, and also it needs to maek staffing decisions about supporting a certain service, with a certain service tier, but there is a cost to that for engineers and also computer resources.



---

# hopper




---

# future posts

DORA metrics. what are the key DORA metrics? why are they important? they business can't change as fast, and can't learn as fast. you only learn by putting things into production, and interacting with the market. the market provides feedback and you adjust, so the faster you can adjust the more you can learn and the better you can take advantage of those signals from the market to give the market what it wants.

breaking out of the monolith, into individual microservices, that can be independently deployed and rolled back, and owned by smaller teams. this means that the change frequency is higher and the lead time is lower. but it also means that teams need to take on new responsibilities and cultures and skills. 


what does replatforming look like for my team?







My team is part of a much larger replatforming journey, where getting off of a PHP monolith is the grand backdrop. Our specific local flavor of replatforming is informed by the fact that we are a frontend team.

Describing replatforming as a single dimension obscures the understanding needed to guide and maximize the value of our local replatforming program.

Pre-Replatforming Preparation: Decoupling Frontend Code via Webpack Migration
Frontend code had been previously built, bundled, and deployed as part of the PHP monolith. As a prepatory step to replatforming, frontend pages need to be decoupled from the monolith: split into a separate repo, built and bundled with Webpack, and deployed independently to a k8s microservice for SSR.



---



















what is replatforming, why are we doing it, why is it hard, how does it help the business, what are the changes in culture and technology needed and organization? how does it create clearer lines of ownership and the ability to move fast? how does it slow things down? what are the risks?


We are replatforming! Hooray! It's a giant multi-year technology led initiative that is incredibly confusing. 

What is replatforming?

This isn't exactly what happened, but it is close enough.

 

what is a good example here? B2B vs B2C big small vs small big.




swiss army knife

jumble of pipes

ownership of code
ownership of data, tables
APIs





