---
layout: page
title: How controllers are specified in the Swagger file
---

# Add controller logic to your API

After downloading the sample project from Apigee API Studio, the first thing to do to get a functioning API up and running is to **write a controller**.

A controller is a Node.js implementation with functions that execute when an API path is called. So, for a "hello world" API, you'll need a controller function that returns "Hello, World!" when you call the API. This topic explains how.

# How controllers are specified in the Swagger file

You can see immediately how the controller is specified on the `/hello` path by looking at the project's Swagger file:

{% highlight yaml %}
    paths:
      /hello:
        x-swagger-router-controller: hello_world
        get:
          description: Returns greetings to the caller
          operationId: hello
{% endhighlight %}

The extension `x-swagger-router-controller` specifies the name of the Node.js controller file associated with the `/hello` path. And the `operationId` specifies the name of a function to call in that file when `/hello` is called.

# Where controller files are located

Controller files are always located in a `swagger-node` project in the directory `<project-root>/api/controllers`.

For example: `<project-root>/api/controllers/hello_world.js`

**Note:** In the Swagger file, the name of the controller is given without a `.js` extension.

# Try it

Let's add a "hello world" controller and call the API.

Remember: According to the Swagger file, the API's `/hello` path executes the `hello()` method in the controller file called `hello_world.js`.

1.  Before we add the controller, we need to make a small adjustment to the Swagger file. In an editor, open `<project-root>/api/swagger/swagger.yaml`.
2.  Edit the `basePath` element, like this, with just a single slash:  

    `basePath: /`
3.  Save the Swagger file.
4.  In an editor, create the file `<project-root>/api/controllers/hello_world.js`
5.  Copy this code into the file:

           var util = require('util');

           module.exports = {
             hello: hello
           };

           function hello(req, res) {
             var hello = "Hello, World!";
             res.json(hello);
           }

6.  Save the file.
7.  In a terminal window, CD to the project's root folder.
8.  Use `npm` to install the dependencies for the project:`  

    npm install`
9.  Start the project: `  

    swagger project start`
10.  In another terminal, call the `/hello` API: `  

    curl -i http://localhost:10010/hello` `Hello, World!`

You've created a simple controller that executes when your API path is called.

# Specify input with a query parameter

Let's add a query parameter to the API. We'll use it to pass in a name.

**Step 1:** Add the `parameters` element to the Swagger file:

      paths:
        /hello:
          x-swagger-router-controller: hello_world
          get:
            description: Returns greetings to the caller
            operationId: hello
            parameters:
              - name: name
                in: query
                description: The name of the person to whom to say hello
                required: false
                type: string

**Step 2:** Add controller code to handle the query parameter. Open the `hello_world.js` file and replace the contents with this code:

          var util = require('util');

          module.exports = {
            hello: hello
          };

          function hello(req, res) {
            var name = req.swagger.params.name.value;
            var hello = name ? util.format('Hello, %s', name) : 'Hello, stranger!';
            res.json(hello);
          }

**Step 3:** Call the API

Now, when you call the API and pass in a `?name` query param, the name is echoed back.

`curl http://localhost:10010/hello?name=Scott` `  
Hello, Scott`

**Note:** We use the `req.swagger` object to obtain access to the query parameter value. This object is populated by a middleware component called `swagger-tools`. To read more, see the [Swagger tools middleware documentation](https://github.com/apigee-127/swagger-tools/blob/master/docs/Middleware.md).

# Next step

Read more about implementing controllers in the [swagger-node documentation](https://github.com/theganyo/swagger-node/tree/master/docs).
