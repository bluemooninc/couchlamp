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

## Simple Insert Sample

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

## CRUD Sample
```
<?php
/**
** Couchbase CRUD smaple
**/
$myCluster = new CouchbaseCluster('192.168.33.10:8091');
$myBucket = $myCluster->openBucket();
$bucketName = 'test_curd';

echo "Creating:";
$res = $myBucket->insert($bucketName, array('some'=>'value'));

echo "Retrieving:";
$res = $myBucket->get($bucketName);
var_dump($res);

echo "Updating:";
$res = $myBucket->replace($bucketName, array('some'=>'value change'));
$res = $myBucket->get($bucketName);
var_dump($res);

echo "Deleting";
$res = $myBucket->remove($bucketName);
var_dump($res);
```

## How to edit Couchbase Views

Browse http://192.168.33.10:8091

Create View on your couch server
beer-sample > Views > Create Development View

Edit View code on Map and save it.
You will see it beer-sample > Views > _design/dev_beer/_view/by_name
```
function (doc, meta) {
    if (doc.type && doc.type == "beer") {
        emit(doc.name, null);
    }
}
```

## Beer-sample 
```
<?php
/**
** Couchbase beer-smaple
**/
define("COUCHBASE_CONNSTR", "192.168.33.10:8091");
define("COUCHBASE_BUCKET", "beer-sample");
define("INDEX_DISPLAY_LIMIT", 20);

$myCluster = new CouchbaseCluster(COUCHBASE_CONNSTR);
$myBucket = $myCluster->openBucket();

// Some input from GET/POST
$input = 'brew';

// Connecting to Couchbase
$cluster = new CouchbaseCluster(COUCHBASE_CONNSTR);
$myBucket = $cluster->openBucket(COUCHBASE_BUCKET);

// Define the Query options
$options = array(
  'limit' => INDEX_DISPLAY_LIMIT, // Limit the number of returned documents
  'startkey' => $input, // Start the search at the given search input
  'endkey' => $input . '\uefff' // End the search with a special character (see explanation below)
);
//
// Retrieve a document 
// You have to make a ViewMap on the Couchbase Admin Tool
// beer-sample > Views > _design/dev_beer/_view/by_name
//
$query = CouchbaseViewQuery::from('dev_beer', 'by_name')
	->limit(INDEX_DISPLAY_LIMIT)
	->range($input, $input . '\uefff')
	->order(CouchbaseViewQuery::ORDER_ASCENDING);
$results = $myBucket->query($query);

// Do something for result value
foreach($results['rows'] as $row) {
    // Load the full document by the ID
    $ret = $myBucket->get($row['id']);
    if($ret) {
        // Decode the JSON string into a PHP array
        $doc = json_decode($ret->value, true);
        $beers[] = array(
            'name' => $doc['name'],
            'brewery' => $doc['brewery_id'],
            'id' => $row['id']
        );
    }
}
var_dump($beers);
```

### php beer.php

Run on terminal
```
[docker@a722a4c57724 couchbase]$ php beer.php 
array(5) {
  [0]=>
  array(3) {
    ["name"]=>
    string(11) "Brew Ribbon"
    ["brewery"]=>
    string(17) "seabright_brewery"
    ["id"]=>
    string(29) "seabright_brewery-brew_ribbon"
  }
  [1]=>
  array(3) {
    ["name"]=>
    string(16) "Brewhouse Blonde"
    ["brewery"]=>
    string(27) "bj_s_restaurant_and_brewery"
    ["id"]=>
    string(44) "bj_s_restaurant_and_brewery-brewhouse_blonde"
  }
  [2]=>
  array(3) {
    ["name"]=>
    string(22) "Brewhouse Coffee Stout"
    ["brewery"]=>
    string(30) "central_waters_brewing_company"
    ["id"]=>
    string(53) "central_waters_brewing_company-brewhouse_coffee_stout"
  }
  [3]=>
  array(3) {
    ["name"]=>
    string(15) "Brewhouse Lager"
    ["brewery"]=>
    string(26) "sacramento_brewing_company"
    ["id"]=>
    string(42) "sacramento_brewing_company-brewhouse_lager"
  }
  [4]=>
  array(3) {
    ["name"]=>
    string(30) "Brewtality Espresso Black Bier"
    ["brewery"]=>
    string(23) "midnight_sun_brewing_co"
    ["id"]=>
    string(54) "midnight_sun_brewing_co-brewtality_espresso_black_bier"
  }
}
```
