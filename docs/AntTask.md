# GMetrics - Ant Task

## Description

  The **GMetrics** Ant Task is implemented by the `org.gmetrics.ant.GMetricsTask` class.

## The Default MetricSet

  If no *MetricSet* is configured for the **GMetrics** Ant Task, then it uses these default *Metrics*:
  * [Cyclomatic Complexity](./metrics/CyclomaticComplexityMetric)
  * [Method Line Count](./metrics/MethodLineCountMetric)
  * [Class Line Count](./metrics/ClassLineCountMetric)


## Attributes

| **Attribute**   | **Description**                                                | **Required**           
|-----------------|----------------------------------------------------------------|------------------------
| *metricSetFile*  | The paths to a Groovy *MetricSet* DSL file. By default, the path specified is relative to the classpath, but it may be optionally prefixed by any of the valid `java.net.URL` prefixes, such as "file:" (to load from a relative or absolute filesystem path), or "http:". If not specified, it uses the set of `Metric`s defined by the [Default MetricSet](./DefaultMetricSet). | No


## Report Nested Element

  The `<report>` nested element defines the type and options for a report. It includes a *type* attribute and contains optional nested `<option>` elements.

| **Attribute**   | **Description**                                                | **Required**           
|-----------------|----------------------------------------------------------------|------------------------
| *type*          | The fully-qualified class name for the report class. This class must implement the `org.gmetrics.report.ReportWriter` interface. | Yes                   

## Option Nested Element

  The `<option>` element is a child of the `<report>` element and defines a
  report-specific option for a report. See the **Example** below.

## Fileset Nested Element

  At least one *fileset* nested element is required, and is used to specify the source files that **GMetrics** should analyze. This is the standard Ant *FileSet*, and is quite powerful and flexible. See the [Apache Ant Manual](http://ant.apache.org/manual/index.html) for more information on *FileSets*.


## Example

  Here is an example Ant XML build file.

```
<taskdef name="gmetrics" classname="org.gmetrics.ant.GMetricsTask">
<target name="runGMetrics">

    <gmetrics metricSetFile="config/metricset.groovy">
        <report type="org.gmetrics.report.BasicHtmlReportWriter">
            <option name="outputFile" value="SampleGMetricsReport.html" />
            <option name="title" value="Sample" />
        </report>
        <fileset dir="src">
            <include name="**/*.groovy"/>
        </fileset>
    </gmetrics>

</target>
```

  Things to note:

  * The *fileset* specifies that all ".groovy" files are analyzed.

  * Remember that you need the **SLF4J** and **Groovy** jars on the classpath.
