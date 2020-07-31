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
