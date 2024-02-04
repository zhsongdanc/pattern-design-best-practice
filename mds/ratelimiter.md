### 背景
促销活动导致其他app超时，需要支持接口级别
### 用户用例
```java
String appId = "app-1"; // 调用方APP-ID
String url = "http://www.eudemon.com/v1/user/12345";// 请求url
RateLimiter ratelimiter = new RateLimiter();
boolean passed = ratelimiter.limit(appId, url);
if (passed) {
// 放行接口请求，继续后续的处理。
}else{
}
```
### 要求
1. 支持各种算法（sliding window, leak bucket, token bucket、基于内存的单机限流算法、基于 Redis 的分布式限流算法，）
2. 易用性：能非常方便地集成到Spring
3. 扩展性、灵活性（配置方式：json,yaml,xml,不同数据源：本地、zk）
4. 性能：低延迟，尽可能减少对接口请求本身响应时间的影响。
5. 容错性：不能因为限流框架的异常， 反过来影响到服务本身的可用性比如，分布式限流算法
依赖集中存储器 Redis。如果 Redis 挂掉了，限流逻辑无法正常运行，这个时候业务接口
也要能正常服务才行。


### 配置举例
```yaml
configs:
- appId: app-1
  limits:
  - api: /v1/user
    limit: 100
  - api: /v1/order
  - limit: 50
- appId: app-2
  limits:
  - api: /v1/user
    limit: 50
  - api: /v1/order
    limit: 50
```
