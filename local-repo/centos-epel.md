yum install epel-release -y

yum install nginx -y

systemctl start nginx

systemctl enable nginx

yum install createrepo  yum-utils -y

chown -R nginx:nginx /data


vi /etc/nginx/nginx.conf
nginx -s reload


Repo
vi /etc/cron.daily/update-centos-repo
#!/bin/bash
DIR=/share/CentOS/7.7.1908/
for REPO in base centosplus extras updates epel
do
     reposync -g -l -d -m --repoid=$REPO --newest-only --download-metadata --download_path=${DIR}

     if [ $REPO = 'base' ]
     then
          createrepo -g comps.xml ${DIR}${REPO}/
     else
          createrepo ${DIR}${REPO}/
     fi
done
