# 数据处理

---
### named_struct

在 SparkSQL 中，named_struct 函数返回的是一个结构体（struct），而不是数组或 JSON。结构体是一种具有命名字段的数据类型，可以包含多个不同类型的值。使用 named_struct 时，实际上是在创建一个新的列，该列的类型是结构体。这个结构体列中的每个元素都是一个结构体实例，它包含了在 named_struct 函数中指定的字段和对应的值。示例如下:


``` sql
-- 使用下面的sql得到的my_struct字段的值，实际展示时看上去是个类似json的结构。 应该是 ，再确认一下
-- 结构体以类似于 JSON 对象的格式表示，这是因为结构体和 JSON 对象在概念上是相似的，都是包含键值对的数据结构。
SELECT named_struct('stuNo', 1, 'stuName', 'foo') AS my_struct
```
- 如果想要将结构体转换为 JSON 字符串，使用 SparkSQL 的 to_json 函数
    ```sql
    SELECT to_json(named_struct('stuNo', 1, 'stuName', 'foo')) AS my_struct
    ```
- 如果要获取struct结构体字段中的值，可以通过 `.字段名`的方式

    ```sql
    SELECT 
    	my_struct.stuNo  	as student_no
    FROM 
    (
    	SELECT named_struct('stuNo', 1, 'stuName', 'foo') AS my_struct
    )
    ```

- 如果要存储在hive表中，可以将字段的格式设定为string或者struct，但两种方式存储的数据形式是不一样的，差别如下:
    -   a. 如果设定字段为string， 存储的应该是个数组
        ```sql
        CREATE TABLE my_table (
            struct_data  string
        )
        STORED AS ORC;
        ```
    -   b. 如果设定的是struct，存储的应该是个？？

        ```sql
        
        -- 如果存成struct，获取的 ？？？？
        CREATE TABLE my_table (
            struct_data STRUCT<stuNo:bigint, stuName:STRING>
        )
        STORED AS ORC;
        ```
        
- 如果想存储一个struct结构体的数组，那么可以通过`groupby` 和 `collect_set`来拼凑。并且，存储格式也可以两种格式:
    
    - 存储为结构体数组
        ```sql
        CREATE TABLE IF NOT EXISTS `${target.table}`
        (
            `arraybystruct`             ARRAY<STRUCT<stuNo:bigint, stuName:STRING>>     
        )
        ```
        
    - 存储为字符串数组
        - 如果存储为字符串 
     
