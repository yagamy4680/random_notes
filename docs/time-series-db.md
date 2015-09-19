## Time series database 

[Definitions from wikipedia](http://en.wikipedia.org/wiki/Time_series_database):

- [InfluxDB](http://influxdb.com/)
- [SkyDB](http://skydb.io/guide/)
- [SciDB](http://www.scidb.org/)
- [Druid](http://druid.io/druid.html)
- [Geras](http://1248.io/geras.php)
- [OpenTSDB](http://opentsdb.net/)
- [KairosDB](https://code.google.com/p/kairosdb/)


## Time-series database and/or Monitoring system

### [Prometheus](http://prometheus.io/)

An open-source service monitoring system and time series database.

Alerting rules are defined in the following syntax:

```text
ALERT <alert name>
  IF <expression>
  [FOR <duration>]
  [WITH <label set>]
  SUMMARY "<summary template>"
  DESCRIPTION "<description template>"
```




#### Articles

- [Time-Series Databases and InfluxDB](http://www.xaprb.com/blog/2014/03/02/time-series-databases-influxdb/)
  - native time-series database
  - native http api
  - distributed
  - on-top-of LevelDB, may switch to other storage in the future
  - developed in GO lang
