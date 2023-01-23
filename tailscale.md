# Tailscale

##  What is

Tailscale is a surprisingly easy to use VPN that allows a fine grained control over all devices in a tailnet (network made of tailscale devices from an organization). [Tailscale](https://tailscale.com/)

##  Why use it

Tailscale lets you easily manage access to private resources, quickly SSH into devices on your network, and work securely from anywhere in the world... [Tailscale](https://tailscale.com/)

## Current implementation

>Private services located in a VPC are only available for tailscale devices that posseses the needed permissions. Permissions are being managed with a GitOps action, you can find it at: [percy-liberty-acl-tailscale](https://github.com/gitMLS/percy-liberty-acl-tailscale)

![Tailscale Subnet Router architecture](https://docs.gruntwork.io/assets/images/tailscale-subnet-router-architecture-e55ea6b8a3cd3977ddb4520d4db25c5a.png "Tailscale Subnet Router architecture")

***Tailscale Subnet Router architecture***

## Installation and usage (client side)

- ### Linux
    
    #### One liner:

        curl -fsSL https://tailscale.com/install.sh | sh

    #### Download and install options:
    
    [Tailscale on Linux](https://tailscale.com/download/linux)

- ### Mac

    #### Download and install options:
    
    [Tailscale on Mac](https://tailscale.com/download/mac)

- ### Windows 

    #### One liner (winget):

        winget install -e --id tailscale.tailscale
        
    #### Download and install options:
    
    [Tailscale on Windows](https://tailscale.com/download/windows)
  