# Remix - Simple Fetch and Render Data, Styled with TailwindCSS 3.0

## A quick Remix tutorial on fetching data
Remix is a Web Framework for creating amazing user experiences. 

We will be covering how to fetch data from GitHub Organization Members. This will fetch a default company, and render the members avatar picture, their username, and a link to their profile. It can be used for any organization on GitHub. 

Here is an image of what we will be creating: 

![A website showing the 4 members of the Remix-Run team with their profile pictures, usernames, and links to their profile](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fhrfsjnbfyche4r322dc.png)

We are going to create a default page that fetches data. We are also going to have a search feature so that users can type on a search and be re-directed to a page that shows their results. I'm using TailwindCSS for a small bit of styling and their new columns feature in V3.0. We will be using the power of Remix to avoid any useState or useEffect hooks and just let the web do what it was designed to do. We will also get to peek at the network tab and see how Remix is pulling our cached data without us doing any work!

Let's get started! 🚀


## Remix App
Let's create the default Remix app. 

For this tutorial, I'm going to be using the default `create-remix@latest` command which sets up our project and gives us a demo site, we will also be using the Remix App Server for local testing. You can change this at a later time if you wish to deploy it. 

Open your terminal and run `npx create-remix@latest` 
When prompted where you want to deploy, choose Remix App Server. Name your project however you like, I'm going to name mine _remix-fetch-example_.

## TailwindCSS 3.0 Setup with Remix
Open your terminal and let's install tailwind
`npm install -D tailwindcss`
We also need Concurrently for running tailwind.css in our dev environment. 
`npm install concurrently`

Perfect, now lets initialize Tailwind to create our tailwind.config.js file
`npx tailwindcss init`

Let's update our `tailwind.config.js` file in our application for their purge function for any javascript `.js` or `.jsx` file. 
```javascript
module.exports = {
  purge: ["./app/**/*.{js,jsx}"], // Here we are going to tell Tailwind to use any javascript .js or .jsx file
  theme: { extend: {

  } },
  variants: {},
  plugins: [], 
};
```

Perfect, now we need to update our `package.json` file with scripts to generate our tailwind.css file. 
Update your `package.json` scripts section to match this
```javascript
  "scripts": {
    "build": "npm run build:css && remix build",
    "build:css": "tailwindcss -o ./app/tailwind.css",
    "dev": "concurrently \"npm run dev:css\" \"remix dev\"",
    "dev:css": "tailwindcss -o ./app/tailwind.css --watch",
    "postinstall": "remix setup node",
    "start": "remix-serve build"
  },
```

Now when we run `npm run dev` it will generate a tailwind.css file in the root of our /app/ folder. We need to tell Remix that we can to use this style sheet. I'm going to set this up in our Root file so that TailwindCSS styles are imported to the entire site. Remix does this by importing our styles and using their links function to apply the stylesheet to the head of the HTML file. 

Open your `root.jsx` file under (`/app`)
Add the following import statement and then update the exported links function:
```javascript
import tailwindstyles from "./tailwind.css";


// https://remix.run/api/app#links
export let links = () => {
  return [
    { rel: "stylesheet", href: globalStylesUrl },
    {
      rel: "stylesheet",
      href: darkStylesUrl,
      media: "(prefers-color-scheme: dark)"
    },
    { rel: "stylesheet", href: tailwindstyles }
  ];
};
```

Perfect, TailwindCSS is all setup in our Remix app

## Add a link to our new page
Open your `root.jsx` file under (`/app/root.jsx`)

Locate the section `function Layout({ children }) {`
Add a list item to `/fetch`
```javascript
              <li>
                <Link to="/">Home</Link>
              </li>
              <li>
                <Link to="/fetch">Fetch</Link>
              </li>
              <li>
                <a href="https://remix.run/docs">Remix Docs</a>
              </li>
```

## Create Fetch Route
In Remix, creating a route is as simple. In your (`/app/routes/`) folder, create a new folder called `fetch`. 

### Creating fetch module
We are going to create a single module that will be used to pull in data to our app, it will be used both by our default fetch route and our search route. 

This module will have a single function that fetches and returns data for a given company. Our function will accept a parameter called 'company'. If the parameter is not used then, by default we are going to fetch the Remix-run GitHub organization. 

