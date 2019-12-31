# WSL-Guide
Windows Subsystem for Linux (WSL) Guide

## Requirements
- Windows Insider account ([join](https://www.windowscentral.com/how-join-windows-insider-program))
- Slow (recommended) or Fast ring insider program ([more](https://insider.windows.com/en-us/how-to-pc/))
- Windows 10
- Not support for Windows 10 S

## Installation WSL1
- Tutorial: https://docs.microsoft.com/en-us/windows/wsl/install-win10

## Installation WSL2
- Requirement
  - WSL 2 is only available in Windows 10 builds 18917 or higher
  - Ensure that you have WSL installed (you can find instructions to do so here) and that you are running Windows 10 build 18917 or higher
To make sure you are using build 18917 or higher please join the Windows Insider Program and select the 'Fast' ring or the 'Slow' ring.
  - You can check your Windows version by opening Command Prompt and running the ver command.
  - Enable the 'Virtual Machine Platform' optional component
  - Set a distro to be backed by WSL 2 using the command line
  - Verify what versions of WSL your distros are using
    
- Tutorial: https://docs.microsoft.com/en-us/windows/wsl/wsl2-install


## Networking (Must setup)
- in WSL
  1. add the following entry to /etc/wsl.conf:
      ```
      # [network]
      # generateResolvConf = false       
      ```
  1. `sudo nano /etc/resolv.conf` and add `nameserver 8.8.8.8` and `nameserver 8.8.4.4`
  1. Duplicate `sudo cp /etc/resolv.conf /etc/resolv.conf.new`
  1. Shutdown WSL in Powershell  
     - Shutdown everything: Build 18917+
        ```
        wsl --shutdown
        ```
     - Terminate specific distro: Windows 1903+
        ```
        wsl -t <DistroName>
        ````  
  1. open WSL again
  1. Unlink the current resolv.con `sudo unlink /etc/resolv.conf`
  1. `sudo cp /etc/resolv.conf.new /etc/resolv.conf`
  1. you should able to connect to internet
  
  
### Bash loses network connectivity once connected to a VPN
- If after connecting to a VPN on Windows, bash loses network connectivity, try this workaround from within bash. This workaround will allow you to manually override the DNS resolution through `/etc/resolv.conf`.

  1. Take a note of the DNS server of the VPN from doing `ipconfig.exe /all`
  1. Make a copy of the existing resolv.conf `sudo cp /etc/resolv.conf /etc/resolv.conf.new`
  1. Unlink the current resolv.con `sudo unlink /etc/resolv.conf`
  1. `sudo mv /etc/resolv.conf.new /etc/resolv.conf`
  1. Open `/etc/resolv.conf` and
       - `sudo nano /etc/resolv.conf` and add `nameserver 8.8.8.8` and `nameserver 8.8.4.4`
      - Delete the first line from the file, which says "# This file was automatically generated by WSL. To stop automatic generation of this file, remove this line.".
      - Add the DNS entry from (1) above as the very first entry in the list of DNS servers.
      - Close the file.
- Once you have disconnected the VPN, you will have to revert the changes to /etc/resolv.conf. To do this, do:

  1. `cd /etc`
  1. `sudo mv resolv.conf resolv.conf.new`
  1. `sudo ln -s ../run/resolvconf/resolv.conf resolv.conf`
  
## VsCode
- Install the [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) extention 
- You will be able to start the vscode in wsl with `code .` command
- You can also avoid passwords by configuring WSL to use the Windows Git credential manager. See tips and [tricks](https://code.visualstudio.com/docs/remote/troubleshooting#_sharing-git-credentials-between-windows-and-wsl) to for details.
