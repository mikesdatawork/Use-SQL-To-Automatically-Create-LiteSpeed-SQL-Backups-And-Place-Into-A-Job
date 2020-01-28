![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Use SQL To Automatically Create LiteSpeed SQL Backups And Place Into A Job
**Post Date: May 5, 2014**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>Here's some really great backup logic if you are using LiteSpeed for your SQL backups. This does a number of things.
0. Configures SQL Server to use Backup Compression for all backups.
1. Automatically creates the folder structure on your Backup Share based on ServerName(or instance name), DatabaseName, etc. 2. Creates an automatic TimeStamp in the backup file name.
3. Adds the ".bkp" backup extension to the file to readily identify between standard '.bak', and '.bkp'.
All you need to do is put this into an SQL Backup Job, and it automatically creates the rest of your backup folder structure under the backup share you specify. Ensure that the Agent Service Account has full rights to your backup share for this to work.</p>      


## SQL-Logic
```SQL
use master;
 
set quoted_identifier off
set nocount on
 
declare @dbserver varchar(255)
declare @servername varchar(255)
declare @fullbackup varchar(255)
declare @logbackup varchar(255)
declare @diffbackup varchar(255)
declare @backup_time_stamp varchar(255)
declare @backup_location varchar(255) declare @create_folder_servername varchar(max) declare @create_folder_backup_type varchar(max) declare @backup_database varchar(max)
 
set @fullbackup = 'Full'
set @logbackup = 'Tran'
set @diffbackup = 'Diff'
set @servername = ( select @@servername )
set @dbserver = ( select left(@@servername, charindex('\', @@servername) +50) )
set @backup_time_stamp = ( select replace(replace(replace(replace(replace(convert(varchar,getdate(), 9),' ',' '),':','-'),' ',' ' ), 'pm', ' pm'), 'am', ' am') ) set @backup_location = '\\MyBackupShare\MyBackupFolder\' set @backup_database = ''
exec master..sp_configure 'show advanced options' , '1' reconfigure
exec master..sp_configure 'xp_cmdshell' , '1' reconfigure
```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)



## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

