## ndb_mgmd docker build
FROM debian:8.2
MAINTAINER Jonathan Lowsley <jlowsley@gmail.com>

# set archive file
ENV MYSQL_CLUSTER_TARBALL mysql-cluster-gpl-7.4.6-linux-glibc2.5-x86_64.tar.gz

# install wget to pull archive from URL
RUN export DEBIAN_FRONTEND="noninteractive" \
    && apt-get update \
    && apt-get install -y curl \
    && echo "America/Los_Angeles" > /etc/timezone \
    && dpkg-reconfigure tzdata


# use bash wrapper to get variable substitution
# divine variables 
# pull archive and extract and delete archive in one step to minimize layer size and avoid the tarball getting hemmed in
RUN ["/bin/bash","-c", "\
    MYSQL_CLUSTER_MAJOR_VERSION=${MYSQL_CLUSTER_TARBALL:18:3} ;\
    MYSQL_CLUSTER_DIR=${MYSQL_CLUSTER_TARBALL//.tar.gz/} ;\
    MYSQL_CLUSTER_URL=http://dev.mysql.com/get/downloads/mysql-cluster-${MYSQL_CLUSTER_MAJOR_VERSION}/${MYSQL_CLUSTER_TARBALL} ;\
    cd /usr/local \
    && curl -s -L -O $MYSQL_CLUSTER_URL \
    && tar xzf ${MYSQL_CLUSTER_TARBALL} \
    && rm -f ${MYSQL_CLUSTER_TARBALL} \
    && ln -s ${MYSQL_CLUSTER_DIR} mysql \
    && chown -R root: /usr/local/mysql \
    && mkdir -p /var/lib/mysql-cluster/BACKUP \
    "] 

# define entrypoint with static options, add additional args through docker run
ENTRYPOINT ["/usr/local/mysql/bin/ndb_mgmd","--nodaemon","--config-file=/var/lib/mysql-cluster/config.ini","--config-dir=/var/lib/mysql-cluster/","--skip-config-cache"]

