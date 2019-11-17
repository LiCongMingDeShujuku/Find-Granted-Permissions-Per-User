![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 查找每个用户的授予权限
#### Find Granted Permissions Per User
**发布-日期:2015年2月27日（评论**

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
这是用于获取每个帐户的权限和每个帐户授予角色的列表的一些快速的SQL逻辑（logic）。

如果你要删除账户 [int \ myaccount]，并且在该帐户向其他数据库对象授予权限遇时到错误，那么需要你去寻找这些权限，但我不建议使用GUI来确定这些权限的位置。

查找每个帐户明确给出的SQL GRANTS。


## English
Here’s some quick SQL logic to get a list of permissions per account and roles that were granted per account.

If you are dropping an account with for example: drop login [int\myaccount], and you run into an error where the account was used to grant permissions to some other database object, then you’ll need to go about finding those permissions, but I would not recommend using the GUI to determine where those grants were done.

To find SQL GRANTS which were explicitly given per account.

---
## Logic
```SQL
use master;
set nocount on
 
select
	'account name' 	= servprin.name
,   'type' 			= servprin.type_desc
,   'permission' 	= servperm.permission_name
,   'state' 		= servperm.state_desc
from
	sys.server_principals servprin
	join sys.server_permissions servperm
	on servprin.principal_id = servperm.grantee_principal_id
where
	servprin.type in ('s', 'u', 'g')
	--and servprin.name like '%whatever%'
order by
	'account name' asc


```

如果你想要将数据库所有者更改为某些内容（例如“sa”），则可以运行以下逻辑（logic）。

If you want to change the database owner to something ( for example ‘sa’ ), you can run the following logic.

```SQL
use master;
set nocount on
declare @change_database_owner      varchar(max)
set 	@change_database_owner      = ''
select  @change_database_owner      = @change_database_owner + 
'exec [' + name + ']..sp_changedbowner ''sa'';' + char(10)
from    sys.databases where name not in ('tempdb') order by name asc
exec    (@change_database_owner)

```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

