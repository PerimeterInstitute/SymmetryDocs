# VNC (Remote Desktop)

VNC (virtual network computing) is a client/server remote desktop protocol.  There are many different VNC clients and servers
from different vendors and they mostly interoperate.

VNC is unusual in that you, as a regular user on the Symmetry machine, start up a VNC server for yourself when you want to use
it.  That is, you first use `ssh` to connect to Symmetry and run the `vncserver` command to start a VNC server for yourself.
The server (with your desktop session) will stay alive after you log out of the `ssh` session.  You then start up your VNC
client program and connect to the VNC server that you started.

Start by opening an `ssh` connection to Symmetry:
```
ssh symmetry
```
Then run the `vncserver` program.  If this is the first time you are running it, it will ask you for a (new) password for your
desktops.  Please don't use your Perimeter-wide password; choose a new password.
```
dlang@mn001:~$ vncserver

You will require a password to access your desktops.

Password:
Verify:

New 'mn001:2 (dlang)' desktop is mn001:2

Creating default startup script /home/dlang/.vnc/xstartup
Creating default config /home/dlang/.vnc/config
Starting applications specified in /home/dlang/.vnc/xstartup
Log file is /home/dlang/.vnc/mn001:2.log
```

If you ever forget your password, you can delete the config file where it is stored, and the next time you run `vncserver` it
will ask you for a new password.
```
# to reset password:
rm ~/.vnc/config
```

VNC servers are identified by a colon and a number.  For example, above, it created the server `mn001:2`.  Ignore the `mn001`
part for now, but remember the number after the colon.  You will need that number to connect to *your* server, rather than
someone else's.

You can list your currently running servers using `vncserver -list`.  You can stop a running server with `vncserver -kill :2`
(substituting your server's number instead of `2`).

## VNC Clients

Several VNC clients are available, and provide different features and slickness:
 * TigerVNC -- Mac and other platforms (we run the TigerVNC server): https://bintray.com/tigervnc/stable/tigervnc/1.9.0
 * RealVNC -- they sell a VNC server and provide free clients (including iOS, Android, and Google Chrome): https://www.realvnc.com/en/connect/download/viewer/

To configure TigerVNC, you can use this [Config file assets/symmetry.tigervnc] as a starting point.  After running the
`TigerVNC Client` program, click the `Load` button to load this config file.  Note that you will still need to use the
matching *number* of your VNC server, `symmetry.pi.local:###`

After you click the `Connect` button you should get a window showing you your remote desktop on the Symmetry head node.

[![VNC Remote Desktop]({{ site.url }}/assets/vnc1.jpg)

If you resize this window, the remote desktop will get resized.

On Mac OSX, the TigerVNC application is a bit strange -- in order to get a menu, you must hit the F8 key.  (On recent
TouchBar laptops, hold the `Fn` key and then type the `F8` key.  This will give you the option of full-screen mode, exiting, etc.

[![VNC F8 Menu]({{ site.url }}/assets/vnc2.jpg)














