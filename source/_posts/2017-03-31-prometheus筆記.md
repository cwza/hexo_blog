title: prometheus筆記
categories: monitor
tags: [golang, prometheus, monitor]
date: 2017-03-31 14:46:57
---

## Histograms vs Summaries

Histogram與Summaries都適合拿來observe request duration or response size.
使用histogram and summary時, 除了有_count與_sum的matrics以外, 還會有buckets or quantile
可以拿來計算quantiles