# Deploying Open Commerce

Open Commerce is an open source eCommerce platform suitable for use in production. All services are either publicly available Docker images on DockerHub or are Git repositories that include a `Dockerfile` so that you can build your own Docker image after customizing the service. This means that you can deploy the Open Commerce system on any infrastructure that supports running a Docker container cluster with a shared network.

## Deploying Open Commerce on Digital Ocean

The following guide is intended for users that are in the evaluation phase of Open Commerce. It is not meant to be a production
grade deployment, rather a simple guided tutorial that will provide insight on how the platform works together. With that said,
the concepts covered in the guided can be used to build a production grade deployment.

[Deploying Open Commerce on Digital Ocean using Ansible and Traefik](https://github.com/reactioncommerce/proxy-traefik)

## Deploying Open Commerce with Kubernetes via Helm

[Kubernetes](https://kubernetes.io/) is a technology created by Google for orchestrating containers in production. While Kubernetes
(often referred to as K8) is a powerful tool, it can also be complex and daunting. To make this easier Open Commerce offers Helm
charts that should work with most major cloud providers. Complete instructions on how to utilize these charts are available [here](https://github.com/reactioncommerce/mailchimp-open-commerce-helm-chart/tree/develop/docs/cloud-deploy).
If you want to try out deploying Open Commerce via these charts on a Mini version of K8, the instructions for that are available [here](https://github.com/reactioncommerce/mailchimp-open-commerce-helm-chart/tree/develop/docs/local-deploy)

> **Note**: It is important to understand that Open Commerce provides these tools for your convenience and to help you get started but
we do not and cannot provide any sort of support. It is up to each implementor to ensure that these tools are appropriate and well-understood.

## Docker Images

You can find all the published Open Commerce images [on our DockerHub page](https://hub.docker.com/u/reactioncommerce).

## Container Requirements

We recommend that you make at least **2GB of memory** available to each container.

## MongoDB Database

In a development environment, a local MongoDB database is created for you. In production, you will need to set up a separate production-ready MongoDB server and create a database in it. You then provide this database connection string in an environment variable for services that need to connect to it.
Since some services use change streams, your MongoDB server must be part of a replica set and must have oplog enabled. Due to the complexity of operating
and managing a production database at scale, we recommend we use a hosting provider like [Atlas](https://www.mongodb.com/atlas)

> Don't forget to set up periodic database backups. Do them as often as you can afford to do.

## Database Migrations

When you deploy, the services will not start properly unless your MongoDB databases are on the expected data version. Use the Open Commerce migration tools to migrate your data up or down before deploying. In some cases, migrating down is not possible. If you are rolling back to a previous Reaction version and migrating down is not possible, you will need to restore from the last backup prior to the upgrade.
