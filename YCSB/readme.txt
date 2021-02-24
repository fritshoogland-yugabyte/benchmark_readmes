wget https://github.com/yugabyte/YCSB/releases/download/1.0/ycsb.tar.gz
tar xzf ycsb.tar.gz
cd YCSB
vi db.properties
hosts=127.0.0.1
port=9042
cassandra.username=yugabyte

