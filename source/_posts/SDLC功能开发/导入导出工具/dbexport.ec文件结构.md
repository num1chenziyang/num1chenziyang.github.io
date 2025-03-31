main
 
1.前置准备  
涉及资源设置等。  
value_fflush存储文件的fflush的值
 
procopts  
process the command line  
处理命令行，用于处理工具的模式，此处我想设置一个-p以专门导出创建公共结构的语句。  
给予各种flag特征值与预处理
 
check_dbsecadm（username，dbname）  
{prt_err_out，通用的错误输出函数}
 
dbopen，dbclose，验证用
    
2.操作  
dbopen  
/*only dba and gbasedbt can do an export*/
 
bldschema  
拼接sql语句  
…….
 
value_fflush=fflush(sqlfile);  
用于刷新文件流，sqlfile即文件符
    
3.释放资源
            

gdb配合watch -n 2 ls命令可以定位到哪个语句生成了新文件
 
200：varinits()，初始化全局变量  
此时sqlfile和msgfile均初始化为FILE*类型
 
203：procopts(argc, argv)  
{  
750：msgfile=fopen("dbexport.out","w")，生成dbexport.out文件  
此时msgfile为dbexport.out文件  
1014：sprintf(publicorafname, "%s_public_ora.sql", basename); 创建czy_public_ora.sql文件  
}
    
291：dbxinit()  
{  
714：diskinit()  
{  
diskmod.c：73：mkrmchdir，生成czy.exp目录  
diskmod.c：79：fopen，生成czy.sql文件  
}  
}
   

306：bldschema()，生成czy_ora.sql文件，并向其中写入内容
   

314：fflush(sqlfile);，向czy.sql文件写入内容
   

320：fflush(msgfile);，向dbexport.out文件写入内容