# docker-ndb_mgmd
Docker build for MySQL Cluster ndb_mgmd container

    git clone https://github.com/jlowsley/docker-ndb_mgmd.git

    cd docker-ndb_mgmd/7.1.29
    docker build username/ndb_mgmd:7.1.29 .

    # dbid=<arbitrary cluster id>
    dbid=31
    
    # node=<ndb cluster node id>
    nodeid=2
    
    # bindaddr=<node ip address>
    bindaddr=172.16.10.2
    
    # docker run container and pass args
    docker run -d --name ndb_mgmd_${dbid}_node${nodeid} --net=host \
    -p ${bindaddr}:1186:1186 \
    -v /var/lib/mysql-cluster/${dbid}:/var/lib/mysql-cluster \
    -v /mnt/nfssrv/somevol/ndbcluster_backups/${dbid}:/var/lib/mysql-cluster/BACKUP \
    username/ndb_mgmd:7.1.29 --ndb-nodeid=${nodeid} --bind-address=${bindaddr}

Helpful aliases (*custom*)

    cat >> /root/.bashrc << 'EOF'
    alias init31="docker run -d --name ndb_mgmd_db31_node2 --net=host -p 172.16.21.2:1186:1186 -v /var/lib/mysql-cluster/db31:/var/lib/mysql-cluster -v /mnt/nfssrv/somevol/ndbcluster_backups/${dbid}:/var/lib/mysql-cluster/BACKUP jlowsley/ndb_mgmd:7.1.29 --ndb-nodeid=2 --bind-address=172.16.21.2" 
    alias start31="docker start \`docker ps -a --filter='name=ndb_mgmd_db31_node2' | tail -1 | awk '{print \\\$1}'\`" 
    alias stop31="/var/lib/mysql-cluster/db31/bin/ndb_mgm -c 172.16.21.2 -e '2 stop'" 
    alias ndb31="/var/lib/mysql-cluster/db31/bin/ndb_mgm -c 172.16.21.2" 
    EOF

