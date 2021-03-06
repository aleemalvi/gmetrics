# GMetrics - Creating a MetricSet

## Creating a Groovy MetricSet File

  **GMetrics** provides a Groovy DSL (domain-specific language) for defining *MetricSets*.

## A Sample Groovy MetricSet

  The preferred syntax for defining a *MetricSet* is to specify the list of *Metric*s using the *Metric* names. Here is simple example:

```
metricset {

    description 'A simple MetricSet'

    ClassCount          // specify metric name only (note no "Metric" suffix on metric name)

    FieldCount {        // configure the Metric using a Closure
        functions = ['total']
    }

    MethodCount([functions:['total'])    // configure the Metric using a Map of properties
}
```

  Things to note:

  * The Groovy *MetricSet* file itself must be accessible on the classpath.

  * This *MetricSet* includes the **ClassCount**, **FieldCount** and **MethodCount** *Metric*s.

  * Note that the *Metric* names do not include the "Metric" suffix (of the *Metric* implementation class name).

  * *Metric* properties can be configured either within a Closure (as assignments) or through a *Map* passed in within the parentheses (after the *Metric* name).

  * You can specify the *Metric* names for all *Metric*s provided with **GMetrics**. If you provide your own custom *Metric*s, then you must use the `metric(Class)` syntax instead -- see below.


## A More Comprehensive Groovy MetricSet

  Here is an example of a more comprehensive Groovy *MetricSet* file, illustrating specifying *Metric*s by *Metric* class or *Metric* name, and also nested *MetricSet* files:

```
import org.gmetrics.metric.cyclomatic.CyclomaticComplexityMetric

metricset {

    description 'An example MetricSet'

    metricset("MyMetricSet.groovy")

    metric(org.gmetrics.metric.abc.AbcMetric) {
        functions = ['average', 'maximum']
    }

    metric(CyclomaticComplexityMetric)      // specify metric class (see import)

    ClassCount          // specify metric name only (note no "Metric" suffix on metric name)

    FieldCount {          // specify metric name only
        functions = ['total']
    }

    metricset("somepath/MyOtherMetricSet.groovy") {
        ClassLineCount {
            enabled = false
        }

        MethodLineCount name:'CustomMethodLineCount'
    }
}
```

  Things to note:

  * The Groovy *MetricSet* file itself must be accessible on the classpath.

  * The "outer" **metricset** defines the contents of the *MetricSet* (within a *closure*). It can include an optional **description** and any combination of **metricset** (other <MetricSets>) and **metric** statements (individual *Metrics*).

  About the "inner" **metricset** statements:

  * Each **metricset** statement loads a <MetricSet> file. The path specifies a       Groovy file. By default, the paths specified are relative to the classpath.      But these paths may be optionally prefixed by any of the valid `java.net.URL` prefixes, such as "file:" (to load from a relative or absolute path on the filesystem), or "http:".

  * The *MetricSet* can be customized by following the *MetricSet* path with an (optional) *closure*, containing configuration of the properties for individual metrics within the *MetricSet*. Specify the *name* of the metric (as a `String`), followed by its configuration. (Remember to specify the metric *name*, not the class name. In most cases, the metric *name* is the class name without the "Metric" suffix). The name of the *Metric* is followed by:

      + A *Map* of property names and values. See 'MethodLineCount' within the example.

      + A *closure* containing property assignments statements. See 'ClassLineCount' within the example.

  About the **metric** statements:

  * Each **metric** statements loads a single *Metric*.

  * The **metric** statement must specify the class name for a *Metric*. The *Metric* class must be available on the classpath.

  * A **metric** can optionally provide configuration of the *Metric* properties by specifying a *closure* containing property assignment statements. See `AbcMetric` within the example. As within **metricset** statements, properties set this way can be of type `String`, `int`, `long` or `boolean`.

  * The **ClassCount** and **FieldCount** *Metric*s are included by specifying only the *Metric* name (not the class)

  * Setting the `enabled` property of a *Metric* turns it off so that it will not show up in the output report(s).


## Turning off a Metric

  You can turn off a *Metric* by setting its *enabled* boolean property to `false`. It defaults to 
  `true`. This applies to all descendants of `org.gmetrics.metric.AbstractMetric` (i.e., all
  *Metrics* supplied with **GMetrics**).