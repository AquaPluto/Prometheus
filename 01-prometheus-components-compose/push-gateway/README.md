# Push Gateway



在Prometheus上创建Job。

```yaml
scrape_configs:
- job_name: pushgateway
  honor_labels: false
  static_configs:
  # 注意修改下面的push gateway主机的地址；
  - targets: ['pushgw.magedu.com:9091']
    labels:
      pushgateway_instance: metricfire
```



手动提交指标进行测试。

```bash
cat <<EOF | curl --data-binary @- http://localhost:9091/metrics/job/metricfire/instance/article
# TYPE my_test_metric gauge
my_test_metric 77
# TYPE awesomeness_total counter
# HELP awesomeness_total How awesome is this article.
awesomeness_total 987654321
EOF
```



### Push Gateway

Push Gateway的常用选项。

- *— web.listen-address=:9091*, IP （可选） 和用于侦听请求的端口对;
- *— web.telemetry-path=/metrics*, Pushgateway 的度量（用户发送的和内部的）将在其下公开的 Path;
- *— web.external-url=*, 此 Pushgateway 在外部可用的 URL。如果您通过某个域名公开它，则很有用;
- *— web.route-prefix=*, 如果指定，则将其用作所有路由的前缀。默认为 *— web。external-url* 的前缀;
- *— web.enable-lifecycle*, 如果指定，则允许您通过 [API](https://github.com/prometheus/pushgateway#management-api) 关闭 Pushgateway;
- *— web.enable-admin-api*, 如果指定，则启用 Admin API。它允许您执行某些破坏性操作。以下部分将对此进行更多介绍;
- *— persistence.file=*, 如果指定，则 Pushgateway 每 *— persistence.interval* 时间段将其状态写入此文件;
- *— persistence.interval=5m*, 应将状态写入先前指定的文件的频率;
- *— push.disable-consistency-check*, 如果指定，则不会在摄取时检查量度的正确性。在绝对大多数情况下不应指定;
- *— log.level=info*, *debug*、*info*、*warn*、*error* 之一。仅打印级别高于此级别的消息;
- *— log.format=logfmt*, 可能的值： *logfmt*、*json*。如果您想要可与 [Elasticsearch](https://www.elastic.co/) 一起使用的结构化日志，请指定 *json*。
