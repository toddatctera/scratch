# Postgres Cheat Sheet

## Prepared statements to check Postgres stats, waiting queries, and table locks.
	~postgres/bin/psql -U postgres -f ~postgres/stat.sql
	~postgres/bin/psql -U postgres -f ~postgres/waiting.sql
	~postgres/bin/psql -U postgres -f ~postgres/psqllocks.sql
	
	~postgres/bin/psql -U postgres -f ~postgres/statFull.sql
	~postgres/bin/psql -U postgres -f ~postgres/waitingFull.sql
	~postgres/bin/psql -U postgres -f ~postgres/psqllocksFull.sql

## XML Parsing in Postgres

	((xpath('//att[@id="hostname"]/val/text()', XMLPARSE(DOCUMENT base_objects.xml_field)))[1]::text::text) 
	((xpath('//att[@id="osId"]/val/text()', XMLPARSE(DOCUMENT base_objects.xml_field)))[1]::text::text) 
	((xpath('//att[@id="guid"]/val/text()', XMLPARSE(DOCUMENT base_objects.xml_field)))[1]::text::text) 

Example

```
	SELECT uid,
       name,
       TYPE,
       ((xpath('//att[@id="osId"]/val/text()', XMLPARSE(DOCUMENT base_objects.xml_field)))[1]::text::text) AS osID
	FROM base_objects
	WHERE TYPE='com.ctera.db.objects.ManagedDevice';
	 uid |      name      |                type                |                 osid
	-----+----------------+------------------------------------+--------------------------------------
	 522 | vGateway-1b6c  | com.ctera.db.objects.ManagedDevice |
	 530 | todd-edgenisis | com.ctera.db.objects.ManagedDevice |
	 531 | todd-vGateway  | com.ctera.db.objects.ManagedDevice |
	 543 | VTODD-WIN      | com.ctera.db.objects.ManagedDevice | 960db7e8-0fc6-42df-b4e0-b0b76ee59550
	(4 rows)
```
### Find managed devices and their OSId and GUID.
	SELECT uid,name,((xpath('//att[@id="hostname"]/val/text()', XMLPARSE(DOCUMENT base_objects.xml_field)))[1]::text::text) AS hostname, ((xpath('//att[@id="osId"]/val/text()', XMLPARSE(DOCUMENT base_objects.xml_field)))[1]::text::text) as osID, ((xpath('//att[@id="guid"]/val/text()', XMLPARSE(DOCUMENT base_objects.xml_field)))[1]::text::text) AS guid FROM base_objects WHERE type='com.ctera.db.objects.ManagedDevice';

Result
```
 uid |      name      |   hostname    |                 osid                 |                  guid
-----+----------------+---------------+--------------------------------------+----------------------------------------
 522 | vGateway-1b6c  | vGateway-1b6c |                                      |
 530 | todd-edgenisis |               |                                      |
 531 | todd-vGateway  |               |                                      |
 543 | VTODD-WIN      | VTODD-WIN     | 960db7e8-0fc6-42df-b4e0-b0b76ee59550 | {886d407c-1b07-4a89-b31a-8e2ef2894d16}
(4 rows)
```

### Find managed devices where the device name does not match the hostname
```
SELECT subq.*,
       bo.name AS owner_name
FROM
  (SELECT uid,
          owner_id,
          name,
          ((xpath('//att[@id="hostname"]/val/text()', XMLPARSE(DOCUMENT base_objects.xml_field)))[1]::text::text) AS hostname, ((xpath('//att[@id="osId"]/val/text()', XMLPARSE(DOCUMENT base_objects.xml_field)))[1]::text::text) as osID, ((xpath('//att[@id="guid"]/val/text()', XMLPARSE(DOCUMENT base_objects.xml_field)))[1]::text::text) AS guid
   FROM base_objects
   WHERE TYPE='com.ctera.db.objects.ManagedDevice') AS subq
JOIN base_objects AS bo ON subq.owner_id = bo.uid
WHERE subq.name != hostname;
```
Result
```
 uid | owner_id |    name    | hostname  |                 osid                 |                  guid                  | owner_name
-----+----------+------------+-----------+--------------------------------------+----------------------------------------+------------
 543 |      521 | vm-wintodd | VTODD-WIN | 960db7e8-0fc6-42df-b4e0-b0b76ee59550 | {886d407c-1b07-4a89-b31a-8e2ef2894d16} | bobby
(1 row)
```
### Query all cloud drive folders on each tenant and their total files, size, and associated folder groups

