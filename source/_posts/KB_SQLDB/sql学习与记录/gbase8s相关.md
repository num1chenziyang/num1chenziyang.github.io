# 一.gbase8s的时间类型

1.date类型
 
j格式是从0001.1.1开始
    
select sysdate::date::int from dual;  
查看今天date的数字是多少
 
select 1::date from dual;  
可以看出date是从1900-01-01开始的