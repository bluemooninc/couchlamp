# couchlamp

Couchbase PHP client for Dockerfile

docker build -t="bluemoon/couchlamp" .

docker run -i -t -d -p 9080:80 -p 9022:22 -p 9006:3306 --name couchlamp -v /vagrant:/vagrant:rw bluemoon/couchlamp