```
SELECT folder_id,
	bo1.name AS folder_name,
	fs.total_files,
	pg_size_pretty(folder_size) AS folder_size ,
	fs.owner_id,
	bo4.name AS owner_name,
	fs.portal_id ,
	bo2.name AS portal_name,
	fs.folder_group_id,
	bo3.name AS fg_name,
	bo5.owner_id AS fg_owner_id,
	bo5.name AS fg_owner_name
FROM folders_statistics fs,
    base_objects bo1,
    base_objects bo2,
	base_objects bo3,
	base_objects bo4,
	base_objects bo5
WHERE folder_id=bo1.uid
	AND fs.portal_id=bo2.uid
	AND fs.folder_group_id=bo3.uid
	AND fs.owner_id=bo4.uid
	AND fs.folder_group_id=bo5.uid
	AND bo5.uid=bo5.owner_id
ORDER BY 3 DESC;
```
Result
```
 folder_id |       folder_name        | total_files | folder_size | owner_id |  owner_name  | portal_id | portal_name | folder_group_id |           fg_name
-----------+--------------------------+-------------+-------------+----------+--------------+-----------+-------------+-----------------+-----------------------------
       549 | Documents                |         200 | 30 GB       |      521 | bobby        |         8 | todd        |             548 | bobby-CloudFolders27
       347 | shtuff                   |          77 | 1370 MB     |       72 | tenant-admin |         8 | todd        |              10 | portal-CloudFolders
       564 | nfstest                  |          73 | 6153 kB     |      563 | bobby        |       480 | portal      |             482 | portal-CloudFolders
       533 | convertedFromBackup      |          69 | 4380 MB     |       72 | tenant-admin |         8 | todd        |             523 | tenant-admin-fg22
       534 | convertedAgain           |          69 | 4380 MB     |       72 | tenant-admin |         8 | todd        |             523 | tenant-admin-fg22
       540 | thank you                |          69 | 4380 MB     |       72 | tenant-admin |         8 | todd        |             523 | tenant-admin-fg22
```

#### IDK

```
SELECT folder_id,
       bo1.name AS folder_name,
       fs.total_files,
       pg_size_pretty(folder_size) AS folder_size ,
       fs.owner_id,
       bo4.name AS owner_name,
       fs.portal_id ,
       bo2.name AS portal_name,
       fs.folder_group_id,
       bo3.name AS fg_name,
       bo5.owner_id AS fg_owner_id,
       bo6.name AS fg_owner_name
FROM folders_statistics fs,
     base_objects bo1,
     base_objects bo2,
     base_objects bo3,
     base_objects bo4,
     base_objects bo5,
     base_objects bo6
WHERE folder_id=bo1.uid
  AND fs.portal_id=bo2.uid
  AND fs.folder_group_id=bo3.uid
  AND fs.owner_id=bo4.uid
  AND fs.folder_group_id=bo5.uid
  AND bo6.uid IN (SELECT owner_id FROM base_objects WHERE type='com.ctera.db.objects.FoldersGroup')
ORDER BY 3 DESC LIMIT 5;

 folder_id | folder_name | total_files | folder_size | owner_id |  owner_name  | portal_id | portal_name | folder_group_id |       fg_name        | fg_owner_id | fg_owner_name
-----------+-------------+-------------+-------------+----------+--------------+-----------+-------------+-----------------+----------------------+-------------+---------------
       549 | Documents   |         200 | 30 GB       |      521 | bobby        |         8 | todd        |             548 | bobby-CloudFolders27 |         521 | bobby
       549 | Documents   |         200 | 30 GB       |      521 | bobby        |         8 | todd        |             548 | bobby-CloudFolders27 |         521 | tenant-admin
       347 | shtuff      |          77 | 1370 MB     |       72 | tenant-admin |         8 | todd        |              10 | portal-CloudFolders  |             | tenant-admin
       347 | shtuff      |          77 | 1370 MB     |       72 | tenant-admin |         8 | todd        |              10 | portal-CloudFolders  |             | bobby
       564 | nfstest     |          73 | 6153 kB     |      563 | bobby        |       480 | portal      |             482 | portal-CloudFolders  |             | bobby
(5 rows)
```