Create a new file called `github.js`
Add the following 5 lines of code, that's it, 5 lines to fetch data 🚀
```javascript
export async function getMembers(company){
   const searchCompany = !company ? "remix-run" : company;
       let res = await fetch(`https://api.github.com/orgs/${searchCompany}/members`)
    return res;
}
```

### Creating Fetch Index Page
We need a default page when users visit our /fetch route, to tell Remix which default page to load, we are going to create a `index.jsx` file inside our `/fetch` folder. 

First we are going to need to load data, we are going to use the Remix loader function for this and we need to import our getMembers function from our GitHub module. 

Update your (`/app/routes/fetch/index.jsx`) as follows: 
```javascript
import { getMembers } from "./github";

export let loader = async () => {
    return getMembers();
}
```

Perfect, now we want to use this loader in our default function so we can access the content. 

First, we have to import `useLoaderFunction` from Remix so let's add this to the top. 
`import { Form, useLoaderData, redirect } from "remix";`

Then we need to create our default function. Add this to the same index.jsx file. There are some basic TailwindCSS styles being applied here, be sure to include them. 
```javascript
export default function Fetch(){
    let members = useLoaderData();

    return (
        <div>
            <div>
            <h1 className="text-xl text-red-500 my-5">Hello Fetch</h1>
            </div>
            <div>
                <div className="columns-1 md:columns-2 lg:columns-xl">
                   {members.map(member => (
                    <div key={member.login} className="flex flex-col w-full border-solid border-2 border-blue-400 rounded-lg px-2 py-2 my-0.5">
                        <img src={member.avatar_url} className="w-10 h-10 rounded-full" />
                        <h3 className="text-1xl font-semibold">User: {member.login}</h3>
                        <a href={member.html_url}>Github Profile: {member.html_url}</a>
                    </div>
                    ))}
                </div>
            </div>
        </div>
    )
}
```

Perfect, we should now be rendering data on our site. Let's load it up and give it a try. 

Start your server using `npm run dev`. If it doesn't open automatically, then bring up your site using http://localhost:3000/ 

If you see your content being displayed then we are half-way done!

![A website showing the 4 members of the Remix-Run team with their profile pictures, usernames, and links to their profile](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lns6t3fjw8e33ik1elvl.png)


### Creating Search Functionality
We want users to be able to search and put in any organization name on GitHub and see a list of members. We are going to use the Remix 'Form' for this along with `Redirect` and their `Action` function. Our search is going to accept input from the user, then redirect them to a new page and display the results, it will also work purely on routing by looking at the URL parameters. 

In our Index.jsx file under (`/app/routes/fetch/index.jsx`) let's update our imports to include `Form` and `redirect`. 
`import { Form, useLoaderData, redirect } from "remix";`

Perfect, now let's setup our Form that the user will see. In the return section, let's add this right under Hello Fetch, but before our data renders. We will create a Form, add a label, add an input text field named search, and a button to submit the form. 

Update your return section as follows
```javascript
export default function Fetch(){
    let members = useLoaderData();

    return (
        <div>
            <div>
            <h1 className="text-xl text-red-500 my-5">Hello Fetch</h1>
            <Form className="search" method="post">
                <label htmlFor="search">Search:</label>
                <input type="text" name="search" id="search"/>
                <button type="submit" className="bg-blue-200 m-2 p-2 rounded hover:bg-blue-500">Search</button>
            </Form>
            </div>
            <div>
                <div className="columns-1 md:columns-2 lg:columns-3">
                   {members.map(member => (
                    <div key={member.login} className="flex flex-col w-full border-solid border-2 border-blue-400 rounded-lg px-2 py-2 my-0.5">
                        <img src={member.avatar_url} className="w-10 h-10 rounded-full" />
                        <h3 className="text-1xl font-semibold">User: {member.login}</h3>
                        <a href={member.html_url}>Github Profile: {member.html_url}</a>
                    </div>
                    ))}
                </div>
            </div>
        </div>
    )
}

