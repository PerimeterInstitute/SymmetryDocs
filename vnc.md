# VNC (Remote Desktop)

VNC (virtual network computing) is a client/server remote desktop protocol.  There are many different VNC clients and servers
from different vendors and they mostly interoperate.

VNC is unusual in that you, as a regular user on the Symmetry machine, start up a VNC server for yourself when you want to use
it.  That is, you first use `ssh` to connect to the Symmetry head node and run the `vncserver` command to start a VNC server for yourself.
The server (with your desktop session) will stay alive after you log out of the `ssh` session.  On the computer where you want to display the desktop (eg, your laptop), you start up a VNC
client program and connect to the VNC server that you started on Symmetry.

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

You can also run this in one step:
```
ssh symmetry vncserver
```
or
```
ssh symmetry vncserver -list
```
to list any currently running desktop sessions you have, or, for example,
```
ssh symmetry vncserver -kill :1
```
to stop a running desktop session.

VNC servers are identified by a colon and a number.  For example, above, it created the server `mn001:2`.  Ignore the `mn001`
part for now, but remember the number after the colon.  You will need that number to connect to *your* server, rather than
someone else's.

You can list your currently running servers using `vncserver -list`.  You can stop a running server with `vncserver -kill :2`
(substituting your server's number instead of `2`).

## VNC Clients

Several VNC clients are available, and provide different features and slickness:
 * TigerVNC -- Mac and other platforms: <https://bintray.com/tigervnc/stable/tigervnc/1.9.0>
 * RealVNC -- they sell a VNC server and provide free clients (including iOS, Android, and Google Chrome): <https://www.realvnc.com/en/connect/download/viewer/>

To configure TigerVNC, you can use this [Config file](assets/symmetry.tigervnc) as a starting point.  After running the
`TigerVNC Client` program, click the `Load` button to load this config file.  Note that you will still need to use the
matching *number* of your VNC server, `symmetry.pi.local:###`

After you click the `Connect` button you should get a window showing you your remote desktop on the Symmetry head node.

If you resize this window, the remote desktop will get resized.

On Mac OSX, the TigerVNC application is a bit strange -- in order to get a menu, you must hit the F8 key.  (On recent
TouchBar laptops, hold the `Fn` key and then type the `F8` key.  This will give you the option of full-screen mode, exiting, etc.

![VNC F8 Menu](/assets/vnc2.jpg)

### Applications

#### Mathematica

To start an application, press the *K* button in the bottom-left.
Applications are organized into categories, but they don't always make sense;
*Mathematica* appears in the *Lost and Found* category, so it may be easiest to
use the *Search* box to find it!

![Finding Mathematica](/assets/mathematica.jpg)

#### Matlab

*Matlab* does not appear under the *K* start menu, unfortunately.  Instead,
search for and start the *Konsole* terminal program, and type:
```
module load matlab
matlab
```

![Starting Matlab](/assets/matlab.jpg)

## SSH Tunnel

If you are not inside the PI network (VPN or in the building), you can connect to your VNC server using an *SSH tunnel*.

SSH tunnels are created using the `-L` option to the `ssh` program.  For example:

```
ssh -L 5678:symmetry:5902 mars.perimeterinstitute.ca
```

will create an ssh tunnel where connecting to port `5678` on your local computer will tunnel through the ssh connection
and connect to `symmetry` on port `5902`.  The VNC servers listen on port `5900` plus the number after the colon; my VNC
server number is `2` so I am connecting to port `5902`.

Now, when you start your TigerVNC client, you must tell it to connect to *your* local computer's port `5678`:

![VNC Localhost connection](/assets/vnc3.jpg)

















