# Spark Streaming

Go through the [Spark Streaming Programming Guide](https://spark.apache.org/docs/latest/streaming-programming-guide.html)
before beginning this section.  In particular, it covers the concept of DStreams.

The earlier examples demonstrates how to compute statistics on an existing log file - but not how to do realtime monitoring of logs. Spark Streaming enables that functionality.

To run the streaming examples, you will `tail` a log file into `netcat` to send to Spark.
This is not the ideal way to get data into Spark in a production system,
but is an easy workaround for a first Spark Streaming example.  We will cover best practices for [how to import data for Spark Streaming in Chapter 2](../chapter2/streaming.md).

In a terminal window, just run this command on a logfile which you will append to:
```
% tail -f [[YOUR_LOG_FILE]] | nc -lk 9999
```

If you don't have a live log file that is being updated on the fly, you
can add lines manually with the included data file or another your own log file:
```
% cat ../../data/apache.access.log >> [[YOUR_LOG_FILE]]
```

When data is streamed into Spark, there are two common use cases covered:

1. [Windowed Calculations](windows.md) means that you only care about data
received in the last N amount of time.  When monitoring your web servers,
perhaps you only care about what has happened in the last hour.
   * Spark Streaming conveniently
splits the input data into the desired time windows for easy processing, using the `window` function of the streaming library.
   * The `forEachRDD` function allows you to access the RDD's created each time interval.
1. [Cumulative Calculations](total.md) means that you want to keep cumulative
statistics, while streaming in new data to refresh those statistics.  In that
case, you need to maintain the state for those statistics.
   * The Spark
Streaming library has some convenient functions for maintaining state
to support this use case, `updateStateByKey`.
   * `tranform` is also introduced to allow code reuse.