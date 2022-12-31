### TextBelt Open Source (Deployable Edition)

[TextBelt Open Source](https://github.com/typpo/textbelt) is a REST API that sends outgoing SMS.  It uses a free mechanism for sending texts, different from the more reliable paid version available at https://textbelt.com.

Send a text with a simple POST request:

```sh
$ curl -X POST http://my_textbelt_server/text \
   -d number=5551234567 \
   -d "message=I sent this message for free with Textbelt"
```

`number` and `message` parameters are required.

## Local development / testing

first, clone this repo. then...

```
# 1. install dependencies
npm install # for reference, i used node 16.17.0

# 2. config your email inside lib/config.js (using Gmail as an example)
- host: 'smtp.gmail.com'
- auth: {  user:  'youraddress@gmail.com' }
- auth: { pass: make app-specific Gmail password: https://security.google.com/settings/security/apppasswords

# 3. run the server 
redis-server # should default to port 6379
node server/app.js # in a new terminal tab! keep redis running separately

# 4. try sending a text message
curl -X GET http://localhost:9090/text \
   -d number=<your-cell-phone> \
   -d "message=I sent this message for free with Textbelt"
```

you may prefer to change your Gmail credentials (step 2) to environment variables before pushing this code to a private repo on your own GitHub account.

```
git add .
git commit -m 'prepare textbelt for deploy'
git remote set-url origin https://github.com/yourusername/your_new_repo
```

## Deploying to your own server 

1. create a free account at [render.com](https://render.com). this is basically like Heroku.
2. make a "Web Service" from your dashboard or click New > Web Service from navigation
3. auth your GitHub and select the textbelt repository you created
4. name your app, e.g. textbelt, then set Environment to `Node`, Build Command to `npm install`, and Start Command to `node server/app.js`
5. select the free Instance Type
6. if you refactored `lib/config.js` to use environment variables, plug those in by clicking Advanced > Add Environment Variable
7. click Create Web Service at the very bottom

this will build and deploy your code. when it's done, click the URL generated by Render and you should see the "homepage" saying `I'm online!`.

finally, test your server:

```sh
curl -X GET https://your-textbelt-server.onrender.com/text \
  -d number=<your-cell-phone> \
  -d "message=A free text message with Textbelt on Render"
```

enjoy! and huge thanks to Ian Webster for creating TextBelt.
