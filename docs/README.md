## OpenFaaS Cloud Community Cluster

Activate your access.

### How does it work?

The Community Cluster is fully hosted so all you have to do is select a GitHub repo and *click Install* using the GitHub UI. Then just `git push` your OpenFaaS functions into a public or private GitHub repo. Within seconds you'll get your own public HTTPS endpoints with a personalised dashboard and commit statuses reported back directly to your GitHub account.

OpenFaaS Cloud is portable which means you can host your own anywhere where you have OpenFaaS deployed, but that's [another guide](https://github.com/openfaas/openfaas-cloud/tree/master/docs).

![Personal dashboard](https://www.openfaas.com/images/openfaas-cloud-gitlab/dashboard.png)

![Details for function](https://www.openfaas.com/images/openfaas-cloud-gitlab/details.png)

## Introducing OpenFaaS Cloud

[OpenFaaS Cloud](https://github.com/openfaas/openfaas-cloud) is a multi-user platform for delivering OpenFaaS to your users, your community or your team.

Features:
* Personal UI dashboard for your functions (per user)
* Store your functions in a private or public GitHub repo
* CI/CD triggered by `git push` with build/test logs available
* Built-in integration with GitHub statuses and commits
* Integration via GitHub App with fine-gained OAuth permissions

These instructions apply to the OpenFaaS Cloud Community Cluster. This cluster runs on Kubernetes and supports the use of SealedSecrets.

[Fork/Star the Github repo](https://github.com/openfaas/openfaas-cloud) for the conceptual architecture diagram and for more screenshots.

## Quick start (5-10 minutes)

This quick-start takes around 5-10 minutes and doesn't require you to setup Docker or other tooling on your computer. You'll only need access to `git` and the OpenFaaS CLI.

### Request access by Pull Request

1. Agree to the [Terms & Conditions and read the privacy statement](https://github.com/openfaas/openfaas-cloud/blob/master/PRIVACY.md)

2. Send a PR to the [CUSTOMERS](https://github.com/openfaas/openfaas-cloud/blob/master/CUSTOMERS) file with your GitHub username. Add your username to the end of the file.

If you edit the file in the UI, you need to sign-off your commit by entering this text manually in the commit Description.

```
Add your-username-here to CUSTOMERS file

Signed-off-by: My Full Name <my.email@address.com>
```

If you send the PR via the `git` CLI then type in `git commit --signoff` to sign-off your commit and use the message as per above. If you've never signed-off a commit you may need to run `git config user.name "Your Name"` and `git config user.email "your-email"`.

In the Pull Request you'll need to say how you found out about OpenFaaS Cloud and what you'd like to run there. Early access to the Community Cluster is limited and requests to join will be reviewed.

### Create a GitHub repo for your OpenFaaS functions

3. Create a new GitHub repo and clone it - both public and private are supported

Enter the directory of your new git repo with `cd folder`

4. Get the FaaS-CLI from `brew install faas-cli` or `curl -sLSf https://cli.openfaas.com | sudo sh`

5. Create at least one function using a language like `python`, `node` or `go` for instance:

```
faas-cli template pull
```

Pick a language from the list below:

```
faas-cli new --list
```

You don't have to pick Go, but I'll do that in this example. If you'd like to try Node.js and Express.js, try the [New User Guide](https://docs.openfaas.com/openfaas-cloud/user-guide/).

> Note: see appendix for additional templates available on the community cluster.

6. Create a function locally and for the `--prefix` variable use your GitHub user account, so mine would be:

```
faas-cli new helloworld --lang go --prefix alexellis
```

If you want to customize the code edit `./helloworld/handler.go` or read the docs to learn [how to vendor a dependency with OpenFaaS](https://docs.openfaas.com/cli/templates/#10-go).

7. Rename the `.yml` file to `stack.yml` so that OpenFaaS Cloud knows where to find your function(s)

### Add the OpenFaaS GitHub App

8. Install the GitHub App on your repo:

Please *only do this for any OpenFaaS repos you create*, not for "all" repos. 

> If you do this for "all" repos then we will try to build any Git repo that you push changes into which will be wasteful and useless.

https://github.com/apps/openfaas-cloud-community-cluster

This connects GitHub push events to OpenFaaS cloud so that we can build and deploy your function as soon as you push code.

9. Now commit the changes and push them to the remote side.

```
git commit -s -m "Initial function"
git push origin master
```

### View the build/deployment feedback

9.1. View the Commit Statuses / Checks

Navigate to your GitHub repo and click on the "Commits" link - this will open up a list of all your commits. Here you'll see a Tick or X next to each commit status showing the results of the container builder and deployment.

To get the build logs and more information you can click on any of the names of the functions - this uses the new GitHub Checks feature to show detailed information.

9.2 Add more functions

You can have one or many functions in your stack.yml file.

To append new functions type in:

```
faas new <name-of-new-function> --lang go --prefix alexellis --append=./stack.yml 
```

### Log into your personal dashboard

10. Your dashboard

Your dashboard provides build logs for failed builds, unit test errors and will give a live link to your endpoint.

View your dashboard at https://system.o6s.io/dashboard/alexellis replacing *alexellis* for your own username.

You will be asked to Log in with GitHub for Single-Sign-On (SSO) - after that you will be issued a JWT token and cookie which is stored in your browser for further requests.

11. Your function endpoints

Your functions will be available with HTTPS on your own sub-domain, for example:

```
https://alexellis.o6s.io/hallo
```

The above username is "alexellis" and the function name is "hallo"

You can now use your public endpoint for whatever you need. You can read HTTP meta-data like the query-string, headers and path. [Read more in the Workshop](https://github.com/openfaas/workshop/blob/master/lab4.md#inject-configuration-through-environmental-variables).

### Rinse & repeat

12. Iterating on the code

Each time you run `git commit -s` and `git push` a new CI/CD build will be initiated and your functions in the given repo will be discovered and redeployed. If the unit tests fail or the container can't start then the previous version will remain up and running.

13. State for your functions

You can make use of any managed service to store state such as AWS S3 or DBaaS (database as a service) from a remote provider. The Appendix contains instructions on how to provide keys, credentials or other secrets to your functions in a secure way.

14. Remove one or more of the test functions

If you want to remove one or more of your functions you can either remove them from your `stack.yml` file (garbage collection will remove them for you). Or you can remove the GitHub App integration - if you have multiple repos just uncheck the ones you want to delete.

### What else do I need to know?

15. Tell me more about privacy

* Your dashboard

At this point your dashboard can only be viewed by other OpenFaaS Cloud Community Cluster users in the CUSTOMERS file.

* Your endpoints

All your functions will available via your public URL, so if you want to implement authentication you can do this however you'd like. Some options may be: by using HMAC and a symmetric key, basic authentication or an API key. For any of those options you will want to use secrets as described in the appendix.

* Your Docker images

Your Docker images will be published under the shared account ["ofcommunity" on the Docker Hub](https://hub.docker.com/r/ofcommunity/) - this means you can run your images locally for debugging and testing.

15. Auto-scaling

Your function will auto-scale up to a maximum of 4 replicas given a steady flow of traffic.

See the appendix to learn how scale to and from zero works.

16. Get involved!

Now it's over to you to Tweet, write a blog, [join Slack](https://docs.openfaas.com/community) and tell your friends. You can also Star the project, pick up an open issue, make a suggestion or contribute via GitHub: https://github.com/openfaas/openfaas-cloud/

You can also try the [New User Guide](https://docs.openfaas.com/openfaas-cloud/user-guide/) which walks you through building a Node.js Express.js app / API with detailed screenshots and step-by-step instructions.

## Over to you - time to build something

Now that you have the basics in-hand it's over to you to build something and report back on how you found the experience so we can continue to improve the platform.

* Write a simple blog using the node8-express or golang-middleware templates (see the appendix for more)
* Write a Slack bot or auto-responder - here's the [Welcome Bot for OpenFaaS written in Python 3](https://github.com/alexellis/my-fn/tree/master/join-welcome)
* Connect to any data-source you like by using the Incoming Webhook app on [IFTTT.com](https://ifttt.com/)
* Write your own Alexa skill - use my [Dockercon skill as an example](https://blog.alexellis.io/serverless-alexa-skill-mobymingle/)
* Write a URL shortener - by using HTTP redirects you can take a URL such as https://openfaas.o6s.io/signup and have it redirect to any site. You can even use HTTP parameters or query-strings. See [my example](https://github.com/openfaas/cloud-functions)
* Deploy a GitHub badge generator - through the use of parameters or query-strings you can generate dynamic GitHub pages, images or SVGs - [ee an example here](https://github.com/faasinating/faas-cloud-badge)
* Store logs / data or notes using S3 as a backing datastore - [see an example here](https://github.com/openfaas/openfaas-cloud/tree/master/pipeline-log)
* Build a CRUD API using Mongo as a Service via [Atlas Mongodb](https://www.mongodb.com/cloud/atlas) or a similar provider
* Integrate with any OAuth 2.0 provider by building your very own app. How about [Strava](https://developers.strava.com/) or [Monzo Bank](https://monzo.com/)?

Or if you're really running out of ideas you can create a function that responds to webhooks from GitHub like we do in the [OpenFaaS workshop](https://github.com/openfaas/workshop). 

## Appendix

### Secrets

You can make use of access tokens or other secrets through the use of encryption and SealedSecrets. Follow [this guide](https://gist.github.com/alexellis/46099ff77408d93944ee33fff66b240e) to try it out.

### Scale-from/-to zero

Auto-scaling can scale your function down to zero replicas too and then you will incur a cold-start. This may or may not be an issue for you depending on the workload of choice.

To override scale to zero add the following to `stack.yml`

```yaml
functions:
  example:
    labels:
      com.openfaas.scale.zero: false
```

### Templates

Both the official templates are available along with a set of additional templates.

Standard templates:

```
- csharp
- go
- java8
- node
- php7
- python
- python3
- ruby
```

Additional templates:

* [node8-express](https://github.com/openfaas-incubator/node8-express-template)
* [node10-express](https://github.com/openfaas-incubator/node10-express-template)
* [golang-http](https://github.com/openfaas-incubator/golang-http-template)
* [golang-middleware](https://github.com/openfaas-incubator/golang-http-template)

Use `faas-cli template store pull` to fetch one of the additional templates listed above

For a list of official templates type in `faas-cli new --list`.

> Note: the `dockerfile` type is restricted from use in the Community Cluster. If you host your own OpenFaaS Cloud cluster, then you can select whichever templates you want.

If you need additional templates to be made available, please let us know.

### Limits and constraints

Functions are limited to 128MB of memory usage. If you need more resources please get in touch.