---
layout: post
title: MySQL:URLEncode函数、URLDecode函数、MultiURLDecode 函数 
date: 2015-07-11 19:37
thumbnail:
tags: MySQL UrlEncode
categories: MySQL
published: false

---
## Overview

最近的项目数据库中存的全是`URLEncode()`后的数据，导致直接在数据库中执行脚本啥结果都跑不出来……后来在网上找到了MySQL版本的URLEncode函数和URLDecode函数：

#### URLEncode

``` mysql
DELIMITER ;
 
DROP FUNCTION IF EXISTS urlencode;
 
DELIMITER |
 
CREATE FUNCTION urlencode (s VARCHAR(4096)) RETURNS VARCHAR(4096)
DETERMINISTIC 
CONTAINS SQL 
BEGIN
       DECLARE c VARCHAR(4096) DEFAULT '';
       DECLARE pointer INT DEFAULT 1;
       DECLARE s2 VARCHAR(4096) DEFAULT '';
 
       IF ISNULL(s) THEN
           RETURN NULL;
       ELSE
       SET s2 = '';
       WHILE pointer <= length(s) DO
          SET c = MID(s,pointer,1);
          IF c = ' ' THEN
             SET c = '+';
          ELSEIF NOT (ASCII(c) BETWEEN 48 AND 57 OR
                ASCII(c) BETWEEN 65 AND 90 OR
                ASCII(c) BETWEEN 97 AND 122) THEN
             SET c = concat("%",LPAD(CONV(ASCII(c),10,16),2,0));
          END IF;
          SET s2 = CONCAT(s2,c);
          SET pointer = pointer + 1;
       END while;
       END IF;
       RETURN s2;
END;
|
DELIMITER ;
```

#### URLDecode 

``` mysql
DROP FUNCTION IF EXISTS urldecode;
 
DELIMITER |
 
CREATE FUNCTION urldecode (s VARCHAR(4096)) RETURNS VARCHAR(4096)
DETERMINISTIC 
CONTAINS SQL 
BEGIN
       DECLARE c VARCHAR(4096) DEFAULT '';
       DECLARE pointer INT DEFAULT 1;
       DECLARE h CHAR(2);
       DECLARE h1 CHAR(1);
       DECLARE h2 CHAR(1);
       DECLARE s2 VARCHAR(4096) DEFAULT '';
 
       IF ISNULL(s) THEN
          RETURN NULL;
       ELSE
       SET s2 = '';
       WHILE pointer <= LENGTH(s) DO
          SET c = MID(s,pointer,1);
          IF c = '+' THEN
             SET c = ' ';
          ELSEIF c = '%' AND pointer + 2 <= LENGTH(s) THEN
             SET h1 = LOWER(MID(s,pointer+1,1));
             SET h2 = LOWER(MID(s,pointer+2,1));
             IF (h1 BETWEEN '0' AND '9' OR h1 BETWEEN 'a' AND 'f')
                 AND
                 (h2 BETWEEN '0' AND '9' OR h2 BETWEEN 'a' AND 'f') 
                 THEN
                   SET h = CONCAT(h1,h2);
                   SET pointer = pointer + 2;
                   SET c = CHAR(CONV(h,16,10));
              END IF;
          END IF;
          SET s2 = CONCAT(s2,c);
          SET pointer = pointer + 1;
       END while;
       END IF;
       RETURN s2;
END;
  
|
 
DELIMITER ;
```

#### MultiURLDecode

有时候数据会被进行多次 Encode ，这个时候就需要响应进行多次 Decode 操作，直到没有需要进行解码的字符。

``` mysql
DELIMITER ;
 
DROP FUNCTION IF EXISTS multiurldecode;
 
DELIMITER |
 
CREATE FUNCTION multiurldecode (s VARCHAR(4096)) RETURNS VARCHAR(4096)
DETERMINISTIC 
CONTAINS SQL 
BEGIN
       DECLARE pr VARCHAR(4096) DEFAULT '';
       IF ISNULL(s) THEN
          RETURN NULL;
       END IF;       
       REPEAT
          SET pr = s;
          SELECT urldecode(s) INTO s;
       UNTIL pr = s END REPEAT;
       RETURN s;
END;
  
|
 
DELIMITER ;
```



## 小结

`URLEncode`会将一个字符或者汉字变成3个2位十六进制数字：%AB%CD%EF。

所以`URLDecode`的原理大概就是读取三个十六进制数字拼成ABCDEF，然后通过`CONV()`函数转化为十进制，再将通过`CHAR()`这个十进制数转为字符。



#### Reference

[周丕中的生活技术博客](http://zpz.name/2135/)