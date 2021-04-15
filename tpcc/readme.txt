sudo yum install -y java ant git
sudo yum install http://mirror.centos.org/centos/8/PowerTools/x86_64/os/Packages/apache-ivy-2.4.0-14.module_el8.0.0+30+832da3a1.noarch.rpm
git clone https://github.com/yugabyte/tpcc.git
cd tpcc
ant bootstrap
ant resolve
ant build

./tpccbenchmark -c config/workload_all.xml --clear=true
./tpccbenchmark -c config/workload_all.xml --create=true --warehouses=1
./tpccbenchmark -c config/workload_all.xml --load=true --loaderthreads=1


./tpccbenchmark -c config/workload_short.xml --execute=true --nodes=yb-1,yb-2,yb3 --warehouses=1 --num-connections=6 -s 5 -o tt

./tpccbenchmark -c config/workload_all.xml --execute=true -s 5 -o outputfile
