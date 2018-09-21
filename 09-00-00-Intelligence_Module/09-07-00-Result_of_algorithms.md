# Results of algorithms #

The results of the "AI algorithms" are saved to the index „intelligence" specially created
for this purpose. The index with the prediction result. These
following fields are available in the index (where xxx is the name of
the attribute being analyzed):

-   **xxx\_pre** - estimate value
-   **xxx\_cur** - current value at the moment of estimation
-   **method\_name** - name of the algorithm used
-   **rmse** - avarage square error for the analysis in which \_cur values
     were available. **The smaller the value, the better.**
-   **rmse\_normalized** - mean square error for the analysis in which
     \_cur values were available, normalized with \_pre values. **The
     smaller the value, the better.**
-   **overall\_efficiency** - efficiency of the model. **The greater the
     value, the better. A value less than 0 may indicate too little
     data to correctly calculate the indicator**
-   **linear\_function\_a** - directional coefficient of the linear
     function y = ax + b. **Only for the Trend and Linear Regression
     Trend algorithm**
-   **linear\_function\_b** - the intersection of the line with the Y axis
     for the linear function y = ax + b. **Only for the Trend and
     Linear Regression Trend algorithm.**

Visualization and signals related to the results of data analysis
should be created from this index. The index should be available to
users of the Intelligence module.
