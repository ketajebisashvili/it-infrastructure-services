# Backup coverage
In the backup procedure following services are backed up:
- MySQL database - ketajebisashvili-2
- InfluxDB - ketajebisashvili-3

Backups are issued for these two services, since InfluxDB Telegraf and MySQL Agama database services, contains user and log information, which is only restorable by backup service.

MySQL database includes customer data, which is vital to the recovery of this infrastructure, thus it is important that we keep the database safe.

InfluxDB database includes log data, which is vital to the recovery of this infrastructure, thus it is important that we keep the database safe.

<br>

## BACKUP SLA

### Services

#### Backup and Recovery Services 

Client has chosen our Services at his own discretion and in the case of any data loss will no hold the software creator accountable. tay.vk provides a system, which houses a backup of Client’s designated servers (VMs), which is described in the Backup Coverage and specifications for backup and recovery services that we will design and periodically update.

#### _ System Summary_

---------- Infrastructure services: ----------

Web services - Nginx

App services - Agama

Database services - MySQL, InfluxDB

DNS service - Bind9

Monitoring services - Prometheus, Influxdb exporter, Pinger, Bind9 exporter, MySQL exporter, Nginx exporter, Node exporter, Rsyslog, Telegraf, Grafana, HAProxy exporter, Keepalived exporter

Backup services - Duplicity

Additional services - Ansible, uWSGI, Cron

Containerisation services - Docker

Load balancing - HAProxy, Keepalived

Ansible repository - <https://github.com/ketajebisashvili/ica0002> (stores configuration, files, playbooks)

Recovery point objective:


#### Local copies of the databases are created every day at:
+ 01:00 AM UTC+2 Estonian time for MySQL
+ 01:15 AM UTC+2 Estonian time for MySQL
###### This helps keep the data updated till the backup happens

#### Full backup of the databases are created every Sunday at:
+ 01:05 AM UTC+2 Estonian time for MySQL
+ 01:20 AM UTC+2 Estonian time for MySQL

#### Incremetal backup of the databases are created every Monday at:
+ 01:10 AM UTC+2 Estonian time for MySQL
+ 01:25 AM UTC+2 Estonian time for MySQL

#### Explanation:
The 01:10 - 01:30 time window was chosen as the time with the least activity in the region when our service is provided, some backed up services may be in the read-only mode or shutdown.
The time difference between the backups are desiged:
+ For the maximum process time of a copy
+ For the trustworthy difference in the backup naming provided by duplicity service



#### # Versioning and retention

-   InfluxDB

Backups are retained for 4 weeks

-   MySql

Backups are retained for 4 weeks

Every day all backups uploaded to [ketajebisashvili@backup.tay.vk]

We assume that full backup might take up to 1 hour.


#### # Usability

From our side we are providing verification of usability on the weekly bases. We are checking the time of the backup creation, the readability of the stored backup, completion of the stored backup and verifying the possiility of restoration to the needed state.

#### # Restoration Criteria

The data needed for full recovery are available and can be accessed as needed.
There are polices in place to validate the disaster recovery restoration:

-   Backup restoration of all the services should be done only if confirmed that the data was corrupted or modified, stolen or deleted.
    Confirmation must be done to avoid unnecessary usage of computing resources.

#### # Backup RTO (recovery time objective)

Our infastructure has the ability to estimate the length of time it will take for data recovery.

Estimated time of backup per service approximately 1 hour for complete recovery. In case we need the restoration of all services approximately 1 hour is required.

#### ALL SERVICES PROVIDED BY tay.vk