### Check progress of new index creations
Requires Postgres 12+
https://www.postgresql.org/docs/12/progress-reporting.html#CREATE-INDEX-PROGRESS-REPORTING
https://dba.stackexchange.com/questions/11329/monitoring-progress-of-index-construction-in-postgresql
```
SELECT 
  now()::TIME(0), 
  a.query, 
  p.phase, 
  p.blocks_total, 
  p.blocks_done, 
  p.tuples_total, 
  p.tuples_done
FROM pg_stat_progress_create_index p 
JOIN pg_stat_activity a ON p.pid = a.pid;
```

### Count all blocks in the given portal's Folders Group.

    SELECT COUNT(*) FROM blocks WHERE group_id IN (SELECT uid FROM base_objects WHERE portal_id=8 AND type='com.ctera.db.objects.FoldersGroup');
    
Example:
```
postgres=# SELECT COUNT(*) FROM blocks WHERE group_id IN (SELECT uid FROM base_objects WHERE portal_id=8 AND type='com.ctera.db.objects.FoldersGroup');
 count
-------
 71208
(1 row)
```

### Query Cloud Folders if Sync ACLs Enabled
```
SELECT subquery.*,
       bo.name AS owner_name,
       bo.portal_id
FROM
  (SELECT uid,
          owner_id,
          name,
          ((xpath('//att[@id="enableSyncWinNtExtendedAttributes"]/val/text()', XMLPARSE(DOCUMENT base_objects.xml_field)))[1]::text::text) AS sync_acl_enabled
   FROM base_objects
   WHERE type='com.ctera.db.objects.CloudDrive' AND is_deleted=FALSE) AS subquery
JOIN base_objects AS bo ON subquery.owner_id = bo.uid
ORDER BY bo.name;
```

Example:
```
 uid | owner_id |           name           | sync_acl_enabled |   owner_name   | portal_id
-----+----------+--------------------------+------------------+----------------+-----------
 534 |      521 | convertedAgain           | true             | bobby          |         8
 551 |      521 | Favorites                |                  | bobby          |         8
 549 |      521 | Documents                |                  | bobby          |         8
 547 |      521 | Desktop                  |                  | bobby          |         8
 578 |      574 | Data                     | true             | ServiceAccount |         8
 589 |      579 | GlobalShare2             | true             | SyncService    |         8
 581 |      579 | GlobalShare1             | true             | SyncService    |         8
 525 |       72 | datacl                   | true             | tenant-admin   |         8
 347 |       72 | shtuff                   | false            | tenant-admin   |         8
 541 |       72 | insertMe                 | true             | tenant-admin   |         8
 540 |       72 | thank you                | true             | tenant-admin   |         8
 524 |       72 | Mizzou Sports Properties | true             | tenant-admin   |         8
 537 |       72 | mizzou-nas               | true             | tenant-admin   |         8
 533 |       72 | convertedFromBackup      | true             | tenant-admin   |         8
 532 |       72 | datdata                  | false            | tenant-admin   |         8
 527 |       72 | newFolder                |                  | tenant-admin   |         8
 588 |      485 | docs                     | false            | todd           |         8
 590 |      485 | Documents                | false            | todd           |         8
 591 |      485 | My Documents             | true             | todd           |         8
(19 rows)
```
