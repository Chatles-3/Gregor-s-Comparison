# Gregor-s-Comparison
Includes a description of the method and accompanying formula, usage instructions and example, guidance on application, and R script.

# Description
Gregor’s Comparison is named after my fluffy golden cat back home, Gregor, because of its use of the golden ratio. The method is similar to the Bonferroni Correction in that it divides the significance level α by the number of comparisons, C(I,2), but it modifies the Correction further by artificially raising the degrees of freedom for some number of means, I, and number of observations, J, given I is greater than or equal to three and J greater than or equal to 5. Gregor's Comparison is therefore a more liberal method than Bonferroni which is preferable in controlling the family-wise error rate (FWER) to an acceptable range (0.03-0.07).

The Comparison raises the df used in calculating the t-critical value by multiplying the standard df, (IJ-I), by the golden ratio, φ. However, this is where we run into a problem: the degrees of freedom do not scale linearly with I or J, thereby producing FWERs well outside the target range for most values of I and J. In particular, the uncontrolled method is excessively liberal for higher values of I and J. To control for this, we first subtract φ(IJ-I) by I-squared, as this variable was the less sensitive of the two, and then divide the whole result by the square root of J/10. In testing the strength of the uncontrolled method, scenarios involving J = 10 typically landed in the target range, hence why this control does not affect the df calculation when J = 10.

From there, the method works similarly to a typical confidence interval. We use both the significance level α and the degrees of freedom to calculate the t-critical value, which, when multiplied by the standard error, produces our significant difference, in this case referred to as Gregor's Significant Difference (GSD). If two means possess a greater difference than the GSD, then such two means are significant from each other.

# Formula
Putting it all together, the formula for Gregor's Comparison is as follows:
