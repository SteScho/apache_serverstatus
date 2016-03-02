# apache_serverstatus
nagios, icinga, ... plugin and pnp4nagios template for apache server-status

This plugin uses apaches server-status (mod_status) to collect information about your running apache process. It uses wget to receive that data. For more details look at the examples below.

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

Testing apache server-status with wget:
```
wget -q -O- http://localhost/server-status?auto
Total Accesses: 229725
Total kBytes: 1281134
CPULoad: .000495073
Uptime: 1026111
ReqPerSec: .223879
BytesPerSec: 1278.5
BytesPerReq: 5710.66
BusyWorkers: 1
IdleWorkers: 9
Scoreboard: _.___._...___..W_.....................................................................................................................................
```

```
./check_apache_serverstatus.pl --wc=.,@130:,@145: --wc=BytesPerSec,1000,2000
WARNING APACHE SERVER STATUS Wait=8, Start=0, Read=0, Send=1, Keepalive=1, DNS=0, Close=0, Logging=0, Graceful=0, Idle=0, Free=140 ===> Total=150 RATES ReqPerSec=.206872, BytesPerSec=1124.59, BytesPerReq=5436.15|Wait=8 Start=0 Read=0 Send=1 Keepalive=1 DNS=0 Close=0 Logging=0 Graceful=0 Idle=0 Free=140 ReqPerSec=.206872 BytesPerSec=1124.59 BytesPerReq=5436.15
```

You want to know in detail what happens? Use -v Flag!
```
./check_apache_serverstatus# ./check_apache_serverstatus.pl --wc=.,@130,@145 --wc=BytesPerSec,1000,2000 -v
$options = {
             'verbose' => 1,
             'hostname' => 'localhost',
             'wget' => '/usr/bin/wget',
             'wget-options' => '-q --no-check-certificate -O-',
             'help' => undef,
             'version' => undef,
             'warncrit' => [
                             '.,@130,@145',
                             'BytesPerSec,1000,2000'
                           ],
             'url' => undef
           };
$warncrit = {
              'BytesPerSec' => {
                                 'w' => '1000',
                                 'c' => '2000'
                               },
              '.' => {
                       'w' => '@130',
                       'c' => '@145'
                     }
            };
Url: http://localhost/server-status?auto
$server-status = {
                   'Total kBytes' => '1281078',
                   'Scoreboard' => '_.__W._...___..__.....................................................................................................................................',
                   'BytesPerSec' => '1278.78',
                   'BusyWorkers' => '1',
                   'BytesPerReq' => '5710.78',
                   'Uptime' => '1025840',
                   'Total Accesses' => '229710',
                   'ReqPerSec' => '.223924',
                   'IdleWorkers' => '9',
                   'CPULoad' => '.000494229'
                 };
$scoreboard = {
                '.' => 140,
                'W' => 1,
                '_' => 9
              };
checking warn/crit for "BytesPerSec"...
  value: "1278.78"
    checking type critcal = 2000
    inside or outside: out   min: 0   max: 2000
    checking type warning = 1000
    inside or outside: out   min: 0   max: 1000
  result: 1
checking warn/crit for "."...
  value: "140"
    checking type critcal = @145
    inside or outside: in   min: 0   max: 145
  result: 2
Check overall status: 2
CRITICAL APACHE SERVER STATUS Wait=9, Start=0, Read=0, Send=1, Keepalive=0, DNS=0, Close=0, Logging=0, Graceful=0, Idle=0, Free=140 ===> Total=150 RATES ReqPerSec=.223924, BytesPerSec=1278.78, BytesPerReq=5710.78|Wait=9 Start=0 Read=0 Send=1 Keepalive=0 DNS=0 Close=0 Logging=0 Graceful=0 Idle=0 Free=140 ReqPerSec=.223924 BytesPerSec=1278.78 BytesPerReq=5710.78
```

###### apache 2.2
![Screenshot apache 2.2](https://raw.githubusercontent.com/SteScho/apache_serverstatus/master/screenshots/diagramm-1.png?raw=true "Apache 2.2")

###### apache 2.4 with ReqPerSec, BytesPerReq and BytesPerSec
![Screenshot apache 2.4](https://raw.githubusercontent.com/SteScho/apache_serverstatus/master/screenshots/diagramm-2.png?raw=true "Apache 2.4")
