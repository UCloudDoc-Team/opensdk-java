# 泛化调用

泛化调用是指通过自定义请求与响应对象，来发起调用的用法。

该方法依然可以享有自动计算签名的便利。

如果您希望调用的 API 在 SDK 中尚未被支持，可以使用泛化调用方式来抢先体验。

## 代码示例

### 添加参数对象

```java
import cn.ucloud.common.annotation.UcloudParam;
import cn.ucloud.common.pojo.BaseRequestParam;

import javax.validation.constraints.NotEmpty;

public class DescribeImageParam extends BaseRequestParam {
    @UcloudParam("Region")
    private String region;

    @UcloudParam("Zone")
    private String zone;

    public DescribeImageParam(
            @NotEmpty(message = "region can not be empty") String region,
            @NotEmpty(message = "zone can not be empty") String zone
    ) {
        super("DescribeImage");
        this.region = region;
        this.zone = zone;
    }
}
```

### 添加响应对象

```java
import cn.ucloud.common.pojo.BaseResponseResult;
import com.google.gson.annotations.SerializedName;

import java.util.List;

public class DescribeImageResult extends BaseResponseResult {
    @SerializedName("ImageSet")
    private List<ImageSet> imageSet;

    public static class ImageSet {
        @SerializedName("ImageId")
        private String imageId;
    }
}
```

### 调用泛化请求方法

```java
import cn.ucloud.common.client.UcloudClient;
import cn.ucloud.common.pojo.Account;
import cn.ucloud.common.pojo.UcloudConfig;
import cn.ucloud.common.client.DefaultUcloudClient;

public class Main {
    public static void main(String[] args) {
        // build client
        UcloudClient client = new DefaultUcloudClient(new UcloudConfig(
                new Account(
                        System.getenv("UCLOUD_PRIVATE_KEY"),
                        System.getenv("UCLOUD_PUBLIC_KEY")
                )
        ));

        // build params
        DescribeImageParam param = new DescribeImageParam("cn-bj2", "cn-bj2-02");
        param.setProjectId(System.getenv("UCLOUD_PROJECT_ID"));

        // invoke action
        DescribeImageResult result = null;
        try {
            result = (DescribeImageResult) client.doAction(param, DescribeImageResult.class);
        } catch (Exception e) {
            e.printStackTrace();
        }

        // show response
        System.out.println(result);
    }
}
```

