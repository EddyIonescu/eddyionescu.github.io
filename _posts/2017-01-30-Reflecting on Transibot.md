Look, I made a chatbot!

We all know that chatbots are one of the latest buzzwords floating around in the tech world, but when was the last time a chatbot really did anything useful for you (or maybe the other way around if it was advanced enough)?

I decided that I wanted to make a chatbot myself because:

<h2 id="l1">It's a cool side-project</h2>

Self-explanatory. Apps are so 2015. Plus back-end development is more interesting than client-side.

<h2 id="l2">It's possibly the future - and might inspire some good ideas</h2>

The biggest advantage they offer over web-sites (no matter how fast and mobile-friendly) and apps is that they're so easy to use. Our home screens are cluttered with apps that we'll likely never use ever again - but it's guaranteed that there'll be at least one messaging app front and centre. There's nothing to install, nor to bookmark. Instead of searching up a friend, you search up "Transibot" - and you're in!

That's the name of the chatbot I made - Transibot. It tells you when the next buses closest to you will arrive - in real-time! 
It only works for Waterloo Region at the moment - but I plan on adding more cities soon.

The compromise is freedom (on the front-end, at least). 

Facebook's API is limited in how you can communicate with the user - so I couldn't show an interactive map and read-in the input or show any nested-menus.
The front-end had to be simple and straightforward. While this works well for Transibot, it's difficult to have bots for apps like Spotify, 
where tapping a few times is easier than typing out what you want to listen to. It'd be like using a command-line version!

