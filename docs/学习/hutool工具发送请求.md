---
title: hutool工具发送请求
createTime: 2024/09/24 16:25:53
permalink: /article/18633rxz/
---
# 使用hutool工具发送request 请求模板

```
private JSONArray fetchData(){
 		String url = "https://www.baidu.com";
	   	String token = "asdfasdfsdf";
        JSONObject requestBody = new JSONObject();
        JSONObject filterOption = new JSONObject();
        JSONObject filters = new JSONObject();
        filterOption.set("filters", filters);
        requestBody.set("filterOption", filterOption);
        requestBody.set("a", 1);
        requestBody.set("b", 1000);
        requestBody.set("c", "list");
        log.info(requestBody.toString());
        try (HttpResponse response = HttpRequest.post(url)
                .header("Content-Type", "application/json")
                .header("token", token)
                .body(requestBody.toString())
                .timeout(20000)  // 设置超时时间
                .execute()) {

            // 处理响应
            if (response.getStatus() == 200) {
                String responseBody = response.body();
                logger.info(responseBody);
                return new JSONObject(responseBody).getJSONArray("results");
            } else {
                logger.error("错误获取数据: " + response.getStatus() + " - " + response.body());
                throw new RuntimeException("错误获取数据");
            }

        } catch (Exception e) {
            logger.error("数据错误", e);
            throw e; // 或者根据需要处理异常
        }
    }
```