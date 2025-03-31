# 一.启动Oracle

1.切换到Oracle用户  
2.执行命令：sqlplus / as sysdba
 
---------------------------------------------------------------------------------
 
# 二.Oracle的DATE时间类型

## 1.总述

在 Oracle 数据库中，时间格式是通过 DATE 和 TIMESTAMP 数据类型来表示的。以下是 DATE 和 TIMESTAMP 数据类型的格式和常见的时间格式模型：  
**1. DATE** **数据类型**  
DATE 类型在 Oracle 中存储日期和时间信息，精确到秒。默认格式通常是 YYYY-MM-DD HH24:MI:SS。

- **默认显示格式：** 当不进行格式化时，Oracle 会以默认的日期格式（可能与会话的 NLS 设置相关）显示日期值。通常的默认格式是 DD-MON-RR 或 DD-MON-YYYY，例如 01-JAN-23。

**2. TIMESTAMP** **数据类型**  
TIMESTAMP 提供了更高的精度，它能精确到小数秒（毫秒或微秒）。默认格式为 YYYY-MM-DD HH24:MI:SS.FF，其中 FF 表示小数秒的部分。  
**3.** **常见时间格式模型**  
Oracle 通过 TO_CHAR、TO_DATE 和 TO_TIMESTAMP 函数来进行格式化或解析日期和时间值。常用的时间格式模型包括：

- **年** **(Year)**
    
    - YYYY：四位数字年份，例如 2023
    - RR：两位数年份，解决千年问题（20世纪和21世纪）
    - YY：两位数字年份，例如 23
- **月** **(Month)**
    
    - MM：两位数字的月份（01-12）
    - MON：三字母缩写的月份，例如 JAN、FEB
    - MONTH：全称的月份，例如 JANUARY
- **日** **(Day)**
    
    - DD：两位数字的日期（01-31）
    - DY：三字母缩写的星期几，例如 MON
    - DAY：星期几全称，例如 MONDAY
- **时间** **(Time)**
    
    - HH24：24小时制的小时（00-23）
    - HH12：12小时制的小时（01-12）
    - MI：分钟（00-59）
    - SS：秒（00-59）
    - FF：小数秒（1到9位）

**4.** **使用示例**  
**示例** **1****：****TO_CHAR** **将** **DATE** **或** **TIMESTAMP** **转换为字符串**  
将当前系统日期转换为指定格式的字符串：  
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS') AS formatted_date FROM dual;  
输出：  
2024-09-18 14:35:00  
**示例** **2****：****TO_DATE** **将字符串转换为** **DATE**  
将字符串 '2024-09-18 14:35:00' 转换为日期类型：  
SELECT TO_DATE('2024-09-18 14:35:00', 'YYYY-MM-DD HH24:MI:SS') AS date_value FROM dual;  
**示例** **3****：****TO_TIMESTAMP** **将字符串转换为** **TIMESTAMP**  
将字符串 '2024-09-18 14:35:00.123456' 转换为时间戳类型：  
SELECT TO_TIMESTAMP('2024-09-18 14:35:00.123456', 'YYYY-MM-DD HH24:MI:SS.FF6') AS timestamp_value FROM dual;  
**5.** **更改默认日期格式**  
可以通过 ALTER SESSION 更改当前会话的默认日期显示格式。例如，设置日期格式为 YYYY-MM-DD：  
ALTER SESSION SET NLS_DATE_FORMAT = 'YYYY-MM-DD';  
这样，SELECT SYSDATE FROM dual; 会显示类似 2024-09-18 的格式。  
**总结**

- Oracle 的 DATE 类型存储日期和时间信息，精确到秒，而 TIMESTAMP 可以精确到毫秒或微秒。
- 使用 TO_CHAR、TO_DATE 和 TO_TIMESTAMP 函数可以格式化或解析日期和时间。
- 常见的时间格式包括 YYYY、MM、DD、HH24、MI、SS 等。
   

## 2.DATE 及其他相关时间类型的详细介绍

**1. DATE** **数据类型**

- **存储内容**：DATE 类型在 Oracle 中存储 **日期和时间** 信息，精确到秒。它包括年、月、日、小时、分钟和秒。
- **默认格式**：默认情况下，Oracle 显示 DATE 类型的格式是 DD-MON-RR 或 DD-MON-YYYY，具体取决于数据库的 NLS_DATE_FORMAT 设置。例如，18-SEP-24 或 18-SEP-2024。
- **范围**：DATE 类型的有效范围为公元前 4712 年到公元 9999 年。
- **存储格式**：Oracle 使用 7 个字节来存储日期和时间信息，其中：
    
    - 1 个字节用于世纪
    - 1 个字节用于年份
    - 1 个字节用于月份
    - 1 个字节用于日期
    - 1 个字节用于小时
    - 1 个字节用于分钟
    - 1 个字节用于秒

