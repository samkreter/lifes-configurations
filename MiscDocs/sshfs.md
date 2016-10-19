Using SSHFS for Jetson's Files
===============================


Mount Filesystem
----------------

1. Create directory in mnt

        sudo mkdir /mnt/droplet <--replace "droplet" whatever you prefer

2. Add the mount for the server

        sudo sshfs -o allow_other,defer_permissions root@xxx.xxx.xxx.xxx:/ /mnt/droplet

        sudo sshfs -o allow_other,defer_permissions,IdentityFile=~/.ssh/id_rsa root@xxx.xxx.xxx.xxx:/ /mnt/droplet

Unmount Filesystem
------------------

    sudo umount /mnt/droplet


For permenant access
--------------------

1. Open config file

        sudo vim /etc/fstab

2. Add below command to bottom of the file

        sshfs#root@xxx.xxx.xxx.xxx:/ /mnt/droplet

3. Save and reboot the system