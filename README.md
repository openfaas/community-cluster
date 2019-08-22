# community-cluster

Sign-up for the OpenFaaS Cloud Community Cluster for git-based deployments and free TLS endpoints. The Community Cluster is free to use.

The OpenFaaS Cloud Community Cluster comes with:

* Deep GitHub integration & CI/CD. Just `git push`
* Public endpoints with free, automatic TLS
* Personal dashboard with OAuth2 / MFA
* Auto-scaling and metrics
* Private and public repos supported

Preview of the dashboard:

![Personal dashboard](https://www.openfaas.com/images/openfaas-cloud-gitlab/dashboard.png)

![Details for function](https://www.openfaas.com/images/openfaas-cloud-gitlab/details.png)

## 0. Agree to the terms & conditions

By applying you have read and agree to the [terms and conditions of the Community Cluster](https://github.com/openfaas/openfaas-cloud/blob/master/PRIVACY.md).

## 1. Apply for access

First off you should [apply for access](https://forms.gle/8e6ZXJKMcDHpV6Xu6). All applications will be reviewed, so please fill them out accurately and make sure you agree to the terms and conditions of the platform.

> Note: You will also be [invited to OpenFaaS Slack](https://docs.openfaas.com/community/), log-in and join the #openfaas-cloud channel.

## 2. After you are approved for access

Once you've had an email confirmation you can go ahead and get started [with enabling your account](/docs/).

* The first step is to send a PR to the `CUSTOMERS` file with your GitHub username or GitHub organization.

Please read the instructions very carefully, this change needs to be made via the `git` CLI or if you are using the UI, you need to type in a specific commit message. You'll receive details for this in 2.1.

* Second you will install the OpenFaaS Cloud GitHub App integration onto your chosen repo(s)

Please do not install the app on every repo. You can have more than one repo integrated with OpenFaaS Cloud, but you should select each one individually.

* Third you can push code created with `faas-cli new` into your repo

* Fourth - you'll be able to access your dashboard for metrics, endpoints and feedback

### 2.1 Activate your access

Activate your access now with the [quickstart](./docs/)

## 3. DIY / self-service

If you'd like to spin up your own OpenFaaS Cloud for your team, or for development you can use the [ofc-bootstrap](https://github.com/openfaas-incubator/ofc-bootstrap) tool.
