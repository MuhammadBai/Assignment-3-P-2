# Assignment 3 Part 2: Web Server and Load Balancer Setup

### Overview

In this project, I worked on setting up two web servers with Arch Linux and configured a load balancer to distribute traffic between them. Additionally, I updated the server configuration to include a file server where users can access files by visiting a `droplet_ip/documents`. 

This README explains the steps how everything works together.

## What I Did

### 1. Created Two Web Servers
* I created two Arch Linux droplets (servers) on DigitalOcean and tagged them as web.
* I configureed my SSH config file so I can easily  connect by typing ssh drop1 or ssh drop2 instead of long IP addresses.
* Updated the hostnames to drop1 and drop2.

### 2. Installed the Necessary Software
On both servers, I installed:
* **Nginx:** The web server software that serves websites and files.
* **Neovim:** My preferred text editor for editing configuration files.

### 3. Set Up a Load Balancer
I created a **load balancer** in DigitalOcean that:
* Balances traffic between the two droplets tagged web.
* it Is located in the same region (SFO3) and network (Default VPC) as the droplets.
**This ensures:**
* If one server goes down, the traffic is automatically redirected to the other.
* Both servers share the workload for better performance.

### 4. Cloned the Starter Code
* Cloned my instructor's repository to both servers.i.e `(https://git.sr.ht/~nathan_climbs/2420-as3-p2-start)`
* Updated the directory structure for the webgen system user to include a /documents directory:
```
/var/lib/webgen/
├── bin/             # Contains the 'generate_index' script for creating index.html
├── documents/       # Contains file-one, file-two
│   ├── file-one     # Sample file with some text
│   └── file-two     # Another sample file with text
└── HTML/            # Contains the generated 'index.html'
```

### 5. Configured Nginx
* I created two directories to manage server configurations:
```
sudo mkdir /etc/nginx/sites-available
sudo mkdir /etc/nginx/sites-enabled
```
* Wrote a new server configuration `drop1server_block.conf` in `/etc/nginx/sites-available`. This configuration does the following:

* Serves the main website at / using the index.html file in /var/lib/webgen/HTML/.
* Serves files at /documents using the documents folder.
* Lists files automatically when visiting /documents.

* Linked the configuration file to Nginx’s enabled sites directory:
`sudo ln -s /etc/nginx/sites-available/drop1server_block.conf /etc/nginx/sites-enabled/drop1server_block.conf`

* Tested the Nginx configuration and reloaded it:
```
sudo nginx -t
sudo systemctl reload nginx
```
### 6. Tested Everything
* Accessed the load balancer’s public IP to verify that it properly distributes traffic between the two servers.
Checked the file server by visiting http://64.23.176.133/documents to confirm that file-one and file-two were listed and downloadable.

### Why I Did It This Way
* **Load Balancer:** It is reliable to add load balancer because if in case one server fail other server will take the lead and provides scalability where more servers can be added easily.
* **Nginx:** This configuration make it easier to manage multiple websites or features like the file server.
* **System User:** We create system user as webgen which keep things secure by limiting access to only what is necessary.

### How You Can Test It
* Visit the load balancer’s public IP in a browser.
* Go to drop_2_ip/documents to view and download the sample files.

