# General Questions:

* What did you learn yesterday/this week?

AssetCamp - learned about how to store protected media files properly and restrict from public access using django-sendfiles and updating nginx configurations

* What excites or interests you about coding?

Learning new technologies and working on cool/fulfilling work, being able to solve problems and create beautiful solutions that help people/make their lives easier

* What is a recent technical challenge you experienced and how did you solve it?

See question above (What did you learn yesterday...). Another challenge I've faced recently was while working at Tesla and being tasked with fixing a Visual Basic script to convert/compress CATIA files. Wasn't familiar with the technolgy and couldn't find any resources online to help. So, I had to reverse engineer the current script and eventually found a solution.

* What UI, Security, Performance, SEO, Maintainability or Technology considerations do you make while building a web application or site?

UI - Want to make the website as easy as possible for users to use, but at the same time, make it visually appealing and grab the users attention. Security - HTTPS, POST sensitive information, when storing using information use hash+salt (django). Performance - compress images, lazy loading, caching, CDNs. Maintainability - make site modular and easy to scale, while using technologies I'm comfortable with in order to move quickly. 

* Talk about your preferred development environment.

Macbook, VS Code, Terminal, Chrome

* Which version control systems are you familiar with?

Git

* Can you describe your workflow when you create a web page?

Figure out objectives, break down tasks and priorities, draw designs, make mockups, get approval, build the backend, build basic front-end, flush out details and edge cases, write test cases.

* If you have 5 different stylesheets, how would you best integrate them into the site?

Use a CSS precompiler like SASS/SCSS and combine them into one to limit the number of requests a page has to make to get styles.

* Can you describe the difference between progressive enhancement and graceful degradation?

From StackOverflow - **Graceful Degradation** starts at a ideal user experience level and decreases depending on user agent capabilities down to a minimum level, catering for agents that don't support certain features used by the baseline. **Progressive Enhancement** starts at a broad minimum user experience and increases depending on user agent capabilities up to a more capable level, catering for agents that support more advanced features than the baseline.

* How would you optimize a website's assets/resources?

Compress images, minimize CSS/JS files, combine them into one, use a CDN, CSS at the top, JS at the bottom, cache.

* How many resources will a browser download from a given domain at a time?

Depends on the browser, but usually it can download around 6 in parallel. For Chrome it is 6, Firefox 6, IE11 is 8.

  * What are the exceptions?

  So the limits are per domain, so in theory, you could use a wildcard DNS where *.foo.com points to the same IP address. Seems like something I'd use with caution though. I think if you are at this point (of trying to get around the limit) then maybe you are doing a bit too much.

* Name 3 ways to decrease page load (perceived or actual load time).

Minimize images, css, and js files, reduce number of file requests (css, js, and images) by combining them, use a CDN (cache).

* If you jumped on a project and they used tabs and you used spaces, what would you do?

Follow project guidelines. I prefer tabs (can always use something like VS Code and convert tabs/spaces).

* Describe how you would create a simple slideshow page.

If something like jQuery was used, we could find a plugin. Or if required to create it from scratch, I would start by formatting the HTML, use CSS to add styles and keep all slides besides the first one to be hidden. Write some JavaScript functions so that when they press next, it triggers a transition to the next slide.

If the slideshow was the main part of the page, can use anchors or something so that they can bookmark the slide specifically or share a link.

* If you could master one technology this year, what would it be?

Would love to learn more about VueJS, have heard really good things about it. Did a tutorial earlier this year and it seems really cool. But if I could master one thing - it would be JavaScript. I feel like if I could fully master this, I would be able to pick up the other JS libraries and frameworks really quickly.

* Explain the importance of standards and standards bodies.

Standards make it easier for developers to achieve a more stable web, so the stuff you build cna be cross-browser and platform. Also aids in development and maintenance time and allows for backwards compatibility.

* What is Flash of Unstyled Content? How do you avoid FOUC?

Flash of unstyled content is when there's a quick flash during page load where you can see the content unstyled. It is a side-effect of the CSS @import rule that exists in the Windows version of IE 5 or newer. The LINK element solution — the addition of a link element to the basic head element is the most natural solution to the FOUC problem as almost every page can benefit from the addition of either an alternative stylesheet or a media-dependant stylesheet. 

In an attempt to avoid unstyled content, front-end developers may choose to hide all content until it is fully loaded, at which point a load event handler is triggered and the content appears. 

* Explain what ARIA and screenreaders are, and how to make a website accessible.

From Mozilla - Accessible Rich Internet Applications (ARIA) defines ways to make Web content and Web applications (especially those developed with Ajax and JavaScript) more accessible to people with disabilities. For example, ARIA enables accessible navigation landmarks, JavaScript widgets, form hints and error messages, live content updates, and more.

ARIA is a set of special accessibility attributes which can be added to any markup, but is especially suited to HTML. The role attribute defines what the general type of object is (such as an article, alert, or slider). Additional ARIA attributes provide other useful properties, such as a description for a form or the current value of a progressbar.

* Explain some of the pros and cons for CSS animations versus JavaScript animations.

CSS animations:
 Pros - great for simple transitions between states (like rollovers) when compatibility with older browsers isn't required. 3D transforms usually perform very well (iOS7 being a notable exception), and CSS animations can be very attractive for developers who prefer putting all of their animation and presentation logic in the CSS layer. Use CSS animations for simpler "one-shot" transitions, like toggling UI element states.
 Cons - CSS-based animation doesn't work in IE9 and earlier.

JS animations:
 Pros - JavaScript-based animation delivers far more flexibility, better workflow for complex animations and rich interactivity, and it often performs just as fast. Use JavaScript animations when you want to have advanced effects like bouncing, stop, pause, rewind, or slow down.
 Cons - 

* What does CORS stand for and what issue does it address?

From Mozilla - Cross-Origin Resource Sharing (CORS) is a mechanism that uses additional HTTP headers to let a user agent gain permission to access selected resources from a server on a different origin (domain) than the site currently in use. A user agent makes a cross-origin HTTP request when it requests a resource from a different domain, protocol, or port than the one from which the current document originated.

An example of a cross-origin request: A HTML page served from http://domain-a.com makes an <img> src request for http://domain-b.com/image.jpg. Many pages on the web today load resources like CSS stylesheets, images, and scripts from separate domains, such as content delivery networks (CDNs).

For security reasons, browsers restrict cross-origin HTTP requests initiated from within scripts. For example, XMLHttpRequest and the Fetch API follow the same-origin policy. This means that a web application using those APIs can only request HTTP resources from the same domain the application was loaded from unless CORS headers are used.

