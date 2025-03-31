**1.FTS****工具说明**  
**配置：**  
FTS工具支持并发测试，需要配置自己的配置文件和环境变量，涉及修改两个文件：profile.xxx与conf/ftsenv.xxx  
以profile.wqy和conf/ftsenv.wqy为例，修改内容如下（名字改成自己的）：  
1.profile.wqy  
    FTSCONFIG=ftsenv.wqy  
    FTSUSER=wangqianyu  
2.conf/ftsenv.wqy  
    dbInstall：  
        installPath：/opt/FTS_test（FTS自动安装数据库的位置，写自己想要的安装目录）  
        instancePort：58051（端口号要和其他人不同，看一下别人的避免重复）  
    ......  
    USERASSIGNMENT:  
        wangqianyu：1（需与profile.wqy的FTSUSER=wangqianyu一致）
 
**用法：**  
1.cd /home/FTS  
2.source profile.xxx  
3.ftsrun -v  
（显示FTS Version 1.4.1，说明工具可以使用）  
4.ftsrun -si  
（FTS工具将自动安装/home/FTS/install/GBase8sV8.8_3.5.2_1204.tar到你自己配置的安装目录中，此过程是自动的，无需其他操作）  
5.ftsrun /home/FTS/testcase/3_sysFunction/c_121current_timestamp/  
（找到自己希望跑的用例目录，例如：/home/FTS/testcase/3_sysFunction/c_121current_timestamp）
 
**说明：**  
1.用例中的.act文件为实际跑出的文件，.exp文件为预期文件，两者对比失败则FAIL，对比规则见手册。  
2.用例跑完有三种状态，PASS,FAIL,EXCEPTION，报EXCEPTION说明没有.exp文件。  
3.跑完后会有一个log文件，/home/FTS/summary_xxxxxxxxxx.log。  
4.详见手册：\\172.16.3.128\water\chenziyang\FTS配置与使用手册.docx。
    
**2.FTS****用例测试分析**  
测试中遇到与预期不匹配的，在oracle和O兼容版本单独跑一下测试用例，看结果：  
1、功能验证结果与oracle结果一致的，和测试协商是否可以修改预期。  
2、功能验证结果与O兼容版本一致的，与oracle不一致的，可能存在功能需要完善，记录问题，及时反馈。  
3、功能验证结果与O兼容版本不一致的，是当前验证的版本可能存在未合并的bug修复的提交，需找一下bug修复对应的提交，记录下来。
    
**3.git****托管**  
现在testcase加入git托管。
 
库:http://172.16.2.108/root/gbase8s_rd.git
 
分支:VR_FTS_TESTCASE
 
这个库是NQA的库，所以拉下来此分支会有点慢，约五分钟，命令如下：  
git clone --single-branch -b VR_FTS_TESTCASE [http://172.16.2.108/root/gbase8s_rd.git](http://172.16.2.108/root/gbase8s_rd.git)
 
将本次功能验证自己的修改提交到git上，以后咱们的用例修改都用git。
 
请大家将修改过的用例和补充的用例及预期文件，同步到git上，以便回归用@所有人