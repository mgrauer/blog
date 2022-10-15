My team is part of a much larger replatforming journey, where getting off of a PHP monolith is the grand backdrop. Our specific local flavor of replatforming is informed by the fact that we are a frontend team.

Describing replatforming as a single dimension obscures the understanding needed to guide and maximize the value of our local replatforming program.

## Pre-Replatforming Preparation: Decoupling Frontend Code via Webpack Migration

Frontend code had been previously built, bundled, and deployed as part of the PHP monolith. As a prepatory step to replatforming, frontend pages need to be decoupled from the monolith: split into a separate repo, built and bundled with Webpack, and deployed independently to a k8s microservice for SSR.

## Dimensions of Replatforming

These dimensions are mostly orthogonal.

1. No monolith dependencies - The primary remaining monolith attachments are:
  -  Traffic routing - does the directly service accept external network traffic?
  - Initial page load - all of the page routing, pre-action boilerplate, context and data gathering, event tracking, etc. that occur withn the PHP controller. 
  - Data hydration requests that occur during SSR.
2. Monorepo - Pages started out in the monolith, were decoupled into their own repos, and need to be re-coupled into a monorepo.
3. Server Driven UI - Components are to be rewritten so that they can be programmed by non-engineers using a CMS.
4. Federated GraphQL
 - Page level data should come from the Federated Graph, rather than a monolith controller.
 - Components data should be hydrated from the Federated Graph, rather than by calling monolith REST or graqphl endpoints.
5. Common Component Usage - Replatformed pages should use common components, intended for reuse, rather than one-off components.
6. Reusable Component Definition of Done - Components should adhere to the Reusable Component Defintion of Done.
7. Next.js
  - Pages should us next.js for SSR instead of using the homegrown-SSR.
  - Pages should accept externally routed traffic at the next.js webserver.
8. 100% of production traffic - Traffic can be split between the next.js and homegrown-SSR version of the page based on
  - Percentage of traffic
  - Regex matching in the URL
