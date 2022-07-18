+++
title = "myDatabase"
linkTitle = "myDatabase"
description = "myDatabase Integration Specification"
weight = 100
+++

This document describes the input file format and output database table format for integration with myDatabase.

# Input File Format

TBD

## Shop File Format

TBD

{{< table paging=1 >}}
<thead class="thead-dark" responsive=1>
    <tr>
        <th>Field</th>
        <th>Type</th>
        <th>Required</th>
        <th>Description</th>
    </tr>
</thead>
<tbody>
    <tr>
        <td><code>Id</code></td>
        <td><code>int</code></td>
        <td>YES</td>
        <td>Record identifier. The record is ignored if a subsequent record has the same identifier.</td>
    </tr>
    <tr>
        <td><code>manager</code></td>
        <td><code>char</code></td>
        <td>YES</td>
        <td>Manager identifier. This field links to the <code>id</code> field of the person input file.</td>
    </tr>
    <tr>
        <td><code>hours</code></td>
        <td><code>date</code></td>
        <td>NO</td>
        <td>Time interval covered by this record. Format: (open time-close time) &lt;HH&gt;:&lt;MM&gt;-&lt;HH&gt;:&lt;MM&gt;, Example: <code>07:30-20:45</code></td>
    </tr>
    <tr>
        <td><code>sales</code></td>
        <td><code>dec</code></td>
        <td>CONDITIONAL</td>
        <td>Total sales amount for the <code>hours</code> interval. Not used if <code>hours</code> field is empty.</td>
    </tr>
    <tr>
        <td><code>ccy</code></td>
        <td><code>char</code></td>
        <td>CONDITIONAL</td>
        <td>Currency symbol. Not used if <code>hours</code> field is empty.</td>
    </tr>
</tbody>
{{< /table >}}

Example:

![](/exampleInputShop.png)

## Person File Format

TBD

{{< table paging=1 >}}
<thead class="thead-dark">
    <tr>
        <th>Field</th>
        <th>Type</th>
        <th>Required</th>
        <th>Description</th>
    </tr>
</thead>
<tbody>
    <tr>
        <td><code>id</code></td>
        <td><code>char</code></td>
        <td>YES</td>
        <td>Manager identifier. This is the join from the shop input file <code>manager</code> field.</td>
    </tr>
    <tr>
        <td><code>name</code></td>
        <td><code>char</code></td>
        <td>YES</td>
        <td>Manager name, all lower case.</td>
    </tr>
    <tr>
        <td><code>joined_date</code></td>
        <td><code>date</code></td>
        <td>NO</td>
        <td>Manager start of employment data. This field is not used.</td>
    </tr>
    <tr>
        <td><code>active</code></td>
        <td><code>char</code></td>
        <td>YES</td>
        <td>Manager status: <code>Y</code> = active, <code>N</code> = inactive. If the status is inactive, a record is not added to the database table.</td>
    </tr>
</tbody>
{{< /table >}}

Example:

![](/exampleInputPerson.png)

# Database Format

TBD

{{< table paging=1 >}}
<thead class="thead-dark">
    <tr>
        <th>Field</th>
        <th>Type</th>
        <th>Null</th>
        <th>Key</th>
        <th>Default</th>
        <th>Notes</th>
    </tr>
</thead>
<tbody>
    <tr>
        <td><code>ID</code></code></td>
        <td>int</td>
        <td></td>
        <td>Primary</td>
        <td></td>
        <td>Shop identifier.</td>
    </tr>
    <tr>
        <td><code>DATE</code></td>
        <td>date</td>
        <td></td>
        <td></td>
        <td></td>
        <td>The shop records date. The date from the shop CSV file name: <code>shop_&lt;date&gt;.csv</code>. Example: <code>shop_20180511.csv</code>. The data is encoded in this field as &lt;year&gt;-&lt;month&gt;-&lt;day&gt; Example: 2018-05-11</td>
    </tr>
    <tr>
        <td><code>MANAGER</code></td>
        <td>char</td>
        <td></td>
        <td></td>
        <td></td>
        <td>Manager name from the person input file <code>name</code> field.</td>
    </tr>
    <tr>
        <td><code>OPEN</code></td>
        <td>char</td>
        <td>YES</td>
        <td></td>
        <td></td>
        <td>Shop open time derived from the opening time of the shop input file <code>hours</code> field. Set to <code>null</code> if the shop input file <code>hours</code> field is empty. The opening time is encoded in this field as &lt;HH&gh;:&lt;MM&gh;</td>
    </tr>
    <tr>
        <td><code>CLOSE</code></td>
        <td>char</td>
        <td>YES</td>
        <td></td>
        <td></td>
        <td>Shop close time derived from the closing time of the shop input file <code>hours</code> field. Set to <code>null</code> if the shop input file <code>hours</code> field is empty. The closing time is encoded in this field as &lt;HH&gh;:&lt;MM&gh;</td>
    </tr>
    <tr>
        <td><code>DURATION</code></td>
        <td>int</td>
        <td></td>
        <td></td>
        <td><code>0</code></td>
        <td>Duration from <code>OPEN</code> to <code>CLOSE</code>, in minutes. Set to <code>0</code> if the shop input file <code>hours</code> field is empty.</td>
    </tr>
    <tr>
        <td><code>REVENUE</code></td>
        <td>decimal</td>
        <td></td>
        <td></td>
        <td><code>0</code></td>
        <td>Revenue from the shop input file <code>sales</code> field. Set to <code>0</code> if the shop input file <code>hours</code> field is empty.</td>
    </tr>
    <tr>
        <td><code>CCY</code></td>
        <td>char</td>
        <td></td>
        <td></td>
        <td><code>N/A</code></td>
        <td>Alphabetic code associated with the currency symbol in the shop input file <code>ccy</code> field, as specified by the <a href= https://www.six-group.com/en/products-services/financial-information/data-standards.html>Currency Code Services – ISO 4217 Maintenance Agency</a>. Set to <code>N/A</code> if the shop input file <code>hours</code> field is empty.</td>
    </tr>
</tbody>
{{< /table >}}

Example:

![](/exampleTable.png)

# Reference

[ISO 4217 Currency Codes](https://www.iso.org/iso-4217-currency-codes.html)
