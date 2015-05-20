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

## Sample code

[docker@88ea8d0efb70 www]$ vim /var/www/hello.php 
<?php
$cluster = new CouchbaseCluster('192.168.33.10:8091');
$db = $cluster->openBucket('default');
$db->upsert('hello', array('name'=>'world','phone'=>'123-456-7890'));
$res = $db->get('hello');
var_dump($res->value);

[docker@88ea8d0efb70 www]$ php hello.php 
PHP Warning:  Module 'json' already loaded in Unknown on line 0
object(stdClass)#4 (2) {
  ["name"]=>
  string(5) "world"
  ["phone"]=>
  string(12) "123-456-7890"
}
