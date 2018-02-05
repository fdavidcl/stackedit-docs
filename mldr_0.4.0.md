---
title: "mldr 0.4: update now!"
---

We've just released version 0.4 of mldr. Here's a brief look at the changes.

## Reimplementation of evaluation metrics

The set of evaluation metrics included in mldr has been accumulating issues and bug reports in the last months. This fact has encouraged us to fully revise and reimplement these metrics. The new implementations have been thoroughly tested and compared to other in different libraries.

With the aim of facilitating experimentation with different classifiers among diverse platforms, we have provided parameters that customize the behavior of performance metrics when certain values cannot be calculated (e.g. divisions by zero). In particular, mldr mimics the behavior of MULAN metrics by default, but other options are available. Let's look at some examples:

~~~R
true_labels <- matrix(c(
1,1,1,
0,0,0,
1,0,0,
1,1,1,
0,0,0,
1,0,0
), ncol = 3, byrow = TRUE)
predicted_labels <- matrix(c(
1,1,1,
0,0,0,
1,0,0,
1,1,0,
1,0,0,
0,1,0
), ncol = 3, byrow = TRUE)

# strategies for undefined values: "diagnose", "ignore", "na"
precision(true_labels, predicted_labels, undefined_value = "diagnose")
# single value to 
macro_recall(true_labels, predicted_labels, undefined_value = 0)
macro_fmeasure(
  true_labels, predicted_labels,
  undefined_value = function(tp, fp, tn, fn) as.numeric(fp == 0 && fn == 0)
)
~~~

## Improvements on read and write of ARFF files

The parser for ARFF files is now more robust, including support for single-quoted and double-quoted attributes, as well as a negative amount of labels in the MEKA header (indicating labels are at the end of the attribute list).

Exporting to ARFF has seen some improvements as well, but you may want to check out mldr.datasets, which includes more options and support for other formats.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUwMzI3MjE5MV19
-->