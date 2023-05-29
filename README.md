# rpatch

#### 简单介绍
提供1字节patchN方案。
1. 通过已知的N、E或者pem格式的publickey文件，提供一字节patch方案，输出PATCH后的N、E、D、P、Q；并提供两种自定义数据验证方式：1.公钥加密数据，私钥解密数据；2.私钥签名数据，公钥验证签名。
2. 采用golang编写，支持全平台环境。
3. 支持N为素数的这种特殊计算方式。
4. 支持输入数据为16进制或10进制。

#### 说明
**为什么会有此工具**  
传统针对rsa加密的patch方式都是自己生成相同位数的密钥对，然后用自己的公钥进行替换。偶然从前辈大佬那里了解到了还有一种1字节patchN方式，完成后就可以自行加解密运算了，这对于一个逆向爱好者来说真是莫大的吸引。基于自己的兴趣，各位大佬的帮助，现有工具的参考，以及ChatGPT的联动下实现了满足我个人需求的这款小工具。  
**其他说明**：
1. 1字节patch后的N必然不安全
2. 1字节patch和整个数据patch在效果上没有区别。要的就是好玩，所以我只需要1字节patch，不需要2字节、3字节或更多字节的patch......
3. 基于上面这点，我认为安全不应该是此技术考虑的问题；能分解我生成的N再去解密数据的人，自己直接生成替换N也是相同的效果
4. 因为个人能力的问题，工具可能有bug甚至部分实现方式、想法都是错误的；欢迎大家提出来，能力范围内会尽量修复，超出则只能待其他大佬提供更好的工具

#### 使用方法

