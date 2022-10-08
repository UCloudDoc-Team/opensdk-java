# 快速开始

以获取 udisk 信息为例

## 初始化 Client

第一步、创建 `UDiskClient` 并实例化，默认实例化为 `DefaultUdiskClient`

在 Java SDK 中，`*Client` 都是 SDK 的客户端接口，通过 `*Client` 发起目标操作。

在实例化 `*Client` 时，设置 `Config` 参数和 `Credential`，`Config` 接受一个 `cn.ucloud.common.config.Config` 对象。 `Credential` 接受一个 `cn.ucloud.common.credential.Credential` 对象。

其中 `Credential` 参数用来配置 PrivateKey 和 PublicKey。 `Config` 参数用来配置其他的公共参数。

| 配置            | 类型 | 描述                                                         |
| --------------- | ---- | ------------------------------------------------------------ |
| **Region**      | String  | 服务所在地域，可参考 [地域和可用区列表](https://docs.ucloud.cn/api/summary/regionlist) |
| **ProjectId**   | String  | 项目的唯一标识，用于组织资源，大多数资源都需要 ProjectId，如果是主账号或财务账号，默认值为默认 ProjectId，如果是子账号非财务账号，则该字段必须填写。 |
| **BaseUrl**     | String  | API 服务的访问端点，默认是 https://api.ucloud.cn |
| **UserAgent**   | String  | UserAgent 是 SDK 客户端特有的属性，用于区分使用 SDK 的版本。User-Agent 的定义请参考 [MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent)。用户自定义的 UserAgent 将追加到 SDK 版本号的末尾。例如， “MyAPP/0.10.1” -> “Python/3.7.0 Python-SDK/0.1.0 MyAPP/0.10.1” |
| **Timeout**     | Integer  | 请求超时时间，默认 30s                               |
| **MaxRetries**  | Integer  | 最大重试次数. 默认重试 3 次。设置该值大于 0 将对网络和服务可用性问题进行自动重试，使用指数退避的重试间隔，并自动跳过资源创建类的接口。 |
| **Logger**    | org.slf4j.Logger  | 配置自定义 Logger           |

## 构建参数

第二步、创建 DescribeUDiskRequest 并实例化，作为请求的参数对象。

在 Java SDK 中，`*Request` 对象都是请求的参数类。

请求参数参数需要通过 `set` 方法设置。每个参数类中属性注释 `NotEmpty` 代表参数为必要参数。如果缺少了必要参数，SDK 将抛出 `cn.ucloud.common.exception.ValidatorException`，并且在异常信息中会给出必要参数的属性名称。

## 发起请求

```java
try{
    result = client.method(param);
} catch (Exception e){
    // 这里可能会抛出一些异常
    // 比如网络异常，业务状态码 RetCode 不为 0 等
}
```

完整代码示例：

```java
import cn.ucloud.common.config.Config;
import cn.ucloud.common.credential.Credential;
import cn.ucloud.udisk.client.UDiskClient;
import cn.ucloud.udisk.models.DescribeUDiskRequest;
import cn.ucloud.udisk.models.DescribeUDiskResponse;

public class Main {
    public static void main(String []args)  {
        Credential credential = new Credential("my_public_key","my_private_key");
        Config config = new Config();
        config.setRegion("cn-bj2");
        config.setProjectId("org-xxx");
        UDiskClient client = new UDiskClient(config, credential);
        DescribeUDiskRequest req = new DescribeUDiskRequest();
        req.setOffset(0);
        req.setLimit(10);
        DescribeUDiskResponse resp = null;
        try {
            resp = client.describeUDisk(req);
        } catch (Exception e) {
            e.printStackTrace();
            return;
        }
        System.out.println(resp);
    }
}
```

