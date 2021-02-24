sudo yum install autoconf automake gcc gcc-c++ make libtool postgresql-devel
git clone https://github.com/yugabyte/sysbench.git
cd sysbench
./autogen.sh
./configure --with-pgsql --without-mysql
make -j
sudo make install
sysbench oltp_point_select --tables=10 --table_size=1000000 --range_key_partitioning=true --db-driver=pgsql --pgsql-host=127.0.0.1 --pgsql-port=5433 --pgsql-user=yugabyte --pgsql-db=yugabyte prepare
sysbench oltp_point_select --tables=10 --table_size=1000000 --range_key_partitioning=true --db-driver=pgsql --pgsql-host=127.0.0.1 --pgsql-port=5433 --pgsql-user=yugabyte --pgsql-db=yugabyte --threads=4 --time=120 --warmup-time=10 run
sysbench oltp_read_only --tables=10 --table_size=1000000 --range_key_partitioning=true --db-driver=pgsql --pgsql-host=127.0.0.1 --pgsql-port=5433 --pgsql-user=yugabyte --pgsql-db=yugabyte --threads=4 --time=120 --warmup-time=10 run
sysbench oltp_point_select --tables=10 --table_size=1000000 --range_key_partitioning=true --db-driver=pgsql --pgsql-host=127.0.0.1 --pgsql-port=5433 --pgsql-user=yugabyte --pgsql-db=yugabyte cleanup
