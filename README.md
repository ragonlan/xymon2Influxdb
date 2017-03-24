# xymon2Influxdb
Script to process and resend metric data from xymon hosts (rrd) to Influxdb. From acens technologies.

## activate script
Download script from githup:

cd /usr/local/bin
git clone https://github.com/ragonlan/xymon2Influxdb

copy configuration to xymon config dir:
cp /usr/local/bin/xymon2influxdb/xymon2Influxdb.toml

change config if is needed.

create logs files with right permisons:

touch /usr/log/xymon2Influxdb.base.log /usr/log/xymon2Influxdb.data.log && chown xymon /usr/log/xymon2Influxdb.base.log

Edit task.cfg and modify rrdstatus and rrddata seccion:

[rrdstatus]
        ENVFILE /usr/lib/xymon/server/etc/xymonserver.cfg
        NEEDS xymond
        CMD xymond_channel --channel=status --log=$XYMONSERVERLOGS/rrd-status.log xymond_rrd --rrddir=$XYMONVAR/rrd --processor="/usr/local/b
in/xymon2Influxdb/xymon2Influxdb.py" --desc=base


[rrddata]
        ENVFILE /usr/lib/xymon/server/etc/xymonserver.cfg
        NEEDS xymond
        CMD xymond_channel --channel=data  --log=$XYMONSERVERLOGS/rrd-data.log xymond_rrd --rrddir=$XYMONVAR/rrd --processor="/usr/local/bin/xymon2Influxdb/xymon2Influxdb.py" --desc=data

