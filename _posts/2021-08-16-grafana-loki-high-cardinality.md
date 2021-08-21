---
title: "Using Loki to Graph High Cardinality Metrics"
date: "2021-08-16T06:34:30-07:00"
categories:
  - blog
tags:
  - observability
---

One value-add that Loki provides is the ability to alert based on high cardinality labels, something that one simply cannot do with Prometheus.

At my company, most of our day-to-day computing is on a specific game, often with additional context like the specific frame of video. Naturally, this gameID and frame index cannot be used as labels because of its nature: very high cardinality.

Our teams want to create alerts off this data - with the necessary context - however, cannot do it with Prometheus.

Furthermore, we would like to be able to visualize these metrics. However, instead of using the traditional timestamp as the x-axis, it's helpful to view a game over the course of each video frame, not necessarily the time the frame was processed. This aligns metrics from different services that may or may not be running at the same time.

With Grafana, this was relatively easy. We did need a little bit of custom configuration, hence the blog post. Sharing this setup with the world, as nothing existing when I initially started to think about this problem.

The solution. We're able to view these metrics in a table, as well as on a graph with a custom x-axis.

- [scatter plot plugin](https://grafana.com/grafana/plugins/michaeldmoore-scatter-panel/)
- [loki datasource](https://grafana.com/docs/grafana/latest/datasources/loki/) + logs in [logfmt](https://brandur.org/logfmt) + query + [labels to fields](https://grafana.com/docs/grafana/latest/panels/transformations/types-options/#labels-to-fields) transform

Example code

```jsx
console.log(
  "foo=bar gameID=3349AE88-C2E4-4FCB-9967-901484A3DD2C frame=100 v1=0.2 v2=0.7"
);
```

Query

```bash
{app="appname"} |= "foo=bar" | logfmt
```

Screenshots

![/assets/images/cardinality.png](/assets/images/cardinality.png)

Contact

If you want more info, reach out on twitter or message me on the grafana labs slack.