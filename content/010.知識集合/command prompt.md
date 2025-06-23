---
aliases: []
---

# Metadata
Status :: #🌱 
Note_Type :: #📝 
Topics :: 
in_one_line :: 

# 近期使用頁面

```dataview
table file.mday as "modified",topics
from [[command prompt]]

sort file.ctime
limit 10
```


# 常用指令

- 複製檔案並強制覆蓋
```sh
Copy-Item $source $Destination -Recurse -force
```
