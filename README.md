# VPS-usage
Key commands and knowledge for using and maintaining a VPS. Optimised for web development (production server).

# Useful commands for linux server administration. Everything tested on Ubuntu 18.04 + DO droplet.

# Specific Software/packages used:
- UFW (unsimplified firewall)
- Nginx 
- Go-access

# Go-access
Go-access is a log viewer for linux systems. It's best use is with the classic access.log (/var/log/nginx/access.log). Installation is pretty straightforward, their github issues are well documented + instructions on their official site are clear.

Note: 
$ apt-get install x
```gcc```, ```make```, ```libgeoip-dev```, ```libmaxminddb```, ```libmaxminddb-dev```, ```ncurses```, ```libncursesw5-dev```, ```geopip-datbase geoip-database-extra php-geoip```, ```geoip```, 

All commands I used that were more or less useful:

To analyze an individual log: [relative path] (cd /var/log/nginx): ```goaccess access.log -v```
To analyze all logs (gzip and normal) [relative path] (cd /var/log/nginx):
```zcat -f access.log* | goaccess --log-format=COMBINED```

To export log analysis to a static html file [relative path] (cd /var/log/nginx): 
```zcat -f access.log* | goaccess --log-format=COMBINED -a -o report.html```


# Nginx:
Very straightforward, just ```apt-get install nginx```. Note: autoindex is disabled by default.

- Server Blocks

```
server {
        root /var/www/example.com/html;
        index index.html index.htm index.nginx-debian.html;

        server_name example.com www.example.com;

        location / {
                try_files $uri $uri/ =404;
                add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
                add_header X-Robots-Tag "none";
        }

        error_page 404 /custom_404.html;
        location = /custom_404.html {
                root /usr/share/nginx/html;
                internal;
        }

        location ~* \.(js|css|png|jpg|jpeg|gif|ico)$ {
            expires 2d;
            add_header Cache-Control "public, no-transform";
       } 
}
```
# ufw

Deny an ip address:
```ufw insert 1 deny from X.X.X.X``` X.X.X.X here is an ip address
List apps:
```ufw app list```
View status:
```ufw status```
View numbered: 
```ufw numbered```
View some verbose:
```ufw verbose```
All of the last 3 combined ( the best):
```ufw status numbered verbose```
Deny specific port:
```ufw deny xx``` xx here is a port number 
Delete any rule:
```ufw delete x``` x here is a rule number
Alow specific app:
```ufw allow 'Nginx HTTP'```

# Show useful networking information:
```ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'```
```ifconfig```


# Logs
SSH log:
```cat /var/log/auth```
NGINX/http port 80 logs:
```cat /var/log/nginx/access.log*```
View logs in real time:
- for nginx```tail -f /var/log/nginx/access.log```
- for ssh ```tail -f /var/log/auth.log```
# Random useful commands

view nginx status: ```systemctl status nginx```


# SCP

NOTES: 
1) When running scp, make sure the target folder on the remote hosts EXISTS. 
2) X.X.X.X is the ip address of the remote host.

Copy all contents of folder in which this command is executed TO a *remote* host /var/www/html folder: 
```scp * user@X.X.X.X:/var/www/html/```

Copy all contents of folder TO a *remote* host /var/www/html folder:
```scp /dir1/dir2/* user@X.X.X.X:/var/www/html```

Copy file in which this command is executed TO a *remote* host /var/www/html folder:
```scp file.txt user@X.X.X.X:/var/www/html/```

Copy all contents of folder in which this command is executed TO a *remote* host /var/www/html folder with port specified: 
```scp -P X * user@X.X.X.X:/var/www/html```

Copy file in which this command is executed TO a *remote* host /var/www/html folder with port specified:

```scp -P X file.txt user@X.X.X.X:/var/www/html```

# Rsync

Synchronise/Copy/Transfer all contents of folder in which rsync is executed TO a *remote* host /var/www/html folder:
```rsync -av . user@X.X.X.X:/var/www/html```

Synchronise/Copy/Transfer all contents of folder in which rsync is executed TO a *remote* host /var/www/html folder:
``` rsync -av . -e 'ssh -p xx' user@X.X.X.X:/var/www/html``` here xx is port and X.X.X.X ip
# Wget
Straightforward

# Git clone
Straightforward

# SFTP
Filezilla site manager. Fairly simple. DIrectory selected in Left pane is where downloaded files go. 

# Config files
- /etc/ssh/sshd_config
Changed default port to specific one, disallowed password auth and allowed public key auth. 

# General

Assign ownership of the directory with the $USER environment variable:
```sudo chown -R $USER:$USER /path/to/dir``` "/path/to/dir" being the path to the directory to change permissions on.
This command changes perms to your linux user with which your executing this command, so it's not root.
Check with whoami before to check if you execute with good user.
