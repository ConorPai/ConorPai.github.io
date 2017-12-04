---
title: C#实现带证书http请求
date: 2017-12-04 16:01:19
tags: C#
---

使用X509Certificates类调用证书
1.添加命名空间
```cs
using System.Security.Cryptography.X509Certificates;
```

2.添加验证回调
```cs
private static bool CheckValidationResult(object sender, X509Certificate certificate, X509Chain chain, SslPolicyErrors errors)
{
	if (errors == SslPolicyErrors.None)
		return true;
	return false;
}
```

3.实现带请书http请求
```cs
//配置证书回调方法
ServicePointManager.ServerCertificateValidationCallback = new RemoteCertificateValidationCallback(CheckValidationResult);

//配置请求
HttpWebRequest request = (HttpWebRequest)WebRequest.Create(sUrl);
request.Method = "POST";
X509Certificate objx509 = new X509Certificate(Application.StartupPath + "\\ForestarAuth.cer");	//可以在第二个参数中传入证书密码
request.ClientCertificates.Add(objx509);

//获取请求结果
HttpWebResponse res = (HttpWebResponse)request.GetResponse();
if (res.StatusCode != HttpStatusCode.OK)
{
    response = "连接错误，状态码：" + res.StatusCode.ToString();
    return false;
}

//读取请求结果
Stream resStream = res.GetResponseStream();
StreamReader sReader = new StreamReader(resStream, Encoding.GetEncoding("utf-8"));
char[] buffer = new char[256];
int count = sReader.Read(buffer, 0, 256);
StringBuilder sb = new StringBuilder();
while (count > 0)
{
    String sTemp = new String(buffer, 0, count);
    sb.Append(sTemp);
    count = sReader.Read(buffer, 0, 256);
}
response = sb.ToString();
```