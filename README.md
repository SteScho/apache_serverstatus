# apache_serverstatus
nagios, icinga, ... plugin and pnp4nagios template for apache server-status

```
Options:
 -h, --help
    Print help
 -V, --version
    Print version
 -H, --hostname
    Host name or IP address - will be used as 
    http://<hostname>/server-status. You can overwrite this
    url by using -u/--url.
 -v, --verbose
    Be much more verbose.
 --wget
    Path to wget. Could also be used to use lynx or something
    else instead of wget. Output must be send to stdout.
 --woptions
    Arguments passed to wget.
 -u, --url
    Use this url to connect to the apache server-status. Usefull
    if the auto generated url out of the hostname is not correct.
 --wc=<field,warning,critical>, --warncrit=<field,warning,critical>
    Field could be any of the letters of the apache scoreboard or
    of the other keys returned by server-status. Can be set multiple
    times if you want to check more than one field.
```

##### Examples:

```
./check_apache_serverstatus.pl --wc=.,@130:,@145: --wc=BytesPerSec,1000,2000
WARNING APACHE SERVER STATUS Wait=8, Start=0, Read=0, Send=1, Keepalive=1, DNS=0, Close=0, Logging=0, Graceful=0, Idle=0, Free=140 ===> Total=150 RATES ReqPerSec=.206872, BytesPerSec=1124.59, BytesPerReq=5436.15|Wait=8 Start=0 Read=0 Send=1 Keepalive=1 DNS=0 Close=0 Logging=0 Graceful=0 Idle=0 Free=140 ReqPerSec=.206872 BytesPerSec=1124.59 BytesPerReq=5436.15
```

###### apache 2.2
![Screenshot apache 2.2](/screenshots/diagramm-1.png?raw=true "Apache 2.2")

###### apache 2.4 with ReqPerSec, BytesPerReq and BytesPerSec
![Screenshot apache 2.4](/screenshots/diagramm-2.png?raw=true "Apache 2.4")
