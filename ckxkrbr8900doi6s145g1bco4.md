# Convert React Site to Remix

This article will cover the transition of my Single Page App (SPA) created in React with TailwindCSS styling to a Remix site with TailwindCSS 3.0 and nested routing. I am performing this transition to take advantage of Remix routing and blazing fast content delivery with server side rendering. I also plan to take advantage of some prefetching when the user navigates to one of the available routes. Since my business is growing, I plan to be adding more content and eventually authentication for clients. My site is currently hosted on Amazon AWS Amplify and I will also be moving it to Vercel. 

If you have a current React site and wish to convert it to Remix this would be a great article to review. 

![React to Remix Blog Image](https://cdn.hashnode.com/res/hashnode/image/upload/v1640372197958/ndhCV-Ubd.png)

## Converting my React Website into Remix
It's been a month since Remix v1.0 came out and was publicly available, and ever since then I've been wanting to redo two of my websites that are hosted on Vercel to use Remix. This blog article will review the process that I took to convert one of my websites to Remix. 

### Summary
We are going to open our existing website project, create a new branch for creating our Remix site. We are then going to create a Remix App in a folder inside our current website and move our site contents into the Remix project folder. At the end we will remove the previous parent folder and ensure our Remix app is our primary site. I'll be performing a Pull Request on GitHub to merge the branch to main branch. 

### Initial Setup
1. Open GitHub Repo in VSCode or clone repo
2. Create new Branch - "Remix"
3. Run `npx create-remix@latest`
4. I chose to create a new directory `taco-it_remix` and chose Vercel hosting. I also allowed remix to run npm install so our base Remix site will be functional. 
![Terminal showing Create Remix App commands and options chosen](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xt5ewgjv06otxxi67pq7.png)

### Copy Files
1. Copied my Components and Images folder from React side to Remix `/app/` folder (`/src/Components` & `/src/Images`) to (`/taco-it_remix/app/Components` & `/taco-it_remix/app/Images`)
2. .githubignore File
     1. Open `.gitignore` file and copy contents into `/taco-it_remix/.gitignore` file
3. Created a `styles` folder under `/taco-it_remix/app/styles` and copied (`/taco-it_remix/app/Components/WallPaper.css`) into `/taco-it_remix/app/styles/WallPaper.css`. 
4. Package.json file
     1. We need to copy our dependencies from our Package.json file and also update our Remix Package.json file. 
     2. I opened my React Package.json file and copied my dependencies that will be needed in the Remix website (emotion/react, emotion/styled, material-ui/icons, mui/icons-material, react-typewriter) to my Remix Package.json file. 
     3. I ran `npm install` to install all my dependencies

### TailwindCSS 3.0 Setup
Since the React version of my website was using a previous version of TailwindCSS, I want to enable TailwindCSS 3.0 functionality on this Remix version of my website so we are going to set that up so all of our existing styles will still apply. 

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

export let links = () => {
  return [
    { rel: "stylesheet", href: tailwindstyles }
  ];
};
```

Perfect, TailwindCSS is all setup in our Remix app and our styles will be applied. 

## Index Route Setup
In the previous React website, the Index route was served by `index.js` in the root of the `src` directory. In Remix, we are going to use our `/app/routes/index.jsx` file to render our home page. I'm going to import my HomePage.js component into my Remix index.jsx file which will render all of my sub-components. This index file is rendered after our Root.jsx file so it will house all of our root route `('/')` data. 

1. Index Route Setup
     1. Opened routes/index.jsx and deleted everything
     2. Imported HomePage Component
     3. Created default Index() function to return my HomePage Component. 
     4. Added Route to render my HomePage component which contains all other components
```javascript
import HomePage from "~/Components/HomePage";
export default function Index() {
  return (
    <>
    <HomePage />
    </>
  );
}
```
2. Custom wallpaper and css file setup
I use a custom css file on my site to create the wallpaper effect on the home page, here's how I set that up in Remix. 
     1. Copy /Components/WallPaper.css into /App/Styles/WallPaper.css
     2. Update Index to import the JSX and CSS files. We will import the CSS using the Remix links component so it appends our stylesheet only on our index page, and it will not apply to nested routes. 
```javascript
import HomePage from "~/Components/HomePage";
import WallPaper from "~/Components/WallPaper"

import WallPaperStyles from '~/styles/WallPaper.css'

export const links = () => {
    return [
        {
            rel: "stylesheet",
            href: WallPaperStyles
        },
    ]
}

export default function Index() {
  return (
    <>
    <WallPaper />
    <HomePage />
    </>
  );
}

```

Awesome, now we should be able to run `npm run dev` and be presented with our site. 

For the rest of the cleanup steps I'm going to remove the previous React items in the folder and move my Remix site to be in the parent folder only. 

## Site Swap Conclusion
At this point our website should be pretty much done. In Summary, I created a new Remix site using `npx create-remix@latest`, I copied our existing React code to the correct locations, we setup TailwindCSS for Remix, and updated our Index.jsx file to render our HomePage component and now the site is working. Run `npm run dev` and verify site functionality. Once it was working, I pushed to Github for site generation. 

Next I will continue adding routes and additional content as future site updates now that it's running on Remix. 

_Note:_ I experienced an issue with some of my Material-UI icons not wanting to render. I was able to resolve this by modifying the dependencies to latest version of MUI and Emotion. 



