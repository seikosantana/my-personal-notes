# PowerDNS on Ubuntu
(or likely any other Linux distros)
Main reference: https://www.howtoforge.com/how-to-install-powerdns-admin-on-ubuntu-20-04/

## Why would I even run my own DNS Server? What for?
I and team were working on an installation on a big site. We have numbers of devices that collects telemetry periodically and sends it to a server. Yes, sensor devices they are. Those devices were sending to a server and the other day we need to migrate everything and use only one server, and it's pretty troublesome to reconfigure all devices to change the IP address. _Now, imagine if we were sending to a host with a domain instead of IP address_.

## Why PowerDNS?
No particular reason. I've seen some suggestions about using BIND9, learn to configure it, understand real concepts behind DNS, blah blah blah.. **but I prefer ease of configuration** and besides I don't have tons of domains to resolve. To be honest, i have only two domains to resolve to, and a few subdomains in each. PowerDNS offers a web interface to allow easier DNS configuration through the web application.

## Installing PowerDNS
Look at the guide on the link on the top of this post. That explains pretty much everything, but here are a few things we should note: Use the **schema** that is meant for your `pdns` version. What does that mean?

The installation of `pdns` I did in the past gives me PowerDNS with version 4.1.1 and the schema given just does not work. On the `Install PowerDNS` section of that article, you'll be guided to install `pdns-server-backend` and then on the `Configure PowerDNS` section we are told to import an SQL file with `mysql -u pdnsadmin -p pdns < /usr/share/pdns-backend-mysql/schema/schema.mysql.sql` and yes, that file didn't even exist.

### So, where to get the schema?
From the PowerDNS [official documentation](https://doc.powerdns.com/authoritative/backends/generic-mysql.html#default-schema), there's a link to [the 4.1 schema on Github](https://github.com/PowerDNS/pdns/blob/rel/auth-4.1.x/modules/gmysqlbackend/schema.mysql.sql). Grab that and import that one instead to the newly created database.

## Installing PowerDNS Admin
This is the thing I wanted, not because it is PowerDNS, but because it has PowerDNS admin lol.
Follow that guide on the section. Everything will happen to be smooth, until we execute `flask db upgrade`. Also related to different version of MariaDB we are using, but if you look closely, the error message a clue related to length thingy. I didn't write this right after installation so I kinda forgot what it says but basically we''ll have to modify the column type to support that thing the `upgrade` script is complaining about. Will update this section after I do another PowerDNS Admin installation, but I believe this note is sufficient to perform a successfully running pdns and pdns-admin installation.
