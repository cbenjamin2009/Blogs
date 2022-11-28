# Why I Love Remix!

## Remix is a Framework for building better user experiences, sometimes with React

Remix launched just last week on Monday, November 22nd. It is taking the React community by storm, but why? Continue reading to find out and even take it for a spin in your browser. 

## What is Remix?
Remix is a Framework that is created by the team at Remix.Run and founded by [Ryan Florence](https://twitter.com/ryanflorence) and [Michael Jackson ](https://twitter.com/mjackson). You know, the guys who gave us [React Router](https://github.com/remix-run/react-router). Starting out it can be quickly deployed with React but does not require react.  

## What makes Remix so special?
Remix is taking us back to the glorious 90s 💿 when web development was still a young child learning how to walk. Those original creators who designed the web framework of retrieving data and manipulating data with methods such as GET, PUSH, PUT were quite genius. Remix is built on the Web Fetch API, which means it can run anywhere, but also means that you are using the fundamentals of web development from the 90s and losing the janky concepts you've come to know such as `e.preventdefault()`. 

## Where can you use Remix?
Remix ran run literally everywhere. You can run it serverless, you can run it in Node.js, you can put it on a Cloudflare Worker, or you can publish directly to Vercel, Netlify, and a variety of other hosting platforms within minutes. 

## Why use Remix?
You should use Remix if you are into creating amazing websites with top notch user experience and blazing fast content delivery. It is not a framework for those who love adding transitional spinners on all of their components while they are fetching data because it's just too fast. Remix fetches everything in parallel ‖ instead of the typical Waterfall 💧 approach. Remix also takes care of your State!

## Remix Nested Routes
Remix also gives you a super power called nested routes. Why is this so incredible? Remix only loads the nested route that changed, can update just the single nested component that was updated by user interaction, or if a nested route experiences an error you can catch that with an error boundary and provide a helpful message to your use without crashing your whole app. 

Nested Routes also gives you nested CSS styling. You can load CSS only for the page that you are on, and as soon as the user navigates away from that page, the stylesheet is removed!

## Network tab
If you take a look at the network tab of a Remix project, compared to the project on any other framework you will notice a lot of things are missing! Why is this? Because those geniuses at Remix only bundle and send your user what they actually need for the content on the screen and nothing else, and they remove excess content as soon as it's no longer needed. Remix has it's own cache that makes page reloads faster than Raptor engine on the Starship Rocket 🚀. It reloads anything that hasn't changed from the cache and only fetches new data, it's like magic! 


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/erh5pxwv2r8e1va24jxj.png)

## Turn off Javascript?!?
Did you know, in most instances of Remix you can actually turn off JavaScript on the browser and the page still works?! This is because Remix renders everything server-side and is built on HTML fundamentals. This isn't to say you shouldn't push client-side JavaScript in your code for user experiences, but the core functions of your app will still work without it!

## Go try Remix and let me know what you think! 
The amazing folks at CodeSandbox created a platform for trying Remix right from your browser, give it a go  [here](https://codesandbox.io/s/github/remix-run/remix/tree/main/examples/jokes?utm_source=dotnew)

**Support - Your support is 100% optional**
[You can buy me a Taco to show your support](https://www.buymeacoffee.com/ChrisBenjamin)

Edited 12/1 to clarify that Remix is not solely a React framework.  