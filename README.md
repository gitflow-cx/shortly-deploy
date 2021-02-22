## Shortly: Deployment
In this sprint, you will learn about basic deployment and various build tools. The tools and techniques you gain experience with here will allow you to kick off your group projects with a bang.

Deployment, at times, can be really painful. In particular, deployment bugs often take longer cycles to investigate than local code, which can feel very frustrating, and, deployment bugs may often feel like they are just nuances to someone else's tools that they did not take the time to document well. This is in fact often true, but it comes with the territory, and learning to enjoy the perhaps slower-seeming process will help you develop a set of tools that gives you tremendous power as an engineer.

In the spirit of deployment you will find many of the instructions given at a rather high level. It will be up to you and your pair to orient and figure out the details of how to do what needs doing.

## High level goals of this sprint
- Learn how to deploy on Heroku
- Learn how to deploy on DigitalOcean
- Feel some pain around deployment bugs and the pace of deployment
- Gain some rudimentary exposure to how build tools (like Grunt) can make your life easier
- Gain some exposure to MongoDB and Mongoose

## Bare minimum requirements

# Learn Heroku
Heroku is widely popular for good reason. Getting started with it is incredibly easy, and the documentation is just fantastic. Many developers and startups will begin with Heroku, allowing them to focus on rapid development, potentially moving on to more sophisticated deployment options only when the need arises.

Learning to get started with Heroku is not such an interesting challenge, but you should have it as a tool in your toolbelt. Expect to spend about an hour and no more working with Heroku.

Complete Getting Started with Node.js on Heroku

# Tests
You will find all tests in this repo are in a pending state. They are all written for MongoDB. Before you begin the MongoDB refactor later in the sprint, remove 'x' from each describe block in the tests.

This repo uses supertest (see the require block in test/ServerSpec.js), a powerful library for testing HTTP. Use it to write at least 2 additional tests for the application

## Set up a git remote for easy deployment
There are many services out there to help make it easy on you, the developer, to continually deploy newer versions of your code. Remember how easy git push heroku master was? You are now going to roll your own version of this for your DigitalOcean deployment by way of a somewhat deep dive into git. The following task will require that you balance getting what you need to know out of the resource vs. going down a rabbit hole.

the credentials to connect to Digital Ocean are the following:

- [INSERT DIGITAL OCEAN CREDENTIALS HERE]

For this sprint you will need to:

- Read How to Set Up Automatic Deployment with git with a VPS, up to the section titled Beta
- Having SSHed into your droplet, create some directory (not inside your cloned down code) to create a "bare" git repo, by issuing the git init --bare command from inside it
- Taking a lead from the article you just read, create and edit a post-receive file inside the hooks directory of the "bare" repo that sets the --work-tree option to the directory where you have cloned down your codebase, and the --git-dir option to the location of the "bare" repo that you created. Be sure to chmod +x the post-receive file you just created
- Again taking a lead from the article above, on your local machine, create a live remote inside your shortly-deploy repository that points to the "bare" repo in your droplet, as the root user
- Make some change to your local shortly-deploy repo on the master branch, commit the changes, and then git push live master. If you are successful the command line will indicate it, and, you should be able to observe the changes when SSHed into to your droplet


## Understand the Gruntfile
Building and deploying an app involves a number of important tasks that need to be performed in a certain order. When you're trying to rapidly prototype your app, this can become repetitive and is prone to error. Grunt is one of many super useful tools that can automate a wide variety of tasks for you.

Read about task runners like Gulp & Grunt then take a look at the gruntfile.js in your repo.

Now, if you take a look at the gulp file, you will see that some tasks have been registered already like:

- Starting the server using nodemon (upload task, line 107)
- Uglify your code before deployment (deploy task, line 115 that runs the build task, line 105)
- Run eslint before deployment. If eslint fails, the build process should exit (deploy task, line 115 that runs the test task, line 103)
- Run your Mocha tests before deployment. If any tests fail, the build process should exit (deploy task, line 115 that runs the test task, line 103)

One you took a closer look at the grunt file, change your npm scripts to the following then try running them.

      "scripts": {
        "start": "grunt build && grunt upload",
        "test": "grunt test",
        "lint": "eslint ./"
      }

## Refactor your database
In the previous sprint, our shortened links were stored using sqlite, a server-less database engine. Sqlite is great for many things, but is not the best choice in most cases for well-trafficked production sites for various reasons.

MongoDB, on the other hand, is one of the most widely used databases in production environments and offers the most popular alternative to using an RDBMS.

- Remove the 'x' from each describe block in the tests
- Refactor the app to use MongoDB and Mongoose, making the tests pass and checking your application locally. Be sure to use promises. If the built-in promises are too limiting for you, plug in your preferred library.
- If you need to install MongoDB on your laptop, follow the installation instructions.
- Assuming you used the MEAN droplet, your deployed DigitalOcean server already has a mongod instance running on it. Push your refactored code up to production and profit

## Advanced Content
Our advanced content is intended to throw you in over your head, requiring you to solve problems with very little support or oversight, much like you would as a mid or senior level engineer.

# Refactor server.js to use promises
Several routes in the server use nested callbacks. Refactor them all to use promises. Bluebird, a popular and performant promise library, is already included

# Use another cloud service (optional)
Deploy your site to another service of your choosing:
Azure
AWS is massive, and you should give it a try if ready to get in a little over your head. The best place to get started learning about AWS is by taking taking the AWS quickLab on EC2

## Nightmare Mode - Docker
Install the Docker Toolbox on your local machine
Use docker-machine (installed above) to create "machines" on DigitalOcean
Use a Dockerfile, with the node image as a base, to create a Docker image for your shortly web server
Use the mongo docker image to launch a mongo container, networking your shortly web server container to it
Compose the two containers and launch them both on DigitalOcean
Use your newfound Docker prowess to roll your own CD
