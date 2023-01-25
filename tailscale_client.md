# Tailscale

##  What is

Tailscale is a surprisingly easy to use VPN that allows a fine grained control over all devices in a tailnet (network made of tailscale devices from an organization). [Tailscale](https://tailscale.com/)

##  Why use it

Tailscale lets you easily manage access to private resources, quickly SSH into devices on your network, and work securely from anywhere in the world... [Tailscale](https://tailscale.com/)

## Current implementation

>Private services located in a VPC are only available for tailscale devices that posseses the needed permissions. Permissions are being managed with a GitOps action, you can find it at: [percy-liberty-acl-tailscale](https://github.com/gitMLS/percy-liberty-acl-tailscale)

[Subnet Router Architecture](https://docs.gruntwork.io/assets/images/tailscale-subnet-router-architecture-e55ea6b8a3cd3977ddb4520d4db25c5a.png)

## Installation and usage (client side)

- ### **Linux**
    
    #### **One liner:**

        curl -fsSL https://tailscale.com/install.sh | sh

    #### **Download and install options:**
    
    [Tailscale on Linux](https://tailscale.com/download/linux)

    #### **Use Cases:**
    1. #### **Access to a Private Service:**

            sudo tailscale up --accept-routes=true --reset    

    2. #### **Exit Node Whitelisted IP Address:**
    
            sudo tailscale up --accept-routes=true --exit-node={Tailscale exit node device IP} --reset

    
    If local network access is required (Ex: testing a local service while exiting to the internet through the relay node) use the next flags:

    >    --exit-node-allow-lan-access
    >
    >    --exit-node={Tailscale exit node device IP} 
    >
    **Exs:**

            sudo tailscale up --accept-routes=true --exit-node={Tailscale exit node device IP} --exit-node-allow-lan-access --reset
    
    ***The same flags used for Linux are called under the hood while using UI clients. The described process for Linux equivalent on UI clients (Mac and Windows) is documented at the links provided in each section.***

- ### **Mac**

    #### **Download and install options:**
    
    [Tailscale on Mac](https://tailscale.com/download/mac)
  
    #### **Use Cases:**

    1. #### **Access to a private Service:**
        
        Windows UI client initialize automatically all the flags needed for this case.
        
    2. #### **Exit Node Whitelisted IP Address:**
    
        - [Mac Exit Node Use](https://tailscale.com/kb/1103/exit-nodes/?tab=macos#:~:text=You%20can%20use%20an%20exit%20node%20from%20the%20menu%20bar)

- ### **Windows** 

    #### **One liner (winget):**

        winget install -e --id tailscale.tailscale

    #### **Download and install options:**
    
    [Tailscale on Windows](https://tailscale.com/download/windows)    
    
    #### **Use Cases:**

    1. #### **Access to a private Service**
    
        Windows UI client initialize automatically all the flags needed for this case.

    2. #### **Exit Node Whitelisted IP Address:**

        - [Windows](https://tailscale.com/kb/1103/exit-nodes/?tab=windows#:~:text=You%20can%20use%20an%20exit%20node%20from%20the%20system%20tray%20menu)    