```

Awesomesauce. Now we need to setup our Action so it knows what to do when the user submits our form. 

Our Action is going to extract the form data from our serialized request and get the value of the "search" text field. We are then going to use that with Redirect to send our user to the results page

At the top of the same index.jsx file (`/app/routes/fetch/index.jsx`) add the following action function below our existing loader function. 

```javascript
export let action = async ({request}) => {
    //When a user searches, the form data will be submitted as formData in the request
    // we will look in there for the form field "search" and obtain it's value for the redirect
    const formData = await request.formData();
    const searchCompany = formData.get("search")
    return redirect(`/fetch/${searchCompany}`)
}
```

Now we have the ability to search, it's time to setup the route where the redirect is sending us. 

For our Search function, we are going to setup a parameterized route, this means our file name will begin with a $ and will act as a variable for fetching data from our GitHub module. 

In your (`/app/routes/fetch`) folder, create a file called `$search.jsx`. Be sure to include the $. 

Our Search file will be a more condensed version of our fetch index. We are again using the Remix loader function, but this time we are going to look at the URL parameters, and thanks to parameterized routing, we have a URL param named search that we can pass to our GitHub module to fetch data. We will then render that using the `useLoaderData()` function. 

Update  your `$search.jsx` file as follows:
``` javascript
import { useLoaderData } from "remix";
import { getMembers } from "./github";

export let loader = async ({params}) => {
    let res = await getMembers(params.search);
    return res;
}

export default function Search(){
    let members = useLoaderData();
    return (
        <div>
                <h1 className="text-xl text-red-500 my-5">Hello Search</h1>
                <div className="columns-1 md:columns-2 lg:columns-xl">
                    {members.map(member => (
                    <div key={member.login} className="flex flex-col w-full border-solid border-2 border-blue-400 rounded-lg px-2 py-2 my-0.5">
                        <img src={member.avatar_url} className="w-10 h-10 rounded-full" />
                        <h3 className="text-1xl font-semibold">User: {member.login}</h3>
                        <a href={member.html_url}>Github Profile: {member.html_url}</a>
                    </div>
                    ))}
                </div>
        </div>
    )
}
```
Your App should now look like this with the search form:

![A website showing a search field, submit button and the 4 members of the Remix-Run team with their profile pictures, usernames, and links to their profile](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k7w513lms3bgivsszh94.png)

Let's give it a go, open your app and search for "microsoft" and press Search. You should be redirected and get a result similar to this:


![A website showing all of the Microsoft GitHub members with their profile pictures, usernames, and links to their profiles](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2de775qs5vlqktg1obrd.png)

Perfect, your app is now complete! The rest of this tutorial will go over some network tab information and looking at how this content is Cached. 

## Let's look at the network tab for caching
If you pull up your developer tools and look at the network tab. You can see that your fetch route is now pulling in the images from memory cache instead of fetching them from the server. The same for our CSS file, and most of the JavaScript is coming from our disk cache. Keep in mind this is all localhost and the experience would be slightly different if hosted on the web. 

![Network tab showing waterfall of items downloaded as page loaded pulling from cache](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/f6783kbhbfno79thjamw.png)

Let's look at a bigger one with the Microsoft search

![Network tab showing waterfall of items downloaded as page loaded pulling from cache](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/i7p061i53dflyffgrjl4.png)

Now let's look at a organization we haven't loaded, I'm going to choose Nasa. Here we can see that our Style is still pulling from Cache and it's loading in all of the images. As the page loaded, there was a brief pause and then all the content was loaded at once. 

![Network tab showing waterfall of items downloaded as page loaded showing the progression of download](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/az1hclolcafn0m0ixoum.png)

I turned on Slow 3G and searched "vercel". All of the columns were populated with Users and Profile links, while the images took a bit longer to download, however on the user experience, loading the usable content first creates a better experience. 

![Network tab showing waterfall of items downloaded as page loaded showing a faster render of javascript files and slow image download](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/w7qzjfln09v0benz29w1.png)

Now that it's loaded, if the user refreshes, all of the previous unchanged content will pull in from cache which will give a much more responsive experience if the user has visited the page before or navigates back to this page. 


![Network tab showing waterfall of items downloaded as page loaded showing a must faster render using cache](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/orm76s91k7bolpciccba.png)


## Conclusion
Remix is an amazing web framework that makes it quick and easy to pull data into your site and render that content, it can be done in very few lines of code. It's also quite simple to submit new requests for data and have that rendered. The built in caching functionality greatly improves user experience but simultaneous downloading of content ensures the user has functional content before their network downloads the larger bandwidth required data. 