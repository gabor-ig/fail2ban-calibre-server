## fail2ban-calibre-server

### Introduction 
This will create a fail2ban filter and jail for failed login attemts for a calibre-server installation running on port 8180, with authentication enabled, to be served over a virtual host with proxy, for example.

### Setup
Tested on Ubuntu Server 18.04.4 LTS, serving *calibre-server* over an *apache2* virtual host using proxy.

See these tutorials: 
* https://kenfavors.com/code/how-to-install-calibre-server-on-ubuntu-14-04-16-04-18-04/
* https://manual.calibre-ebook.com/server.html#using-a-full-virtual-host 

You need to enable access logging for *calibre-server* to log ip addresses. See **calibre-server.service** for an example configuration. Logs can be rotated with *logrotate*.

SSL/https is optional, make sure you add the certificate and key to the virtual host as well as the calibre-server-backend (calibre user needs read permission to cert files). 

### Fail2Ban
Copy **calibre.conf** to /etc/fail2ban/filter.d. Add calibre jail to /etc/fail2ban/jail.local - see **jail.local** for an example configuration with the calibre-server access log in /var/log/calibre/access.log. 
 
### Final remarks
*Calibre-server* also provides built in brute-force protection, however, I found this to be unreliable. While the *calibre-server* does log failed login attempts, the original IP address is not perserved in proxy mode. The failregex in **calibre.conf** in this project uses a specific 401 167 http error in the calibre access log that pops up in case of failed login attempts. During initial testing, the filter did a good job in recognizing failed login attempts and did not get triggered by genuine user activity. However, I would be happy to have some more people testing it and providing feedback and recommendations. 
