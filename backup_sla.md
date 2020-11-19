# Backup services SLA

## Introduction

### Definition of SLA

This Service Level Agreement (this “SLA”) governs the backup Services. Operations and security teams may update, amend, modify or supplement this SLA from time to time. The terms and conditions of this SLA are applicable to the Managed Backup and Disaster Recovery Services only.  

### Structure of SLA

This SLA describes the backup approach for all services in the infrastructure:  

- Web services - **Nginx**
- App services - **Agama**
- Database services - **MySQL, InfluxDB**
- DNS service - **Bind9**
- Monitoring services - **Prometheus, Grafana, Telegraf**
- Infra as a Code - **Ansible** <https://github.com/shyamajp/ica0002>

Descriptions of backup approaches contain information on specific backup attributes, such as:

- Backup coverage - what is backed up and what is not
- Backup RPO (recovery point objective) - acceptable data loss (time period)
- Versioning and retention - how many backup versions are stored and for how long
- Usability - how is the backup recovery verified (backup should be usable)
- Restoration criteria - when should backup be restored
- Backup RTO (recovery time objective) - how long will it take to restore the service

## Backup coverage

Backup service covers only the next services:

- Database services - **MySQL, InfluxDB**
- Monitoring services - **Grafana**, **Prometheus**
- Ansible repository

**Explanation:**  
> Every service that affects users will be stored fully such as MySQL and Grafana. Some of logging information can be less prioritized as it will not bother my services running in case of disaster.

Other services do not include user provided information, thus it is decided that it will not in need of backup as all can be recovered by Ansible playbook. As all services are run by Ansible, Ansible repository itself will be stored in GitHub and GitLab.

## Backup RPO

Recovery point objective for:

|Service|RPO|
|---|---|
|MySQL|7 days|
|InfluxDB|28 days|
|Prometheus|28 days|
|Grafana|14 days|
|Ansible|7 days|

**Time:** backups should be done automatically at 1 am with full backup on Sundays and incremental backup on other days.

**Explanation:**  
Considering that most people sleep at night, all backups will be done at night. To save space and keep consistancy, full backups are done on Sundays and other days, it will continue with incremental backup.

## Versioning and retention

All services will be stored for maximum 30 days with 4 full versions.

**Explanation:**  
all services must be restored before 30 days. And 4 Sundays with 4 full backups should pass in the past 30 days, thus 4 versions in total.

## Usability

All services will be checked every week on Wednesday, only in development environment.

**Explanation:**  
Things must be done with attention, thus all usability tests will be conducted in development environment to avoid any mistakes.

## Restoration criteria
- Backup restoration of the **MySQL**, **InfluxDB** or **Prometheus** data should be done only if it was detected and confirmed that the stored data was corrupted, modified by the unauthorized 1st/3rd party, stolen or deleted.
- Backup restoration of the **Grafana** configuration should be done only if it was detected and confirmed that the service configuration was corrupted, modified by the unauthorized 1st/3rd party, stolen or deleted.
- **Ansible repository** backup should only be used if the reconfiguration of services is required, and original GitHub based repository is unavailable, or the process of acquiring and using a backup copy is faster than acquiring and using GitHub's copy.

**Explanation:**  
Backup restoration takes time and computing resources, there is no reason to use it, if the data it covers, was not corrupted.

## Backup RTO
- Backup service is expected to take maximum of **12 hours** to fully recover all the data.
- If the whole infrastructures should be restored, excepted maximum required time equals **24 hours**.

**Explanation:**  
Since my services are not huge, it should all be recovered within a day.