Monetization is also an issue - as even most subtle ad or sponsored message would stand out in a conversation (plus, it's against the rules for Messenger bots).
So while you can't monetize the front-end, there's a lot of potential in monetizing the back-end (did anyone say big data?).

When it comes to the back-end, you couldn't have any more freedom.

Unlike mobile apps, where you have to resubmit each time you change something, there's no way anyone can enforce what my bot's going to respond.

Want to make an iOS app? Time to get a mac, learn Swift or Objective-C, get familiar with the Cocoa framework, and hope Apple will approve it before you find a bug and resubmit.

Want to make a bot? Can you code? Good, you're all set! 

<h2 id="l3">Making Transibot</h2>

Yes, you can make a bot in mostly any language. <a href="https://docs.racket-lang.org/more/">Even Racket</a> (a dialact of Scheme used in the introductory CS course at Waterloo. While it's great for teaching functional programming concepts, it's not used in the "real-world").

It's actually very straightforward. You just respond to a bunch of API calls. 

A "bunch" can get fairly intensive after a while - so I looked for a platform that had no issues handling a bunch of connections and that was modern and widely used.
After doing a bit of research it turned out that Node.JS was actually a good fit - and that's why Facebook (and others) have sample-bots in Node.JS.

Advantages: 
<ul>
<li>It's lighter and can handle a lot connections. This is because it's asynchronous by nature. It loops through the event loop and only executes callback functions when triggered.</li>
<li>It's well-suited to the design-patterns behind making bots. Why? Because the back-end for a bot will only respond to events triggered by Messenger, and so callbacks are the simplest way of implementing this logic. </li>
<li>Working with REST APIs, JSON files, and MongoDB (which also works in JSON) is very easy as data-structures are represented like that in JavaScript.</li>
<li>NPM Package Manager makes it easy to use and distribute libraries</li>
</ul>

Disadvantages: 
<ul>
<li>It's Javascript. Syntax standards are sometimes inconsistent - and the syntax itself is very ugly. Maybe one day Node.JS will diverge from the rest of JS and establish itself as a different back-end language (after all, many refer to it as just "Node").</li>
<li>It's difficult to write good code. Why?  </li>
<ol><li>Dynamic Types: This makes it harder to know whether your code will actually work or just crash. Static typing takes more typing, but in the end it saves a lot of time.</li>
<li>Undefined: Everything is undefined. Property doesn't exist? Undefined. Out of bounds? Undefined. Give me an undefined? No problem, I'll give you an undefined. Debugging can quickly become a nightmare.</li>
<li>Everything's asynchronous. It requires a different (more functional) way of thinking. Calling one function right after another does not guarantee that it'll finish last - you (or rather, your bot) can get a lot of strange behaviour, like: Goodbye! Next bus in 5 minutes. Good morning. Get used to Callbacks and Promises. </li></ol>
</ul>

Coping strategies:
<ul>
<li>Pretend it's a functional programming language. See, learning Racket really helped here.</li>
<li>Use <a href="https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise">Promises</a> for handling network calls instead of a bunch of callbacks - makes the code a bit easier to reason about.</li>
<li>Comment in what type of objects I'm getting properties from. Node has no way of knowing whether a property I got from a REST API call exists without actually trying it - so being clear in my code is a great way to spend less time debugging.
</ul>
Now for making it. I decided to host it on AWS because while it's more complicated than say, Heroku, it's way more powerful and I wanted to become familiar with it.

I'm actually hosting two chatbots: a production version and a development version (the master and dev branches on <a href="https://github.com/EddyIonescu/Transibot">github</a>).

Production:
<ul>
<li>Hosted on AWS Elastic-Beanstalk.</li>
<li>Has auto-scaling and load-balancing.</li>
<li>Used AWS certificate manager to make SSL certificate (free) for bot.transibot.com (as Messenger only communicates via HTTPS).</li>
<li>Configured VPC (Virtual Privater Cloud) as to have outside Internet access and allow inbound connections.</li>
</ul>

Development:
<ul>
<li>Hosted on AWS EC2 Instance</li>
<li>Couldn't use AWS certificate manager for EC2, so I had to buy one (about $10/year, thanks <a href="http://velocity.uwaterloo.ca">Velocity</a> for covering that :) for devbot.transibot.com (as Facebook wants certificates signed by a certificate authority).</li>
<li>Used NGINX on Ubuntu as to set up HTTPS and SSL on the instance.</li>
</ul>

Tutorials I referred to:
Note - I didn't follow any of these tutorials exactly, they were just useful references for when I was unsure of how do to something. They may be outdated. If you run into problems, use Google/StackOverflow.

<ul>
<li><a href="https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-16-04">digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-16-04</a></li>
<li><a href="http://programmersdiary.com/node/deploy-mean-stack-app-on-amazon-ec2/">programmersdiary.com/node/deploy-mean-stack-app-on-amazon-ec2/</a></li>
<li><a href="https://www.digitalocean.com/community/tutorials/how-to-create-an-ssl-certificate-on-nginx-for-ubuntu-14-04">digitalocean.com/community/tutorials/how-to-create-an-ssl-certificate-on-nginx-for-ubuntu-14-04</a></li>
<li><a href="http://www.zoharbabin.com/install-ssl-on-ubuntu-and-enable-https/">zoharbabin.com/install-ssl-on-ubuntu-and-enable-https/</a></li>
</ul>

See it on Github: <a href="https://github.com/EddyIonescu/Transibot">github.com/EddyIonescu/Transibot</a> and try it out here: <a href="https://m.me/Transibot">m.me/Transibot</a>


<h2 id="l4">Thoughts for the future</h2>

Chatbots won't be used everywhere - there'll still be a place for apps and web-sites; it's the simpler ones that will either convert to chatbots or die out and get replaced by competitor bots.

On the other hand, because they're so easy to use, they have the potential to disrupt (for lack of a better buzzword) existing services that exist only as mobile apps. 

Take Google Maps, for example; finding out when the next bus will arrive takes way more work - like opening the app, inserting your destination, tapping some other stuff, etc.

Once Transibot starts supporting more cities around Canada and the US along with other platforms (WeChat, Kik, Skype), I'll follow up on its usage, trends (yay, big data!), and other things I've learned along the way.



