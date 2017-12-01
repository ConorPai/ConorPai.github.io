---
title: 使用C#获取系统软硬件信息
date: 2017-12-01 17:01:34
tags: C#
---

最近需要使用C#获取操作系统软硬件信息，在网上找到如下解决方案：

```cs
/// <summary>
/// 操作系统信息
/// </summary>
public string OSInfo
{
    get
    {
        return Environment.OSVersion.ToString();
    }
}
```

```cs

/// <summary>
/// BIOS序列号
/// </summary>
public string BIOSNumber
{
    get
    {
        try
        {
            var query = new ManagementObjectSearcher("SELECT * FROM Win32_BIOS WHERE (SerialNumber IS NOT NULL)").Get();

            string result = string.Empty;
            foreach (var obj in query)
            {
                result = obj["SerialNumber"].ToString();
                break;
            }

            return result;
        }
        catch
        {
            return "";
        }
    }
}
```

```cs
/// <summary>
/// 处理器序列号
/// </summary>
public string CPUNumber
{
    get
    {
        try
        {
            var query = new ManagementObjectSearcher("SELECT * FROM Win32_Processor WHERE (ProcessorId IS NOT NULL)").Get();

            string result = string.Empty;
            foreach (var obj in query)
            {
                result = obj["Processorid"].ToString();
                break;
            }

            return result;
        }
        catch
        {
            return "";
        }
    }
}
```

```cs
/// <summary>
/// 内存信息
/// </summary>
public string MemoryNumber
{
    get
    {
        try
        {
            var query = new ManagementObjectSearcher("SELECT * FROM Win32_PhysicalMemory").Get();

            string result = string.Empty;
            foreach (var obj in query)
            {
                result = obj["PartNumber"].ToString();
                break;
            }

            return result;
        }
        catch
        {
            return "";
        }
    }
}
```

```cs
/// <summary>
/// 主板序列号
/// </summary>
public string BaseBoardNumber
{
    get
    {
        try
        {
            var query = new ManagementObjectSearcher("SELECT * FROM Win32_BaseBoard WHERE (SerialNumber IS NOT NULL)").Get();

            string result = string.Empty;
            foreach (var obj in query)
            {
                result = obj["SerialNumber"].ToString();
                break;
            }

            return result;
        }
        catch
        {
            return "";
        }
    }
}
```

```cs
/// <summary>
/// 硬盘序列号
/// </summary>
public string HardDiskNumber
{
    get
    {
        try
        {
            var query = new ManagementObjectSearcher("SELECT * FROM Win32_DiskDrive WHERE (SerialNumber IS NOT NULL) AND (MediaType LIKE 'Fixed hard disk%')").Get();

            string result = string.Empty;
            foreach (var obj in query)
            {
                result = obj["SerialNumber"].ToString();
                break;
            }

            return result;
        }
        catch
        {
            return "";
        }
    }
}
```

```cs
/// <summary>
/// 网卡信息
/// </summary>
public string NetWorkInfo
{
    get
    {
        try
        {
            var query = new ManagementObjectSearcher("SELECT * FROM Win32_NetworkAdapter WHERE (MACAddress IS NOT NULL) AND (NOT (PNPDeviceID LIKE 'ROOT%'))").Get();

            string result = string.Empty;
            foreach (var obj in query)
            {
                result = obj["MACAddress"].ToString();
                break;
            }

            return result;
        }
        catch
        {
            return "";
        }
    }
}
```

还可以获取其它信息，由于工作中没用到就不在这里列出了，具体可以参考源博：[http://www.cnblogs.com/chen110xi/p/6189499.html](http://www.cnblogs.com/chen110xi/p/6189499.html)