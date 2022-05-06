# Nuxt 3 app on Plesk Obsidian

This repo explains how to run your Nuxt 3 app on Plesk Obsidian.
It took me a couple of hours and a lot of Googling to figure this out. So if it helped you, please star it!

## Step 1. Creating a new domain in Plesk

Assuming you have already installed the Node.js extension for Plesk, you can create a new domain in Plesk.

## Step 2. Create the basic app structure

Create the following in the httpdocs directory:

- A new directory called `.output`
- A new file called app.js containing the following:

```js
(() => import('./.output/server/index.mjs'))();
```

## Step 3. Compile your Nuxt app

Run the following command in the root directory of your app:

```bash
yarn build
```

## Step 4. Deploy your app

Copy the contents of the `.output` directory to the `.output` directory of your domain that you just created.

## Step 5. Setup the Node.js server

In the Plesk Control Panel, go to the Node.js page.
Set the Document Root to `/httpdocs/.output/public`, and the Application Startup File to `app.js`.

## Step 6. Set the correct nginx settings

In the Hosting & DNS tab, click Apache & nginx Settings.
Disable Proxy mode and add the following to the Additional nginx directives:

```
location ~* \.mjs$ {
	types { }   default_type "text/javascript; charset=utf-8";
}

location /api/ {
	expires 1m;
	add_header Cache-Control "public, no-transform";
}
```
