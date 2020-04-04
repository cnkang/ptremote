FROM mariadb:latest

ENV DEBIAN_FRONTEND noninteractive 
COPY db-dump.sql /docker-entrypoint-initdb.d/ 

ENTRYPOINT ["docker-entrypoint.sh"]


