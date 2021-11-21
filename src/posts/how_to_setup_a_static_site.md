---
title: How to setup a static site on Digital Ocean and automatically deploy changes using GitHub Actions
layout: default.liquid
---
#### John Mason - 03-10-2021

## How to setup a static site on Digital Ocean and automatically deploy changes using GitHub Actions

In this post you will learn all about how to setup your own static site.

## Getting a domain
To allow people to view your website and host it on the DNS servers you will need to start by buying a domain name. I brought mine from `namecheap` for around £11.

## Generate your local SSH keys
To connect to the server you will need to use SSH. In a terminal type `ssh-keygen` and then follow the instructions to generate your keys.

## Setup initial Digital Ocean server
To host the site we're going to need a server. I will be using Digital Ocean's simple droplet.

Before we create the droplet on the dashboards navigation menu on the left go to settings > security > add SSH key. Now copy the contents of your public SSH key into the box and give it a name before clicking the add SSH key button. 

Once on the dashboard go to droplets and create a new one. Select a Ubuntu Machine on the basic plan, choose the place you want to host your data, add the SSH key we added to your account and then create the droplet.

You can now start the droplet.

## Connecting to the droplet
On the dashboard go to the droplet section and then click on the droplet you created and make sure it's turned on. Once turned on you should see an IP adress next to `ipv4`. Copy the IP address and then go to powershell on your computer and type `ssh root@[IP adress just copied]`. Follow the instructions and you'll then be connected to your server.

## Setting up the server
Now your connected to the server we're going to need to update the server, generate a new SSH key, install nginx, setup certbot certificate.

### Updating the server
To update the server type the following into the terminal `sudo apt update && sudo apt upgrade -y`.

### Generate the server SSH key
To generate the SSH key for the server start by typing the command `ssh-keygen -m PEM -t rsa -b 4096` and follow the instructions given. This will generate the SSH key for the server. 

We now need to copy the SSH key for our server into the `authorized_keys` folder. Navigate into the .ssh directory `cd ~/.ssh` and then type `cat [pub key file] >> authorized_keys` to add it to the file. Whilst your here copy the value of the private key to and make sure to have it accessible somewhere for later. 

### Install nginx
Type the command `sudo apt-get install nginx` to install nginx onto the server. Once install navigate to `http://[server ip]` to see the basic page loaded by nginx. Now navigate to the nginx directory by navigating to `var/www/html/`. Delete all the files inside so we have a clean web server to deal with.

### Install Certbot
Finally, type `sudo snap install core; sudo snap refresh core` into the terminal followed by `sudo snap install --classic certbot` followed by `sudo ln -s /snap/bin/certbot /usr/bin/certbot` followed by `sudo certbot --nginx`. Certbot is now installed and you've setup your certificate so your site will now run on https. 

## Setting up the domain on your server
Go onto your domain provider and enter the following 3 custom dns urls for your domain name `ns1.digitalocean.com`, `ns2.digitalocean.com`, `ns3.digitalocean.com`. 

Now go onto the digital ocean dashboard go to the networking section and then domains. Add a new domain and then add 2 new domain records. The first one of type A with a host name of `[your domain name]` which direct's to the IP of your server and then the second of type CNAME which contains a host name `www.[your domain name]` with an alias of `[your domain name]`. 

Your server and domain is now all setup.

## Deploying from Github
Create your repository and add your files.

### Setting up repository secrets
On your repository screen navigate to settings > secrets and then add 4 new secrets. 

The first one to add should be called `REMOTE_HOST` with the value of your domain name or server IP. 

Secondly, add another with the name of `REMOTE_TARGET` with the value of the directory we're saving the files too. In this case the value should be `/var/www/html/`.

Thirdly, add another with the name of `REMOTE_USER` with the value of `root`. 

Finally, add one with the value of `SERVER_SSH_KEY` and add in the value of the private key you generated on the **SERVER** making sure to copy the entire files contents into the field. 

### Setting up the workflow
Now on the repository screen navigate to actions > new workflow and setup a simple workflow. Into the contents of the YAML file enter:

{% raw %}
<pre>
<code>
name: CI

on:
  push:
    branches: [ main ]
    
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
       
    steps:
    - uses: actions/checkout@v2
      
    - name: Installing required packages
      run: sudo apt install git curl
    - name: Installing Rust
      run: curl https://sh.rustup.rs -sSf | sh -s -- -y
    - name: Adding Rust to ENV
      run: source $HOME/.cargo/env
    - name: Installing Cobalt
      run: curl -LSfs https://japaric.github.io/trust/install.sh |
        sh -s --
        --git cobalt-org/cobalt.rs
        --crate cobalt
        --force
        --target x86_64-unknown-linux-gnu
        --tag v0.15.0
    - name: Build Cobalt Site
      run: cobalt build -c ./src/_cobalt.yml -d ./build
    - name: Deploy to Staging server
      uses: easingthemes/ssh-deploy@main
      env:
          SSH_PRIVATE_KEY: ${{ secrets.SERVER_SSH_KEY }}
          ARGS: "-rltgoDzvO --delete"
          SOURCE: "build/"
          REMOTE_HOST: ${{ secrets.REMOTE_HOST }}
          REMOTE_USER: ${{ secrets.REMOTE_USER }}
          TARGET: ${{ secrets.REMOTE_TARGET }}
</pre>
</code>
{% endraw %}

The steps involving installing rust, cobalt and building your website may be different and you may want to install node and then run `npm run build` depending on what your website is. Once you've setup the file for your needs you can now commit the file to the repo and watch it run.

It should add your site to the folder on the server and if you navigate to your domain name in your browser you should see your site and it's contents. 

Everytime you push to the repository the website will now be updated.
