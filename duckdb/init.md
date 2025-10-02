duckdb是一个纯内存型的列式引擎。

连接（test.db库）：
```
./duckdb test.db
```
创建表：
```
CREATE TABLE test_numeric_types (
    id INTEGER NOT NULL PRIMARY KEY,
    float_col FLOAT,
    double_col DOUBLE,
    value_name VARCHAR,
    created_at TIMESTAMP DEFAULT current_timestamp
);
```

写入数据：
```
 INSERT INTO test_numeric_types (id, float_col, double_col, value_name, created_at)
  SELECT
      i AS id,
      CAST(random() * 1000 AS FLOAT) AS float_col,         -- 0~1000 随机 float
      random() * 1000000 AS double_col,                   -- 0~1,000,000 随机 double
      'name_' || CAST(i AS VARCHAR) AS value_name,        -- 拼接字符串
      now() AS created_at                   -- 当前时间
  FROM range(10000) AS t(i);  
```

`checkpoint`触发写磁盘，其中包含列压缩过程。
```
checkpoint;
```
清除数据：
```
truncate test_numeric_types;
```
