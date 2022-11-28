# Remix TailwindCSS Tutorial - Super Simple Setup

## TailwindCSS 3.0 Setup with Remix
This is a super quick tutorial to get TailwindCSS up and running in Remix!  I wrote this when TailwindCSS3.0 was released on Remix V1.1.1


![Remix logo, plus symbol, Tailwind logo](https://cdn.hashnode.com/res/hashnode/image/upload/v1642196481303/i8CvMXYmA.png)

This tutorial assumes you have already created your Remix app using the `npx create-remix@latest` command, if not, do that first then follow these steps. 

Open your terminal and let's install TailwindCSS

`npm install -D tailwindcss`

We also need Concurrently for running TailwindCSS in our dev environment. 

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

Now when we run `npm run dev` it will generate a tailwind.css file in the root of our /app/ folder. We need to tell Remix that want to use this stylesheet. I'm going to set this up in our `Root` file so that TailwindCSS styles are imported to the entire site. Remix does this by importing our styles and using their links function to apply the stylesheet to the head of the HTML file. 

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

Perfect, TailwindCSS is all setup in our Remix app!!! 

Go forth and style beautiful apps and sites with amazing user experiences, because that's what Remix is all about!
