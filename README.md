# couchlamp

Couchbase PHP client for Dockerfile

>docker build -t="bluemoon/couchlamp" .

>docker run -i -t -d -p 9080:80 -p 9022:22 -p 9006:3306 --name couchlamp -v /vagrant:/vagrant:rw bluemoon/couchlamp


you will check it

>ssh -p 9022 docker@192.168.33.10
>vim /var/www/info.php
  <?php
  phpinfo();

then watch browser http://192.168.33.10/info.php

## couchbase

couchbase support	enabled
version	1.2.0
libcouchbase version	2.5.0
json support	yes
fastlz support	yes
zlib support	yes
igbinary support	no

Directive	Local Value	Master Value
couchbase.compression_factor	1.3	1.3
couchbase.compression_threshold	2000	2000
couchbase.compressor	none	none
couchbase.config_cache	no value	no value
couchbase.durability_default_poll_interval	100000	100000
couchbase.durability_default_timeout	40000000	40000000
couchbase.instance.persistent	On	On
couchbase.restflush	On	On
couchbase.serializer	php	php
couchbase.skip_config_errors_on_connect	Off	Off
couchbase.view_timeout	75	75