1.  参数说明：
```
Usage of D:\code\golang\rsapatchtool\rpatch.exe:
  -b int
        生成用于分解的素数合集，默认为3位以内，过大影响生成时间与执行效率 (default 3)
  -check1
        测试输入N、E、D或N、E、P、Q是否匹配,通过公钥加密私钥解密方式验证
  -check2
        测试输入N、E、D或N、E、P、Q是否匹配，通过私钥签名数据公钥确认签名数据方式验证
  -d string
        设置输入的d参数
  -e string
        设置输入的e参数
  -key string
        输入pem格式key文件路径
  -n string
        设置输入的n参数
  -o string
        将生成的pem格式publickey保存到文件
  -p string
        设置输入的p参数
  -q string
        设置输入的q参数
```
2.  通过n、e输出1字节patch方案
```
./rpatch -n 93AF7A8E3A6EB93D1B4D1FB7EC29299D2BC8F3CE5F84BFE88E47DDBDD5550C3CE3D2B16A2E2FBD0FBD919E8038BB05752EC92DD1498CB283AA087A93184F1DD9DD5D5DF7857322DFCD70890F814B58448071BBABB0FC8A7868B62EB29CC2664C8FE61DFBC5DB0EE8BF6ECF0B65250514576C4384582211896E5400000042FDED -e 13
正在生成素数合集
素数合集生成完毕
当前正在使用1位素数合集尝试分解
新N为：
93AF7A8E3A6EB93D1B4D1FB7EC29299D2BC8F3CE5F84BFE88E47DDBDD5550C3CE3D2B16A2E2FBD0FBD919E8038BB05752EC9aDD1498CB283AA087A93184F1DD9DD5D5DF7857322DFCD70890F814B58448071BBABB0FC8A7868B62EB29CC2664C8FE61DFBC5DB0EE8BF6ECF0B65250514576C4384582211896E5400000042FDED
新E为：
13
新P为：
5
新Q为：
1d897ee93ee2f1d90575d324c8d50852a25b63f6131a8cc81c74c5f2c44435a5c72a237ba2d6590325e9ec800b589ab1095b8929db828a1a5534e5509e76392b92ac45fe4de3d3c65c49b50319dbde7419b058bbf032821814f13c8a1f5a1475b66139325ac56961bfe2f6357aa10104117c0d80de6d36b51610ccccccda32c9
新D为：
3e2ef03be2b55b780b7ec9fc9954b334b41eb77f50a3af1e8cbff1934c59b44f8858b67d8cb5c8de34deff5e68ba8908eb47719b7d48b6f411b2ba58e1d0785bbb85a0d4024b6cffeab61ec328f74d961b2269f772f111e1db3f3c154f7a46054a100c9fe784a7f635a7eb631d1d0f9cc67e6d45329514db8cc50d79437a85db
publicKeyPEM：
-----BEGIN PUBLIC KEY-----
MIGdMA0GCSqGSIb3DQEBAQUAA4GLADCBhwKBgQCTr3qOOm65PRtNH7fsKSmdK8jz
zl+Ev+iOR9291VUMPOPSsWouL70PvZGegDi7BXUuya3RSYyyg6oIepMYTx3Z3V1d
94VzIt/NcIkPgUtYRIBxu6uw/Ip4aLYuspzCZkyP5h37xdsO6L9uzwtlJQUUV2xD
hFgiEYluVAAAAEL97QIBEw==
-----END PUBLIC KEY-----
程序执行了 112.7343ms
```
3.  验证自定义的数据是否正确
```
./rpatch -n 93AF7A8E3A6EB93D1B4D1FB7EC29299D2BC8F3CE5F84BFE88E47DDBDD5550C3CE3D2B16A2E2FBD0FBD919E8038BB05752EC9aDD1498CB283AA087A93184F1DD9DD5D5DF7857322DFCD70890F814B58448071BBABB0FC8A7868B62EB29CC2664C8FE61DFBC5DB0EE8BF6ECF0B65250514576C4384582211896E5400000042FDED -e 13 -d 3e2ef03be2b55b780b7ec9fc9954b334b41eb77f50a3af1e8cbff1934c59b44f8858b67d8cb5c8de34deff5e68ba8908eb47719b7d48b6f411b2ba58e1d0785bbb85a0d4024b6cffeab61ec328f74d961b2269f772f111e1db3f3c154f7a46054a100c9fe784a7f635a7eb631d1d0f9cc67e6d45329514db8cc50d79437a85db -check1
正在使用公钥加密测试数据：Hello World
加密结果为：0bcea635db2e8530cabcc77f015a5110d6aad3210fc589fec4fb83e072c003b8f48cd44db1da7e352a5952786af43b3aded453f9e1f53337c0ea08e6404dcb097e2564e62ca7ec65c83e8ef7092862e8c5d1f11b3994a9f8abb91afeed0359e612bf1c0477b565b52694efa36b0e37fe70ff88e8563f466e67f1ad5acdbb4b8f
正在使用私钥解密上面输出的数据...
解密结果为：Hello World
测试加解密成功！
```
```
./rpatch -n 93AF7A8E3A6EB93D1B4D1FB7EC29299D2BC8F3CE5F84BFE88E47DDBDD5550C3CE3D2B16A2E2FBD0FBD919E8038BB05752EC9aDD1498CB283AA087A93184F1DD9DD5D5DF7857322DFCD70890F814B58448071BBABB0FC8A7868B62EB29CC2664C8FE61DFBC5DB0EE8BF6ECF0B65250514576C4384582211896E5400000042FDED -e 13 -d 3e2ef03be2b55b780b7ec9fc9954b334b41eb77f50a3af1e8cbff1934c59b44f8858b67d8cb5c8de34deff5e68ba8908eb47719b7d48b6f411b2ba58e1d0785bbb85a0d4024b6cffeab61ec328f74d961b2269f772f111e1db3f3c154f7a46054a100c9fe784a7f635a7eb631d1d0f9cc67e6d45329514db8cc50d79437a85db -check2
正在使用私钥签名测试数据：Hello World
签名结果为：165492383df85525d8ed8925be635a2f6ac613f403f519604d6fe74bd08c5d6ac24eb17353d341f1e7d8728475f778c52f4fbacf599b951cde68f3c01f7b85342f20fa081de71a258a3c7c54dcf62d6043bdb67f30ce0673cf7c18955024ef45a373256c3e33affba6f84a51a2873551be0abf98f201dfdadd08e25834e845a9
正在使用公钥验证上面的签名数据...
测试签名验证成功！
```
#### 特别感谢

1.  感谢飘云阁的大Z哥，飘云老大，王公子在我学习时提供的帮助
2.  感谢飘云阁的java2job大佬的工具[“RSA模数N修改工具”](https://www.chinapyg.com/thread-146310-1-1)，非常好用的windows平台工具，我也是使用后才对如何自己来写这个工具有一定想法。


