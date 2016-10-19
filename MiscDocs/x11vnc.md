Remote Desktop Through VNC
==========================

Setting up a VNC server to allow for remote desktop through ssh
---------------------------------------------------------------

    1. Install the X11VNC server (or through Ubuntu Software Center -> X11VNC Server)

        - sudo apt-get install x11vnc

    2. Create a VNC password file.

        - sudo x11vnc -storepasswd yourVNCpasswordHERE /etc/x11vnc.pass

    3. Create a job file in the editor nano (or gedit, leafpad etc.).

        - sudo nano /etc/init/x11vnc.conf

    4. Paste this into the file:

        start on login-session-start

        script

        /usr/bin/x11vnc -xkb -forever -auth /var/run/lightdm/root/:0 -display :0 -rfbauth /etc/x11vnc.pass -rfbport 5900 -bg -o /var/log/x11vnc.log

        end script

    5. Save the file. You created a job for the Upstart event login-session-start.
    Restart Ubuntu.

To set up the Vnc client
------------------------

    1. Install the vnc client viewer

        - sudo apt-get install vncviewer

    2. Run the vncviewer command, enter ip address and password

Reference: http://askubuntu.com/questions/229989/how-to-setup-x11vnc-to-access-with-graphical-login-screen


Needed Workarounds and Known Issues
-----------------------------------

    1. For some reason the above script for the x11vnc server doens't start on init. To get around this I have set up an alias in the .bash_aliases file that contains

        `alias svnc = "sudo x11vnc -xkb -forever -auth /var/run/lightdm/root/:0 -display :0 -rfbauth /etc/x11vnc.pass -rfbport 5900 -bg -o /var/log/x11vnc.log" `

    This will create an alias that allows you to type svnc which will start the vnc server on the remote machine, in this case jetson tk1

    2. Because of some firewall issues with port 5900, you must use ssh tunneling to attach port 5900 to your local. To do this add the following line to the ~/.bash_aliases file. if the file does not exist created it.

        `alias jetsonVnc = "ssh -f -L 5900:localhost:5900 ubuntu@jetsontk1 -N"

    Now you are able to type the command jetsonVnc and set up the tunnel.

    3. Know Issue. This vnc viewer with the jetson will only work if you have a monitor plugged into the jeston. If you do not, you will get an error saying somehting about the device running in low graphics mode. To fix,

    Plug in a monitor and reboot the jetson.

Step to connect after inital setup
----------------------------------

After all of the above commands and alias have been set and you have re-opened all of the terminals.

    1. log onto the jetson and type the command `svnc` to start the vncserver to server the jetson desktop

    2. On your local machine, type the command `jetsonVnc` in order to set up the ssh tunnel

    3. On your local machine, type the command  `vncviewer` and enter the address 127.0.0.1

    4. You will now be prompted to enter the password you have set up above when setting up the vnc server.

You know should see the desktop of the Jetson TK1.

If you see and error message saying your machine is running in low graphics mode, you must plug in an external monitor into the jetson and restart the machine. This will fix all of the GUI driver issues.