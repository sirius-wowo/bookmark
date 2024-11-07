# tidb基础命令

表中插入字段

```sql
alter table live_market_data_all add column live_order_uv bigint comment '直播间交易用户数(下单用户数)';
alter table live_market_data_all add column actual_pay decimal(20,4) comment '实付金额（元）-整场直播';
```

更改表名
```sql
RENAME TABLE app_live_algorithm_caixi_goods_data TO app_live_algorithm_caixi_goods_data_discard;
```

删除字段
```sql
ALTER TABLE tableName DROP COLUMN colName;
```

删除表
```sql
 drop table app_live_algorithm_immersion_goods_data
```




