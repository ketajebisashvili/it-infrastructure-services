FIX

# Install and configure infrastructure with Ansible:

## Install/Restore with Ansible
Anyone with the privatekey and a local repository from https://github.com/ketajebisashvili/ica0002 can backup/restore the services the following way:

### Install/Restore Infrastructure

#### Use this command if you don't know what is the issue!
#### This command should restore the entire infrastructure to the desired state:
    ansible-playbook infra.yaml

If you know the issue is with the backup, it is mandatory to first run the backup step (mentioned on the bottom) and only after you should run this command!


### Install/Restore individual modules

This command will collect information about the vm-s:

    ansible-playbook infra.yaml --tags se

This command will install/restore duplicity service on the vm-s:
    
    ansible-playbook infra.yaml --tags in

This command will install/restore bind service on the vm-s:
    
    ansible-playbook infra.yaml --tags bi

This command will fix the docker container building and image creation SSL handshake error on the vm-s:
    
    ansible-playbook infra.yaml --tags ip

This command will install/restore MySQL service and create the mysql backup cron file for agama and user information on the vm-s:
    
    ansible-playbook infra.yaml --tags my

This command will install/restore docker service on the vm-s:
    
    ansible-playbook infra.yaml --tags do

This command will install/restore nginx service on the vm-s:
    
    ansible-playbook infra.yaml --tags ng

This command will install/restore uwsgi service and create the Agama docker image on the vm-s:
    
    ansible-playbook infra.yaml --tags we

This command will install/restore prometheus service on the vm-s:
    
    ansible-playbook infra.yaml --tags pr

This command will install/restore influxdb, telegraf and create the influxdb backup cron file for influxdb log service on the vm-s:
    
    ansible-playbook infra.yaml --tags inte

This command will install/restore pinger service on the vm-s:
    
    ansible-playbook infra.yaml --tags ping

This command will install/restore rsyslog service on the vm-s:
    
    ansible-playbook infra.yaml --tags rs

This command will install/restore keepalived  service on the vm-s:
    
    ansible-playbook infra.yaml --tags ke

This command will install/restore haproxy  service on the vm-s:
    
    ansible-playbook infra.yaml --tags ha

 This command will install/restore grafana service by creating the Grafana docker image on the vm-s:
    
    ansible-playbook infra.yaml --tags gr

This command will install/restore all exporter services on the vm1:
    
    ansible-playbook infra.yaml --tags vm1exporters

This command will install/restore all exporter services on the vm2:
    
    ansible-playbook infra.yaml --tags vm2exporters

This command will install/restore all exporter services on the vm3:
    
    ansible-playbook infra.yaml --tags vm3exporters
   


### Install/Restore service modules

This command will install/restore all application services on the vm-s:

    ansible-playbook infra-yaml --tags application

This command will install/restore all internal services on the vm-s:

    ansible-playbook infra-yaml --tags internal

This command will install/restore all database services with their backup utilities on the vm-s:

    ansible-playbook infra-yaml --tags database

This command will install/restore all exporter services on the vm-s:

    ansible-playbook infra-yaml --tags exporter

This command will install/restore all dns services on the vm-s:

    ansible-playbook infra-yaml --tags dns


### Install/Restore Infrastructure

#### Use this command if you dont know what is the issue!
#### This command should restore the entire infrastructure for the desired state:
    ansible-playbook infra.yaml

If you know the issue is with the backup, it is mandatory to first run the backup step (mentioned below) and only after you should run this command!

# Restore MySQL data from the backup:

Since backup user is not configured properly, the easiest way is to first login to the user backup, then use duplicity to get
the backup from the remote server. The tail command will ensure you, that the backup exist and we can use it.
The last step will recover the database and drop the old one.


1. sudo su - backup
2. rm -r restore/
3. duplicity --no-encryption restore rsync://ketajebisashvili@backup.tay.vk//home/ketajebisashvili/ /home/backup/restore/agama
4. exit
5. sudo su -
6. mysql agama < /home/backup/restore/agama.sql

        sudo su - backup

        rm -r restore/

        duplicity --no-encryption restore rsync://ketajebisashvili@backup.tay.vk//home/ketajebisashvili/ /home/backup/restore/agama

        exit

        sudo su -

        mysql agama < /home/backup/restore/agama/agama.sql


# Restore InfluxDB data from the backup:


In the first step, you have to become backup user in order to make the second step, which is to download backup from the remote server.
In the thirs step, you have exit user backup, to login to user root in step 4. Then, after stopping the telegraf service, influxdb will drop its database.
In step 7, we restore the EMPTY database with the file that we downlaoded, then start the telegraf service in the last step.

1. sudo su - backup
2. rm -r restore/
3. duplicity --no-encryption restore rsync://ketajebisashvili@backup.tay.vk//home/ketajebisashvili/ /home/backup/restore/
4. exit
5. sudo su -
6. service telegraf stop
7. influxd restore -portable -database telegraf /home/backup/restore/
8. service telegraf start

        sudo su - backup

        rm -r restore/

        duplicity --no-encryption restore rsync://ketajebisashvili@backup.tay.vk//home/ketajebisashvili/ /home/backup/restore/

        exit

        sudo su -

        influxd restore -portable -database telegraf /home/backup/restore/

        service telegraf start

