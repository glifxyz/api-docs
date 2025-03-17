# 📫 Sample code

#### Example code and apps

**NextJS**

Here is an example NextJS app that has its own local /api/glif API endpoint:

[GitHub - glifxyz/glif-api-demo](http://github.com/glifxyz/glif-api-demo)

**Simple HTML + CSS (Replit)**

Here is a simple pure HTML + CSS + vanilla javascript app hosted on Replit that you can fork and run right there: [https://replit.com/@jamiedubs/FishShop](https://replit.com/@jamiedubs/FishShop)

**Cloudflare Worker**

Since that repl app is pure HTML and doesn’t have its own API, we’re using a Cloudflare Worker serverless function to handle talking to the Glif API w/ CORS headers set:

[https://gist.github.com/jamiew/96adcc968210a78bf7ca9e0c2d299dbd](https://gist.github.com/jamiew/96adcc968210a78bf7ca9e0c2d299dbd)

The “Simple API” above is basically just this code with a shared API token.
