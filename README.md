# xymon2Influxdb
Script to process and resend metric data from xymon hosts (rrd) to Influxdb. From acens technologies.

## dependencies
```bash
sudo pip3 install influxdb
sudo pip3 install toml
```
## activate script
Download script from githup:
```bash
cd /usr/local/bin
git clone https://github.com/ragonlan/xymon2Influxdb
```
copy configuration to xymon config dir:
```bash
cp /usr/local/bin/xymon2influxdb/xymon2Influxdb.toml /etc/xymon
```
change config if it's needed.

create logs files with right permisons:
```bash
touch /usr/log/xymon2Influxdb.base.log /usr/log/xymon2Influxdb.data.log && chown xymon /usr/log/xymon2Influxdb.base.log
```
Edit task.cfg and modify rrdstatus and rrddata seccion:
```ini
[rrdstatus]
        ENVFILE /usr/lib/xymon/server/etc/xymonserver.cfg
        NEEDS xymond
        CMD xymond_channel --channel=status --log=$XYMONSERVERLOGS/rrd-status.log xymond_rrd --rrddir=$XYMONVAR/rrd --processor="/usr/local/bin/xymon2Influxdb/xymon2Influxdb.py" --desc=base

[rrddata]
        ENVFILE /usr/lib/xymon/server/etc/xymonserver.cfg
        NEEDS xymond
        CMD xymond_channel --channel=data  --log=$XYMONSERVERLOGS/rrd-data.log xymond_rrd --rrddir=$XYMONVAR/rrd --processor="/usr/local/bin/xymon2Influxdb/xymon2Influxdb.py" --desc=data
```

## Configracion
configfile is a toml file type:
```toml
# This is a TOML document.
[xymon]  # xymon server hostname. No fqdn
dbname = "xymon"   # Do not use admin user please!!!!
user = "admin"
password = "ChangeMeNOW!"
logfile = "/var/log/xymon2Influxdb.base.log"
metrics2Proccess = 30
verbose = false
[xymon.data]
logfile = "/var/log/xymon2Influxdb.data.log"
```
