---
aliases: [SQL function]
---

# Metadata
Status :: #🌱 
Note_Type :: #📥/🏫 
Topics :: [[SQLZOO]], [[SQL Server.MOC|SQL.MOC]]
in_one_line :: 

# [[常用SQL函式]]

- `Concat`: 連接字串
	- syntax: `Concat(字串1, 字串2, 字串3, ...)`
	- s: [SQL Concatenate 函数](https://www.1keydata.com/tw/sql/sql-concatenate.html)
	- example: [[#Concat 範例]]

- `Replace`: 取代特定欄位中的指定字串
	- syntax: `Replace (str1, str2, str3)`
	- 在字串 str1 中，當 str2 出現時，將其以 str3 替代。
	- s: [SQL Replace 函數](https://www.1keydata.com/tw/sql/sql-replace.html)
	- example: [[#Replace 範例]]



## 範例
### Concat 範例
- 範例1: “Mexico 墨西哥”的首都是”Mexico City”。  
顯示所有國家名字，其首都是國家名字加上”City”
```SQL
SELECT name
FROM world
WHERE Capital = concat(name, ' City')
```

- 範例2: 找出所有首都和其國家名字,而首都要有國家名字中出現。
```SQL
SELECT capital, name
FROM world
WHERE Capital like concat('%', name, '%')
```

### Replace 範例
- 範例1: "Monaco-Ville"是合併國家名字 "Monaco" 和延伸詞"-Ville".  
顯示國家名字，及其延伸詞，如首都是國家名字的延伸。
```SQL
SELECT name, REPLACE(capital, name, '') AS ext
FROM world 
where capital LIKE CONCAT(name, '_%');
```

### idk
```SQL

```