**示例：**  
CREATE TABLE employees (￼ emp_id NUMBER,￼ name VARCHAR2(50),￼ hire_date DATE￼);￼  
-- 插入带有日期和时间的记录￼INSERT INTO employees (emp_id, name, hire_date) ￼VALUES (1, 'Alice', TO_DATE('2023-09-18 14:35:00', 'YYYY-MM-DD HH24:MI:SS'));￼  
-- 查询带有日期的记录￼SELECT name, hire_date FROM employees;￼  
**2. TIMESTAMP** **数据类型**  
TIMESTAMP 是 DATE 类型的扩展，提供更高的精度，允许存储到 **小数秒**。

- **存储内容**：包括年、月、日、小时、分钟、秒以及小数秒。
- **精度**：小数秒部分可以精确到 **秒的小数部分（默认** **6** **位精度）**。
- **存储格式**：TIMESTAMP 类型使用 7-11 个字节来存储数据，增加的小数秒部分使它更适合精确时间记录。

**示例：**  
CREATE TABLE logs (￼ log_id NUMBER,￼ event_time TIMESTAMP￼);￼  
-- 插入带有时间戳的记录￼INSERT INTO logs (log_id, event_time) ￼VALUES (1, TO_TIMESTAMP('2023-09-18 14:35:00.123456', 'YYYY-MM-DD HH24:MI:SS.FF6'));￼  
-- 查询带有时间戳的记录￼SELECT log_id, event_time FROM logs;￼  
**3. TIMESTAMP WITH TIME ZONE**  
这是 TIMESTAMP 的变种，允许存储 **时区信息**。它存储日期、时间、小数秒和时区。

- **存储内容**：日期、时间、小数秒和时区偏移量。
- **用途**：用于需要记录不同地理位置的时间，例如跨国交易系统。

**示例：**  
CREATE TABLE global_events (￼ event_id NUMBER,￼ event_time TIMESTAMP WITH TIME ZONE￼);￼  
-- 插入带有时区的记录￼INSERT INTO global_events (event_id, event_time) ￼VALUES (1, TO_TIMESTAMP_TZ('2023-09-18 14:35:00 +08:00', 'YYYY-MM-DD HH24:MI:SS TZH:TZM'));￼  
-- 查询带有时区的记录￼SELECT event_id, event_time FROM global_events;￼  
**4. TIMESTAMP WITH LOCAL TIME ZONE**  
TIMESTAMP WITH LOCAL TIME ZONE 是另一种时间戳类型，它将时区信息标准化为 **数据库时区**。

- **存储内容**：它存储日期和时间，并自动将时区转换为数据库时区。
- **用途**：适用于应用程序需要跨时区但需要将时间标准化存储的情况。

**示例：**  
CREATE TABLE transactions (￼ txn_id NUMBER,￼ txn_time TIMESTAMP WITH LOCAL TIME ZONE￼);￼  
-- 插入不同时区的时间，但存储为数据库时区￼INSERT INTO transactions (txn_id, txn_time) ￼VALUES (1, TO_TIMESTAMP_TZ('2023-09-18 14:35:00 +02:00', 'YYYY-MM-DD HH24:MI:SS TZH:TZM'));￼  
-- 查询时会根据当前用户会话的时区来显示￼SELECT txn_id, txn_time FROM transactions;￼  
**5. INTERVAL** **数据类型**  
INTERVAL 数据类型用于存储两个时间点之间的 **时间间隔**。它有两种类型：

- **INTERVAL YEAR TO MONTH**：表示以年和月为单位的时间间隔。
- **INTERVAL DAY TO SECOND**：表示以天、小时、分钟和秒为单位的时间间隔。

**示例：**  
-- YEAR TO MONTH￼SELECT INTERVAL '2-6' YEAR TO MONTH AS interval_year_month FROM dual;￼  
-- DAY TO SECOND￼SELECT INTERVAL '5 12:30:45' DAY TO SECOND AS interval_day_second FROM dual;￼  
**6.** **时间操作**  
可以对 DATE 和 TIMESTAMP 数据类型进行加减操作。例如：  
**日期加减**  
-- 当前日期加 7 天￼SELECT SYSDATE + 7 FROM dual;￼  
-- 两个日期相减，得到天数差异￼SELECT TO_DATE('2023-09-18', 'YYYY-MM-DD') - TO_DATE('2023-09-10', 'YYYY-MM-DD') FROM dual;￼  
**总结**  
Oracle 提供了丰富的时间类型和操作，满足不同的时间存储需求：

- DATE：用于存储日期和时间，精确到秒。
- TIMESTAMP：提供更高精度的时间存储，支持小数秒。
- TIMESTAMP WITH TIME ZONE 和 TIMESTAMP WITH LOCAL TIME ZONE：用于处理带时区的信息。
- INTERVAL：用于表示时间间隔。

通过这些类型，开发者可以根据应用需求处理不同的时间和日期信息。
 
---------------------------------------------------------------------------------