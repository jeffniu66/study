# 1. 常用sql

## 1.1 导数据

### 1.1.1 新手流失数据

```sql
SELECT t1.version, guide_step FROM role t1, role_info t2 WHERE t1.role_id = t2.role_id and t1.version = 'huawei';
SELECT t1.version, guide_step FROM role t1, role_info t2 WHERE t1.role_id = t2.role_id and t1.version = 'vivounion';
SELECT * FROM `role_step`;
```



### 

