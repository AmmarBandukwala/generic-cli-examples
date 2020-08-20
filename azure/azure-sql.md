# Microsoft Azure Cloud Command Line - Snippets

## Change the service-tier of a PAAS Azure SQL Database

```shell
az sql db update -g {resourcegroupname} -s {sqlservername}| -n {sqldatabasename} --edition Premium --service-objective P1 --max-size 500GB
```

## Query Azure SQL Database for accounts and assigned db roles

```sql
select dp1.name as 'RoleName',
   isnull (dp2.name, 'No members') As 'UserName'
   from sys.database_role_members as drm
   right outer join sys.database_principals as dp1
   on drm.role_principal_id = dp1.principal_id
   left outer join sys.database_principals as dp2
   on drm.member_principal_id = dp2.principal_id
where dp1.type = 'R'
order by dp1.name
```
