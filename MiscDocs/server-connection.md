Connecting to the Jetson TK1
============================

Needed methods for connecting to the jetson TK1


Displaying Windows From SSH
---------------------------

This is how to configure the ssh connection to be able to remotley display screens on the jetson TK1. This is usufully to allow for showing screens on the jetson without having to have an external keyboard inputed which will take up the USB port

1. connect remotely to the Jetson TK1 through an SSH connection.

2. Once connected execute the command ` export DISPLAY=:0 ` to change the display setting

Now any program executed that will launch a screen, the screen will now be launched on the main session of the jetson through the main display port, which is most likly the HDMI port.


Coniguration for internet/dhcp connection
-----------------------------------------

This configuration is used to recieve an ip address from some kind of dhcp server. You will not be able to ssh into the Jetson with this configuration. You must either set a temporary IP address or a Static IP address.

1. Edit the file ` /etc/network/interfaces `

    - The new configuration should be

            auto eth0
            iface [port] inet dhcp

    - where [port] is the port you want to configure, in this case eth0

2. Next reset the configuration with the commands

    ` sudo ifdown [port] ` **then** ` sudo ifup [port] ` to re-enable the connection

    *where [port] is the name of the port you are working with, in this case it is most likly eth0


Setting temporary IP Address
----------------------------
1. Execute the command ` sudo ifconfig [port] up [address to set] netmask [netmask] `

    - [port] is the port you want to set, in this case is eth0
    - [address to set] is the desired address, in this case 192.168.92.77
    - [netmask] is the esired netmask for the network, in this case 255.255.255.0


Setting Static IP Address
-------------------------

Note that with this configuration you will not be able to connect to the internet or a dhcp network.

1. To set or check the static IP address edit the file ` /etc/network/interfaces `

    - The configuration for in the file should look like

            auto eth0
            iface [port] inet static
            address 198.168.92.77
            netmask 255.255.255.0

    - Here the address will the the static ip address and in this case is set to 198.168.92.77

2. Next reset the configuration with the commands

    ` sudo ifdown [port] ` **then** ` sudo ifup [port] ` to re-enable the connection

    *where [port] is the name of the port you are working with, in this case it is most likly eth0

3. Use the command ` ifconfig ` to check that there is a line ` inet addr:[address you set] ` to make sure the configuration worked properly


Setting Static IP connecting to CGI internet
--------------------------------------------

This is a mixture of the top two that allows for having a static ip for the jetson while still connecting to the CGI intranet

1. To set or check the static IP address edit the file ` /etc/network/interfaces `

    - The configuration for in the file should look like

        auto eth0
        iface eth0 inet static
        address 172.16.10.100
        netmask 255.255.254.0
        gateway 172.16.10.1
        dns-nameservers 172.16.10.12 172.16.10.13
        dns-search cgi.missouri.edu

2. Next reset the configuration with the commands

    ` sudo ifdown [port] ` **then** ` sudo ifup [port] ` to re-enable the connection

    *where [port] is the name of the port you are working with, in this case it is most likly eth0

3. Use the command ` ifconfig ` to check that there is a line ` inet addr:[address you set] ` to make sure the configuration worked properly