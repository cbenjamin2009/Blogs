# Tutorial: Add Sitemap.xml and Robots.txt to Remix Site

### Purpose
I recently converted my website to Remix and needed to setup my Robots.txt file and Sitemap.xml for Google Search Console and SEO in general in the Remix method for serving up files. 

This process was a bit different from previous static generated sites where I created and added the files to the build. In Remix, you must use their loader function and return the content in a specific format. 

Examples: 
- [My Live Sitemap.xml](https://www.taco-it.com/sitemap.xml) 
- [My Live Robots.txt](https://www.taco-it.com/robots.txt)

This tutorial will guide you on a very basic Robots.txt and Sitemap.xml file for a Remix site. I am not generating or creating the content of my Sitemap dynamically, I am using static content at this time. 

This tutorial assumes you already have a Remix app setup such as using the `npx create-remix@latest` bootstrap method.  

This tutorial covers the JavaScript method, but using this for TypeScript would only require a few changes on the loader function for importing the type, the Remix documentation convers this nicely at the link below. 

### Remix Resource Routes
Remix uses Resource Routes to serve files via Get, Push, Put, Post, Patch, and Delete. These are not UI (User Interface) routes and they will not render the rest of the UI components when the route is loaded. 

These Resource Routes are great for Sitemap.xml, Robots.txt, dynamic created files such as PDF's, webhooks for 3rd party services, and many more services. For full documentation, head over to the Remix Docs and read about  [Resource Routes](https://remix.run/docs/en/v1.1.1/guides/resource-routes). 

### Sitemap.xml Setup
For the Sitemap.xml we need to create a special file in our `routes` folder. Since we want the period (.xml) as part of our actual route name, we will have to escape it so that Remix will allow it to be part of the route. 

Create a new file:
This can be done 1 of 2 ways, either escaping just the period character or the whole file name. 

- Period Escape: `sitemap[.]xml.jsx` 
- Full Escape: `[sitemap.xml].jsx` 

This file will only contain a remix loader which will return a Response with our content. Below I will show both the JavaScript and TypeScript methods. 

In the sitemap file that you added under routes. We are going to add a basic Remix Loader. This example includes a single URL in the list pointing at my business website, you would replace the url content with your own sitemap which should contain multiple URL's unless it's a single page app. 

Add the following content:
```javascript
export const loader = () => {
  // handle "GET" request
// separating xml content from Response to keep clean code. 
	const content = `
		<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
	<url>
	<loc>https://www.taco-it.com/</loc>
	<lastmod>2022-01-08T00:15:16+01:00</lastmod>
	<priority>1.0</priority>
	</url>
	</urlset>
	`
	// Return the response with the content, a status 200 message, and the appropriate headers for an XML page
	return new Response(content,{
	  status: 200,
	  headers: {
		"Content-Type": "application/xml",
		"xml-version": "1.0",
		"encoding": "UTF-8"
	  }
	});
};
```

Perfect, now you will want to run your site `npm run dev` and make sure that your sitemap is rendering when you browse the route  [http://localhost:3000/sitemap.xml ](http://localhost:3000/sitemap.xml ) 

You should see something like this: 

![XML File layout for a sitemap.xml file based on the code example above](https://cdn.hashnode.com/res/hashnode/image/upload/v1642617078425/enqZk4Niy.png)

### Robots.txt Setup
The Robots.txt setup is going to be pretty similar to the Sitemap.xml file, instead we are serving up plain text and not XML content. 

Create a new file:
This can be done 1 of 2 ways, either escaping just the period character or the whole file name. 

-Period Escape: `robots[.]txt.jsx` 
-Full Escape: `[robots.txt].jsx` 

Sweet, now we just need to add our loader and return content for the Robots.txt file. 

*Note this is a basic Robots.txt file as copied from Google Search Console and updated with my Sitemap URL, you will need to generate your own Robots file with appropriate settings, and update your Sitemap URL. *

```javascript
export const loader = () => {
// handle "GET" request
// set up our text content that will be returned in the response
    const robotText = `
    User-agent: Googlebot
    Disallow: /nogooglebot/
    
    User-agent: *
    Allow: /
    
    Sitemap: http://www.taco-it.com/sitemap.xml
    `
  // return the text content, a status 200 success response, and set the content type to text/plain 
	return new Response(robotText,{
	  status: 200,
	  headers: {
		"Content-Type": "text/plain",
      }
	});
};
```

Sweetness, you should now have a Robots.txt route in your app. 

Run your site `npm run dev` and make sure that your robots file is rendering when you browse  [http://localhost:3000/robots.txt ](http://localhost:3000/robots.txt ) 

You should see something like this: 

![The text content of the robot.txt file as entered above](https://cdn.hashnode.com/res/hashnode/image/upload/v1642617403820/ZQyb44KW-.png)


## Conclusion
You should now be able to add your Sitemap.xml and Robots.txt files to your Remix website so you can begin the journey of implementing SEO and SEM to get your Remix site to show on search engines. 

*Note: Additional research is needed into setting up a proper Sitemap.xml and Robots.txt file. This is not a one size fits all solution, and I do not recommend using these basic settings for all websites.*