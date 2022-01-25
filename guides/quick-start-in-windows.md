## At a glance

In this guide, You'll configure WSL2 for Windows, a neccessary tool to run Mailchimp Open Commerce.

## What you’ll need

- [Docker](https://www.docker.com/)

- [Docker Compose](https://docs.docker.com/compose/)

- [WSL 2](https://docs.microsoft.com/en-us/windows/wsl/install-win10) and [Docker for WSL](https://docs.docker.com/docker-for-windows/wsl/)

## Install WSL2
  - https://docs.microsoft.com/en-us/windows/wsl/install-win10
  
> **Note**: The automatic Windows Insider install comes with the Ubuntu distro. If you have manually installed WSL2, you can pick another Linux distro. This guide assumes you picked Ubuntu.

## Install Docker Desktop WSL2 backend
  - https://docs.docker.com/desktop/windows/wsl/
  - Open Docker, goto `Settings>Resources>WSL Integration` and verify that everything on that page is activated.
  - Under `Experimental Features` enable `Use Docker Compose v2 candidate`

## Start Ubuntu and install Open Commerce
  - You should be root (`root@` is visible in commandline), if not execute 
    ```bash
    sudo su
    ```
  - Install the *make* package 
    ```bash
    apt install make
    ````
  - Clone the *Mailchimp Open Commerce Developer platform* and go into it's directory
    ```bash
    git clone https://github.com/reactioncommerce/reaction-development-platform
    cd reaction-development-platform
    ```
  - Build the platform and allow firewall
    ```bash
    make
    ```
    > **Note**: During the build process Windows will open a firewall window. You got to accept this by clicking *Allow Access* when it pops up.

*You have now installed Mailchimp Open Commerce.*


