# XmlReportWriter

## Description

  The `org.gmetrics.report.XmlReportWriter` class produces an XML report of **GMetrics** results.

  See a [Sample Report](./SampleGMetricsXmlReport.xml).

## Option Nested Elements

The *option* element is a child of the *report* element and defines a report-specific option for a report.

  `org.gmetrics.report.BasicHtmlReportWriter` supports the following options:

| **Attribute**        | **Description**                                                | **Required**           
|----------------------|----------------------------------------------------------------|------------------------
| *functions*          | The (comma-separated) list of functions for which results are included in the report for a particular metric. Multiple metrics are configured by separating them with semicolons (;). By default, all functions provided by a metric are included within the report. Any metrics not explicitly configured include all applicable functions as well. Valid *function* values are metric-specific, but typically include "total", "average", "minimum" and "maximum". For example, a value of `"MethodLineCount=minimum,average"` for `functions` includes only the *minimum* and *average* function results for the **MethodLineCount** metric. Likewise, a value of `"ABC=average; CyclomaticComplexity=total,maximum"` includes the *average* function results for the **ABC** metric and the *total* and *maximum* function results for the **CyclomaticComplexity** metric. Note that all functions provided by any other metrics (not configured) are included. | No
| *levels*             | The (comma-separated) list of levels at which results are included in the report for a particular metric. Multiple metrics are configured by separating them with semicolons (;). By default, all levels are included for all metrics within the report. Any metrics not explicitly configured include all applicable levels as well. Valid *level* values are "package", "class" and "method". For example, a value of `"MethodLineCount=class,method"` for `levels` includes only the *class* and *method* level results for the **MethodLineCount* metric. Likewise, a value of `"ABC=method; CyclomaticComplexity=package,class"` includes the *method* level results for the **ABC** metric and the *package* and *class* level results for the **CyclomaticComplexity* metric. Note that all levels are included for any other metrics not explicitly configured. | No
| *metrics*            | The (comma-separated) list of names of the metrics included in the report. By default, all metrics within the results are included in the report. For example, a `metrics` value of `"ABC, MethodLineCount"` includes only the **ABC** and **MethodLineCount** metrics within the report. | No
| *outputFile*         | The path and filename for the output report file.              | No                     
| *title*              | The title for the output report.                               | No                     
| *writeToStandardOut* | Set to "true" or `true` to write out the report to *stdout* (`System.out`) instead of writing to a file. | No      

## Example

  Here is an example Ant XML build file illustrating configuration of
  `org.gmetrics.report.XmlReportWriter`.

```
<taskdef name="gmetrics" classname="org.gmetrics.ant.GMetricsTask">
<target name="runGMetrics">
    <gmetrics>
        <report type="org.gmetrics.report.XmlReportWriter">
            <option name="outputFile" value="SampleGMetricsXmlReport.html" />
        </report>
        <fileset dir="src">
            <include name="**/*.groovy"/>
        </fileset>
    </gmetrics>
</target>
```

  And here is an example illustrating explicit configuration of the *metrics*, *levels* and *functions* to be included within the report:

```
<taskdef name="gmetrics" classname="org.gmetrics.ant.GMetricsTask">
<target name="runGMetrics">
    <gmetrics>
        <report type="org.gmetrics.report.XmlReportWriter">
            <option name="outputFile" value="SampleGMetricsXmlReport.html" />
            <option name="metrics" value="CyclomaticComplexity, ABC" />
            <option name="levels" value="CyclomaticComplexity=class,method; ABC=method" />
            <option name="functions" value="ABC=average; CyclomaticComplexity=total,maximum" />
        </report>
        <fileset dir="src">
            <include name="**/*.groovy"/>
        </fileset>
    </gmetrics>
</target>
```