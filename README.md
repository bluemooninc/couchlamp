# couchlamp

Couchbase PHP client for Dockerfile
```
>docker build -t="bluemoon/couchlamp" .
>docker run -i -t -d -p 9080:80 -p 9022:22 -p 9006:3306 --name couchlamp -v /vagrant:/vagrant:rw bluemoon/couchlamp
```


you will check it
```
>ssh -p 9022 docker@192.168.33.10
>vim /vagrant/html/info.php
  <?php
  phpinfo();
```

## phpinfo

watch browser http://192.168.33.10/info.php
```
couchbase
Version	2.0.7
```

## Sample code

[docker@88ea8d0efb70 www]$ vim /vagrant/html/hello.php 
```
<?php
$cluster = new CouchbaseCluster('192.168.33.10:8091');
$db = $cluster->openBucket('default');
$db->upsert('hello', array('name'=>'world','phone'=>'123-456-7890'));
$res = $db->get('hello');
var_dump($res->value);
```
[docker@88ea8d0efb70 www]$ php hello.php 
```
PHP Warning:  Module 'json' already loaded in Unknown on line 0
object(stdClass)#4 (2) {
  ["name"]=>
  string(5) "world"
  ["phone"]=>
  string(12) "123-456-7890"
}
```
