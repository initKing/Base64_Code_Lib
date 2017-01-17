#### iOS中常使用RSA进行加密解密, 此文件适用于移动端和java后台进行数据传输的时候, 进行数据加密解密的工具类
#### 使用的时候, 直接将此类文件拖入到项目中即可!

### 如何使用:

* 生成加密类实例
    
    RSAEncryptor *rsa = [[RSAEncryptor alloc] init];

* 获取公钥路径 (注意拖拽公钥的时候, 记得将'Add to targets'选项打钩, 否则通过[Bundle mainBundle] 查询不到公钥的路径)
    
    NSString *publicKeyPath = [[NSBundle mainBundle] pathForResource:@"public_key" ofType:@"der"];

* 加载公钥

    [rsa loadPublicKeyFromFile:publicKeyPath];

    NSString *parm1 = @"need to encript string";

* 对参数加密

    NSString *encParam1 = [rsa rsaEncryptString:parm1];```
    

### 关于待机密数据过长问题, 分段加密 分段解析实施思路:

* 1 然后将加密后的密文 传递给后台

* 2 如果需要加密的数据长度过长(超过128字节), 那么会导致数据加密不完全, 无法完全解析

* 3 解决办法就是：对待加密的数据进行 '分段加密'


### like this：

    NSString *param = @"this is a long string, or other kind of objects, in the final analysis this is a long long string or object neet to encript, more than 128 byte"; 

> 分割字符串：

    NSString *segment1 = @"this is a long string, or other kind of objects,";

    NSString *segment2 = @" in the final analysis this is a long long string or object neet to encript, more than 128 byte";

> 然后分别加密：

    NSString *encString1 = [rsa rsaEncryptString:segment1];

    NSString *encString2 = [rsa rsaEncryptString:segment2];

> 然后将加密后的密文拼接, 中间可以加个空格, 方便后台根据空格分割密文, 进行分段解析

    NSString *result = [NSString stringWithFormat:@"%@ %@",encString1, encString2];


    
