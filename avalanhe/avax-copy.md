# Run an Avalanche Node with Latitude.sh 


An Avalanche node is a network node that helps to maintain the Avalanche blockchain network. In this guide, we will discuss how to create an Avalanche node on <a href="https://latitude.sh" target="_blank"> Latitude.sh </a>
  
## Introduction

This tutorial will guide you through setting up an Avalanche node on <a href="https://latitude.sh" target="_blank"> Latitude.sh </a>. 
<br> Latitude.sh provides high-performance lighting-fast bare metal servers to ensure that your node is highly secure, available, and accessible.

To get started, you'll need:

-   A Latitude.sh account
-   A terminal with which to SSH into your Latitude.sh machine

This tutorial assumes your local machine has a Unix-style terminal. If you're on Windows, you'll have to adapt some of the commands used here.
  



##  Configuring your server

  

###  Create a Latitude.sh account

You will need to go to Latitude.sh to create an account.

- The first step is to sign up at [Latitude.sh](https://www.latitude.sh/dashboard/signup) 
<img src="https://i.imgur.com/6c5grrS.png"  width="600">

- The second dtep is telling us a bit about your team.
<img src="https://i.imgur.com/lby2jW7.png"  width="600">

- The third step will be to create your first project.
<img src="https://i.imgur.com/uKcNdBZ.png"  width="600">

### Verify your Latitude.sh account.
Now that your account is set up, you need to provide some information to verify it.
Please click the verify account button to get started.
  <img src="https://i.imgur.com/uKDiVQq.png"  width="600">

Please fill add a `payment method`, `govt ID` and a `photo of the credit card` used.
  <img src="https://i.imgur.com/XxmPspW.png"  width="600">

Once that has been completed, you need to wait for the team to get back to you, verification can take up to 48 hours.
  <img src="https://i.imgur.com/KLJyAKJ.png"  width="500"> 


###  Create your server on the Latitude.sh dashboard

Your Latitude.sh account has been verified, and now we are ready to configure and deploy your bare metal server! 
Click on `Servers` in the sidebar and the `Create server` button:

  <img src="https://i.imgur.com/iO960s4.png"  width="600">

 


For an Avalanche RPC node, the `c3.medium.x86` or `c2.medium.x86` servers are recommended, however the `c1.medium.x86` could work.
The server you need is largely dependent on your use case.

  
  <img src="https://i.imgur.com/bBPGOP5.png"  width="700">
  

If you do not see the server available, click on another location until it is listed under the `On-demand` tab.

For this tutorial, we will be using a `c3.medium.x86` machine

  

Options:


  <img src="https://i.imgur.com/BfFxNPB.png"  width="500">
  


-  `Operating System`: Select `Ubuntu 22.04`  *for fast deployment*

- All other settings are optional.

  

Once you are comfortable with your settings click the `Deploy ->` button. Depending on your configuration, deployment can be as fast as 15 seconds for `Ubuntu 22.04`  or up to 10 minutes for other deployments.

  <img src="https://i.imgur.com/PJDnuFv.png"  width="500">
  

After deployment is complete, click to enter the server and save your IP address from your server dashboard, as well as other settings like `username` and `password`.

###  Access your server & further steps
You can get your username and password and all other credentials by clicking the `server` under your project, we can then use these credentials to access your Latitude.sh machine from your local machine using a terminal.

After gaining access, we’ll need to set up our Avalanche node. To do this, follow the <a href="https://docs.avax.network/nodes/build/set-up-node-with-installer" target="_blank">
Set Up Avalanche Node With Installer </a>  tutorial which automates the installation process. 

Your AvalancheGo node should now be running and in the process of bootstrapping, which can take a few hours. To check if it's done, you can issue an API call using  `curl`.
The request is:


``` 
curl -X POST --data '{
    "jsonrpc":"2.0",
    "id"     :1,
    "method" :"info.isBootstrapped",
    "params": {
        "chain":"X"
    }
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
```
Once the node is finished bootstrapping, the response will be:

```
{  
"jsonrpc": "2.0",  
"result": {  
"isBootstrapped": true  
},  
"id": 1  
}
```
You can continue on, even if AvalancheGo isn't done bootstrapping.
In order to make your node a validator, you'll need its node ID. To get it, run:

```
curl -X POST --data '{  
"jsonrpc":"2.0",  
"id" :1,  
"method" :"info.getNodeID"  
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info
```

The response contains the node ID.

`{"jsonrpc":"2.0","result":{"nodeID":"KhDnAoZDW8iRJ3F26iQgK5xXVFMPcaYeu"},"id":1}`

In the above example the node ID is`NodeID-KhDnAoZDW8iRJ3F26iQgK5xXVFMPcaYeu`. Copy your node ID for later. Your node ID is not a secret, so you can just paste it into a text editor.

AvalancheGo has other APIs, such as the  [Health API](https://docs.avax.network/apis/avalanchego/apis/health), that may be used to interact with the node. Some APIs are disabled by default. To enable such APIs, modify the ExecStart section of  `/etc/systemd/system/avalanchego.service`  (created during the installation process) to include flags that enable these endpoints. Don't manually enable any APIs unless you have a reason to.


Exit out of the SSH server by running:

```
exit
```


### Upgrading Your Node

AvalancheGo is an ongoing project and there are regular version upgrades. Most upgrades are recommended but not required. Advance notice will be given for upgrades that are not backwards compatible. To update your node to the latest version, SSH into your server using a terminal and run the installer script again.

```
./avalanchego-installer.sh
```

Your machine is now running the newest AvalancheGo version. To see the status of the AvalancheGo service, run  `sudo systemctl status avalanchego.`


## Wrap Up

That's it! You now have an AvalancheGo node running on a Latitude.sh machine. We recommend setting up  [node monitoring](https://docs.avax.network/nodes/maintain/setting-up-node-monitoring) for your AvalancheGo node.  If you have feedback on this tutorial, or anything else, please let us know.


`Author`
Bombata Habeeb
*Habeeb.bombata@latitude.sh*
