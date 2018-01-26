---
layout: default
title: Writing Web or mobile applications
category: start
section: applications
index-order: 602
sidebar: sidebars/accordion-toc0.md
excerpt: Web or mobile applications that need to interact with deployed business networks should make calls to a REST API. The easiest way to create the REST API is to use the `composer-rest-server` to [**dynamically generate a REST API from the deployed business network**](./web.html).
---

# Writing Web or mobile Applications

Web or mobile applications that need to interact with deployed business networks should make calls to a REST API. The easiest way to create the REST API is to use the `composer-rest-server` to dynamically generate a REST API from the deployed business network.

In addition the `yo hyperledger-composer` command can be used to generate a skeleton Angular project that calls the `composer-rest-server` to interact with a business network.

Please follow the Developer Tutorial for an example of how to use the `composer-rest-server` and the Angular generator.

## Generating an Angular application from a Business Network Archive (.BNA)

The flow for building an Angular application is as such:

<img src="../assets/img/Angular Generator Flow.svg" style="border: none;" alt="Angular Generator Flow">

The reference instructions below are for if you already have a Business Network Archive and would like to build a skeleton Angular application, if you would like to fully understand how to develop a BNA and build an application from there please see the developer tutorial.

**Prerequisites**
You will need the Composer [development tools](../installing/development-tools.html) in order to run the Angular generator.

### 1. Start Fabric running on your local machine

If you have already run through installing the development tools you will have Fabric already installed.

Head to where your Fabric tools are saved and start up Fabric. Below is an example if you have used our development tools install guide:

```bash
cd ~/fabric-tools
./startFabric.sh
./createPeerAdminCard.sh
```

This will also create a `PeerAdmin` .card file which is needed to modify code running on the peers of your deployment.

You can list all of the cards you have installed at any time by running

```bash
composer card list
```

### 2. Prepare the Fabric peers for your BNA

In order to install the Business Network Archive onto the Fabric network you need to prepare the peers with the underlying Composer runtime.

You will need to have the name of the Business Network to do this, below you can see an example with `tutorial-network` along with the 'PeerAdmin' card for your deployment.

```
composer runtime install --card PeerAdmin@hlfv1 --businessNetworkName tutorial-network
```

### 3. Deploy your BNA onto your fabric

It is recommended you start with a clean directory for this. Move your BNA into that directory and change your terminal directory to it also.

We will use the `composer network start` command to deploy the BNA onto Fabric, we will need to use our `PeerAdmin` card to do this. We will also need to create a user on our network, we will use an "Admin" username and password to get started.

Below is an example using a `tutorial-network` BNA.

```bash
composer network start -a tutorial-network.bna -A admin -S adminpw -c PeerAdmin@hlfv1
```

This will create an 'admin' card for the business network, which for the above example is `admin@tutorial-network.card`

### 4. Install the 'admin' card ready for use

We will next take the admin card we have just made and install it ready for use.

```bash
composer card import --file admin@tutorial-network.card
```

### 5. Start the REST server & generate the Swagger API documentation

Navigate to your directory and run the `composer-rest-server` command.

```bash
composer-rest-server
```

  - Enter admin@tutorial-network as the card name. Make sure to not add the `.card` extension.
  - Select never use namespaces when asked whether to use namespaces in the generated API.
  - Select No when asked whether to secure the generated API.
  - Select Yes when asked whether to enable event publication.
  - Select No when asked whether to enable TLS security.

The rest server will then be generated and available on http://localhost:3000/explorer

### 6. Generate the Angular application

The Angular application requires the rest server to be running to connect to the Fabric instance. Make sure you have the REST server running in the background when doing this. You will also need to be in the same directory as your .BNA file when running Yo.

```bash
yo hyperledger-composer
```

Follow the below so your output matches.

```
Welcome to the Hyperledger Composer project generator
? Please select the type of project: Angular
You can run this generator using: 'yo hyperledger-composer:angular'
Welcome to the Hyperledger Composer Angular project generator
? Do you want to connect to a running Business Network? Yes
? Project name: [insert]
? Description: Hyperledger Composer Angular project
? Author name: [insert]
? Author email: [insert]
? License: Apache-2.0
? Name of the Business Network card: admin@tutorial-network
? Do you want to generate a new REST API or connect to an existing REST API?  Connect to an existing REST
 API
? REST server address: http://localhost
? REST server port: 3000
? Should namespaces be used in the generated REST API? Namespaces are not used
Created application!
```

The generated application will be within a sub directory named after the `Project name` entered above.

Finally enter this directory and to get the application running run:

```bash
npm start
```

It will be available on http://localhost:4200


## References

* [**Developer Tutorial**](../tutorials/developer-tutorial.html)
* [**Composer REST Server**](../integrating/getting-started-rest-api.html)
