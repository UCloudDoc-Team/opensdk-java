# 泛化调用

泛化调用是指通过自定义请求与响应对象，来发起调用的用法。

该方法依然可以享有自动计算签名的便利。

如果您希望调用的 API 在 SDK 中尚未被支持，可以使用泛化调用方式来抢先体验。

## 代码示例

### 添加参数对象

```java
package cn.ucloud.example;

import cn.ucloud.common.annotation.UCloudParam;
import cn.ucloud.common.request.Request;

import java.util.List;

public class GetMetricParam extends Request {
    @UCloudParam("Region")
    private String region;

    @UCloudParam("Zone")
    private String zone;

    @UCloudParam("ResourceType")
    private String resourceType;

    @UCloudParam("ResourceId")
    private String resourceId;

    @UCloudParam("MetricName")
    private List<String> metricNameList;

    public GetMetricParam(String region, String zone, String resourceType, String resourceId, List<String> metricNameList) {
        super.setAction("GetMetric");
        this.region = region;
        this.zone = zone;
        this.resourceType = resourceType;
        this.resourceId = resourceId;
        this.metricNameList = metricNameList;
    }
}
```

### 添加响应对象

```java
package cn.ucloud.example;

import cn.ucloud.common.response.Response;
import com.google.gson.annotations.SerializedName;

import java.util.List;
import java.util.Map;

public class GetMetricResult extends Response {
    @SerializedName("DataSets")
    public DataSets dataSets;
    public static class DataSets {
        @SerializedName("CPUUtilization")
        public List<MetricValue> cpuUtilization;
    }
    public static class MetricValue {
        @SerializedName("Timestamp")
        public String timestamp;
        @SerializedName("Value")
        public String value;
    }
}
```

### 调用泛化请求方法

```java
package cn.ucloud.example;

import cn.ucloud.common.client.DefaultClient;
import cn.ucloud.common.config.Config;
import cn.ucloud.common.credential.Credential;
import cn.ucloud.common.request.Request;

import com.google.gson.Gson;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        Config config = new Config();
        String publicKey = "publicKey";
        String privateKey = "privateKey";
        String region = "cn-bj2";
        String zone = "cn-bj2-04";
        String resourceType = "uhost";
        String resourceId = "uhost-xxx";

        Credential credential =
                new Credential(
                        publicKey, privateKey);
        DefaultClient client = new DefaultClient(config, credential);

        List<String> metricNameList = new ArrayList<String>();
        metricNameList.add("CPUUtilization");
        Request request = new GetMetricParam(region, zone, resourceType, resourceId, metricNameList);
        GetMetricResult getMetricResult = null;
        try {
            getMetricResult = (GetMetricResult) client.invoke(request, GetMetricResult.class);
        } catch (Exception e) {
            System.out.println(e.toString());
        }
        System.out.println(new Gson().toJson(getMetricResult));
    }
}
```

