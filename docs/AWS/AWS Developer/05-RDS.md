# RDS

**`AWS RDS`** (Relational Database Service): 

* **MS SQL Server** (port 1433) (up to 16TB of storage when using the Provisioned IOPS and General Purpose SSD storage)
* **Oracle**. Includes license model (BYOL - Bring your own license)
* **MariaDB**
* **MySQL** (port 3306)
* **PostgreSQL** (port 5432)
* **Amazon Aurora**. MySQL and PostgreSQL compatibility. **No free** tier. This is up to 5X **faster** than a traditional MySQL database

**RDS** for OLTP (OnLine Transation Processing) real-time transactions
**RedShift** for OLAP (OnLine Analitics Processing) complex queries to analyze historical data

RDS Storage autoscaling for Sequel Hub???
20 gb of snapshots in sbs-hub-rds-uat?

multiAZ is:

* a primary rds and a standby rds in a different az
* for DR. it does not improve performance

read replica is a read-only copy of the primary rds to improve read performance. the replica can be:

* at the same AZ
* at a different AZ
* at a different region

read replicas has endpoints to allow the connection. multiaz standby rds do not have an endpoint so you can no connect directly

read replicas requires automatic backup

maximum of 5 read replicas