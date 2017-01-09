# letsencrypt-contrib

Author: Telecoms Sans Frontieres

Mail: it@tsfi.org

GIT Repository: https://github.com/TelecomsSansFrontieres

# Usage

```
Usage: certbot-tomcat -f [FILE] [OPTION]...
This script is designed to handle automated management of SSL Letsencrypt
certificates on production servers.

  -f,--file [FILE]  load configuration file

OPTIONS
  -c,--create       create a new SSL/TLS certificate
  -r,--renew        renew-only existing SSL/TLS certificate (exit 2 on success)
  -i,--import       import existing certificate into keystore
  -h,--help         print this help

CONFIGURATION FILE

    DOMAIN          Domain name related to the certificate renewal.

    CERTDIR         Directory that contains generated certificates.
                    Default: /etc/letsencrypt/live/$DOMAIN/

    EMAIL           Email of the system administrator to notify renewal.

    LISTENPORT      Alternative listening for for certbot standalone mode.

    HOMEDIR         Application's user.

    LETSENCRYPTDIR  Directory that contains the certbot files (executable scripts).

    KEYTOOLDIR      Path of JRE binaries.

    KEYSTORE        Full path of the keystore that will contain the certificate. It
                    must be accessible to the application's user.

    PASSFILE        Full patch of the file that contains the encrypting passphrase.
```

# Crontab

Below is an example of crontab that execute frequently the SSL Certificate renewal script and trigger notification script when a new certificate is generated.

/etc/cron.d/letsencrypt:

```
SHELL=/bin/bash
LOG=/var/log/letsencrypt/cron.log
BINPATH=/usr/local/bin
EMAIL="mail@example.org"

57 18 * * * root USER=$LOGNAME; ${BINPATH}/certbot-tomcat -f /etc/letsencrypt/certbot-tomcat.conf --renew; [ $? == 2 ] && /usr/local/bin/at-notify -d 0 -m ${EMAIL} -e "systemctl restart tomcat7"
```
