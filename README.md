# string_agg
 string_agg for SQL Server 2017 lower

1.解决方案名右键属性调整项目配置的目标平台调整，为你的SQL版本

2.连接符为“,”

```c#
public void Accumulate(SqlString value)
{
    if (value.IsNull)
    {
        return;
    }

    this.intermediateResult.Append(value.Value).Append(',');
}

```

3.数据库配置

将生成的.dll文件放到好找的地方

依次执行以下SQL命令

```sql
CREATE ASSEMBLY StringAgg 
FROM 'D:\MSSQL_String_Agg_Function\string_agg.dll'
WITH PERMISSION_SET = SAFE;

exec sp_configure 'clr enabled',1

RECONFIGURE WITH OVERRIDE;

CREATE AGGREGATE string_agg(@input nvarchar(4000))
RETURNS nvarchar(max)
EXTERNAL NAME [StringAgg].[string_agg];
```

4.使用

```sql
select dbo.string_agg(f) from t group by xx
```
