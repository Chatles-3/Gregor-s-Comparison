# Gregor-s-Comparison
Includes a description of the method and accompanying formula, usage instructions and example, guidance on application, and R script.

# Description
Gregor’s Comparison is named after my fluffy golden cat back home, Gregor, because of its use of the golden ratio. The method is similar to the Bonferroni Correction in that it divides the significance level α by the number of comparisons, C(I,2), but it modifies the Correction further by artificially raising the degrees of freedom for some number of means, I, and number of observations, J, given I is greater than or equal to three and J greater than or equal to 5. Gregor's Comparison is therefore a more liberal method than Bonferroni which is preferable in controlling the family-wise error rate (FWER) to an acceptable range (0.03-0.07).

The Comparison raises the df used in calculating the t-critical value by multiplying the standard df, (IJ-I), by the golden ratio, φ. However, this is where we run into a problem: the degrees of freedom do not scale linearly with I or J, thereby producing FWERs well outside the target range for most values of I and J. In particular, the uncontrolled method is excessively liberal for higher values of I and J. To control for this, we first subtract φ(IJ-I) by I-squared, as this variable was the less sensitive of the two, and then divide the whole result by the square root of J/10. In testing the strength of the uncontrolled method, scenarios involving J = 10 typically landed in the target range, hence why this control does not affect the df calculation when J = 10.

From there, the method works similarly to a typical confidence interval. We use both the significance level α and the degrees of freedom to calculate the t-critical value, which, when multiplied by the standard error, produces our significant difference, in this case referred to as Gregor's Significant Difference (GSD). If two means possess a greater difference than the GSD, then such two means are significant from each other.

# Formula
Putting it all together, the formula for Gregor's Comparison is as follows:

GSD = t * SE, where t has a significance level of α/C(I,2) and df (φ * ((I*(J-1))) + I^2) / sqrt(J/10)).

If the difference of two means exceeds the GSD, then the comparison is significant. Otherwise, it is not.

# Usage Instructions
This code does not require the installation of any extra packages. When running the code, the comparison function, titled "greg_comparison" will produce a table as an output; the rightmost column is the one to pay attention to, as it contains the significance determination for each comparison. "TRUE" means that the comparison is significant, while "FALSE" means the comparison is not significant. When simulating the power of both Gregor's and Tukey's method, the output will consist of a vector whose first value refers to Gregor's power, the second Tukey's power, and the third the p-value of their difference using McNemar's test.

# Example
The R script comes prepared with an example that will run if you run the entire code at once. You can find a description of the scenario here:

An experiment compared 5 brands of automobile oil filters with respect to their ability to capture foreign material. The means of each brand are as follows: 14.5, 13.8, 13.3, 14.3, 13.1. There are five brands (I = 5) and each brand consists of nine filter observations (J = 9). An ANOVA revealed that the brands are significantly different in their ability to capture foreign material, but we must run a post-hoc test to determine which specific means are significant. Now, we run Gregor's Comparison:

means <- c(14.5,13.8,13.3,14.3,13.1)
greg_comparison(means, J = 9, MSE = 0.088, alpha = 0.05)

 Comp group1 group2 diff significant
1     1   14.5   13.8 -0.7        TRUE
2     2   14.5   13.3 -1.2        TRUE
3     3   14.5   14.3 -0.2       FALSE
4     4   14.5   13.1 -1.4        TRUE
5     5   13.8   13.3 -0.5        TRUE
6     6   13.8   14.3  0.5        TRUE
7     7   13.8   13.1 -0.7        TRUE
8     8   13.3   14.3  1.0        TRUE
9     9   13.3   13.1 -0.2       FALSE
10   10   14.3   13.1 -1.2        TRUE

Gregor's Comparison determined that the only non-significant differences were between Brands 1 & 4 and 3 & 5. Brand 2 is significantly different from all other brands. Brands 1 & 4 capture the most amount of material, brand 2 captures a middling amount of material, and brands 3 & 5 capture the least amount of material.

# Applications
Although Gregor’s Comparison successfully controls FWER to an acceptable range, it is significantly less powerful than Tukey’s HSD for middling effect sizes statistically, although the true difference is not very large. At these sizes, it is safer to use Tukey’s HSD, but for smaller and larger effect sizes, the two methods are practically interchangeable. One should not extrapolate Gregor’s Comparison beyond the tested scenarios, as the method likely breaks down for increasingly large values of I. However, this is likely not practically significant, as the tested scenarios describe frequent parameters for multiple comparisons.
