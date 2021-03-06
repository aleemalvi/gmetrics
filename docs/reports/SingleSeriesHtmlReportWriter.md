# SingleSeriesHtmlReportWriter

## Description

  The `org.gmetrics.report.SingleSeriesHtmlReportWriter` class produces an HTML report of metric results based on a single metric, single level and single function to provide a single series of data.

  The *metric*, *level* and *function* properties are required (must be non-null and non-empty). These three properties uniquely identify a single series of metric values.

  See a [Sample Report](./SampleGMetricsSingleSeriesReport.html).

## Option Nested Elements

The *option* element is a child of the *report* element and defines a report-specific option for a report.

  `org.gmetrics.report.SingleSeriesHtmlReportWriter` supports the following options:

| **Attribute**         | **Description**                                                | **Required**           
|-----------------------|----------------------------------------------------------------|------------------------
| *metric*              | The name (case-sensitive) of a single Metric included in the analysis results (e.g. "CyclomaticComplexity"). | Yes
| *level*               | The single level at which results are included in the report. Valid *level* values are "package", "class" and "method". | Yes
| *function*            | The function for which results are included in the report. Valid *function* values are metric-specific, but are typically "total", "average", "minimum" or "maximum". | Yes
| *outputFile*          | The path and filename for the output report file.              | No
| *title*               | The title for the output report.                               | No
| *writeToStandardOut*  | Set to "true" or `true` to write out the report to *stdout* (`System.out`) instead of writing to a file. | No
| *sort*                | Controls whether the report results are sorted numerically. A value of *null* or empty means no sorting is performed; otherwise, the value must be either "ascending" or "descending". | No
| *maxResults*          | Specifies the limit on the number of results included in the report. A value of *null*, zero or empty means no limit is applied; otherwise the value must be a positive integer. | No
| *greaterThan*         | Specifies a lower-bound threshold -- only results with a larger value are included within the report. A value of *null* or empty means no lower-bound threshold is applied. | No
| *lessThan*            | Specifies an upper-bound threshold -- only results with a smaller value are included within the report. A value of *null* or empty means no upper-bound threshold is applied. | No

## Example

  Here is an example Ant XML build file illustrating configuration of
  `org.gmetrics.report.SingleSeriesHtmlReportWriter`.

```
  <taskdef name="gmetrics" classname="org.gmetrics.ant.GMetricsTask">
  <target name="runGMetrics">
    <gmetrics>
        <report type="org.gmetrics.report.SingleSeriesHtmlReportWriter">
            <option name="outputFile" value="SampleGMetricsReport.html" />
            <option name="title" value="Sample" />
            <option name="metric" value="CyclomaticComplexity" />
            <option name="level" value="class" />
            <option name="function" value="average" />
            <option name="maxResults" value="50" />
            <option name="greaterThan" value="100.0" />
        </report>
        <fileset dir="src">
            <include name="**/*.groovy"/>
        </fileset>
    </gmetrics>
  </target>
```
