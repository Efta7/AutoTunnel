# AutoTunnel
Distributed SSH tunneling implementation for GNU/Linux

 Note: This software is in early development stages, please help us enhance it further.

## Use case
 In case of DPI where ISPs are blocking VPN connections.
 The idea is to have multiple ssh tunnels providers and to keep updating the servers list
 by contributions of other sysadmins.

 The best example of this is the case of Egypt. Where all ISPs are required to block certian
 websites and blocking VPN providers constantly. This software is created specifically for this
 situation, where members of the IT community are willing to contribute to stop internet censorship.

## How it works
 It's a simple SSH tunneling mechanism but on steroids with everything being fault-tolerant.
 A sysadmin willing to contribute a SSH server will simply create a user with no credentials
 then set his shell to nologin and allow passwordless login and send his SSH server IP/Port to a
 Master List operator.

 The Master List is AES-256-CBC encrypted - BASE64 encoded string posted to a Facebook page.
 The key is preset by the list operator and provided to users of that list as part of the client
 configuration.

 A user will then run the client daemon with the FB page, the key and the SSH username for that list.
 The client daemon will retrieve the list, decrypt it and will select the server with the lowest latency
 and keep switching tunnels through that list.

 The client deamon will forward all TCP connection through that SSH tunnel. so [redsocks](https://github.com/darkk/redsocks)
 is a dependency to this daemon.

 In case one IP failed for any reason. the client will automatically switch to another one retaining the connection.
 In case one Facebook page fails, the Master list operator still have other pages that can serve the same purposes.
 Multiple Facebook pages can provide the list if defined in configuration, given that the SSH username / List key is the same.

 So in this context, ISPs can still block IPs and Facebook can still take pages down while client is still able to browse the
 internet safely.

 This of course requires co-operation across multiple SSH servers owners, We donated 34 servers for this purpose.

Any sysadmin can have his own master list, just create a FB page and post the IPs encypted to it, then share the key, the page id
and the username to SSH servers.

## Installing/Running client side daemon

 Dependencies: curl, redsocks, ping, openssl and openssh-client
 For Debian/Ubuntu or similar distros you can install dependencies by:
```
 sudo apt-get install curl redsocks iputils-ping openssl openssh-client
```

Then run:
```
 git clone https://github.com/Efta7/AutoTunnel.git
 cd AutoTunnel
 chmod +x autotunnel
 ./autotunnel ./autotunnel.conf
```

Configuration:
 We provide default working config example in autotunnel.conf
 However, If the daemon is running without the first parameter as the path of the configuration file, it will look under /etc/autotunnel.conf
 If it couldn't find it, then it will exit.

 You can find comments inside default configuration file on each parameter.

## Installing/Running Server side daemon

 This feature is still in development, so please be patient.

## Installing/Running Master List deamon

 This feature is still in development, so please be patient.

## Contributing your server to the network
 We are implementing this after we finish other server side work, so please be patient.

## License (GPLv3.0)
 See license [here](https://www.gnu.org/licenses/gpl-3.0.txt)
