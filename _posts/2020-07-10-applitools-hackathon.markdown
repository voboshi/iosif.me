---
layout: post
title:  "Applitools' 2020 hackathon"
date:   2020-07-10 21:54:43 +0000
categories: blog testing qa
---

I've written up a few thoughts following Applitools' [summer 2020 hackathon](https://applitools.com/hackathon/). Massive thanks to [Stas Michalski](https://www.linkedin.com/in/stas-michalski-8b7b4416/) from [Applitools](https://applitools.com/) for mentioning the hackathon, I'd have been oblivious otherwise!

## Getting Started

There were two sides to the task: what Applitools referred to as modern tests (using their Ultrafast Grid) and traditional tests (setting up a test framework from scratch, using any tool we prefer). For both, there were 3 tasks that needed to be automated, on 3 breakpoints in all major desktop browsers and one mobile browser: validating a product listing page's responsiveness and layout, using filters on it and validating that their functionality is correct and validating the responsiveness and layout of a product details page. These tasks would be executed against a V1 of the website (considered "bug-free") and would then be used as a regression pack against a V2 / rewrite of the website.

## Setting up Cypress for the Ultrafast Grid

I used [Cypress](https://cypress.io) as I wanted a tool where I could quickly iterate, get human-readable errors and feel comfortable. The browsers required (Chrome, Firefox, Edge Chromium) are all compatible and the system under test was on a single domain, meaning I wouldn't be at any disadvantage choosing Cypress.

No advanced features were needed (e.g. stubbing or intercepting network responses).

The modern tests were extremely easy to set up. I ended up with a minimal amount of dependencies (Cypress itself and the Appium SDK), I configured the needed browsers, layouts and concurrency in applitools.config.js and wrote three spec files (between 23 and 34 lines of code, including all typical boilerplate). 

We were instructed to run these tasks against the V1 website, then mark the runs as our baselines. We would then perform the needed refactors to run the tasks against the V2 website (a 3 character difference between the two folders!) and mark all the bugs in Applitools. Needless to say, the Visual AI did its job, so all I had to do was mark the areas detected and write up what the defect is. 

In summary, for the modern tests: 2 npm dependencies, 6 CSS selectors, 109 total lines of code, 3 character difference to "refactor" the tests to run against a second website, under an hour of work.

## Setting up the traditional cross-browser tests

For the traditional tests I implemented the features that most colleagues are either used to or would implement themselves: a spec-file per layout, page objects, custom commands, screenshot diff-ing, linting and custom logging. 

This may sound like overkill, especially compared to the above, but the end structure was reached iteratively. Neither one of the three plug-ins I tried for screenshot diff-ing (cypress-image-snapshot, cypress-visual-regression and cypress-plugin-snapshots) gave results in any way similar to Applitools. I won't blame these plug-ins, as I had a limited amount of time to get it all working, so most likely gave up sooner than one should have. Since screenshot diff-ing was unreliable, the other way I would check if elements were on-screen for each layout was to use their CSS selectors and perform assertions. In total, I used 57 selectors, so to make future refactor work easier, I implemented page objects. I also needed custom methods to log test results to a text file, which was a requirement for the hackathon.

I didn't count all the lines of code in the traditional solution, as the comparison would have sounded absurd. There was a difference of 12 lines of code between running the suite against V1 and V2 as some selectors needed adjusting. 

## Final thoughts and conclusion

Some other interesting findings:
- on the product details page, the checks between V1 and V2 would have needed another day's worth of work to fit in, as they were either slight layout differences or, my absolute favourite, a different text description inside the product's size drop-down! 
- while Cypress now supports Firefox, it is in beta, and I was not able to get the browser to run the tests. (for reference, Cypress was at v4.9.0 and Firefox was at v77.0.1)

All in all, it has genuinely been an eye-opening experience, as the tasks were similar to what we'd need to do "in the real world" and the total work done exceeds the scope of usual PoCs. 

This post has also been featured on the [Applitools blog](https://applitools.com/blog/how-easy-is-cross-browser-testing/).