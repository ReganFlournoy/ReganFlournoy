# Load necessary libraries
> library(ggplot2)
> library(broom)
> 
> # Step 1: Create the data frame
> data <- data.frame(
+     monthly_bills = c(1000, 1500, 1200, 1600, 1300, 1100, 1400, 1200, 1500, 1400, 1800, 1800),
+     yearly_holidays = c(3, 3, 3, 0, 4, 3, 1, 1, 3, 3, 7, 5),
+     anxiety = c(5, 7, 4, 7, 6, 4, 7, 5, 7, 6, 9, 9)
+ )
> 
> # Step 2: Fit a multiple linear regression model
> model <- lm(anxiety ~ monthly_bills + yearly_holidays, data = data)
> 
> # Step 3: Examine the model summary
> summary(model)

Call:
lm(formula = anxiety ~ monthly_bills + yearly_holidays, data = data)

Residuals:
     Min       1Q   Median       3Q      Max 
-1.15076 -0.31732  0.05251  0.17168  1.03181 

Coefficients:
                  Estimate Std. Error t value Pr(>|t|)    
(Intercept)     -2.1485839  1.0707860  -2.007   0.0757 .  
monthly_bills    0.0059129  0.0008173   7.235  4.9e-05 ***
yearly_holidays  0.0679739  0.1124960   0.604   0.5606    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.6351 on 9 degrees of freedom
Multiple R-squared:  0.8816,	Adjusted R-squared:  0.8553 
F-statistic: 33.51 on 2 and 9 DF,  p-value: 6.758e-05

> 
> # Step 4: Visualize the relationship (2D plot for each variable vs. anxiety)
> # Monthly bills vs Anxiety
> ggplot(data, aes(x = monthly_bills, y = anxiety)) +
+     geom_point() +
+     geom_smooth(method = "lm", color = "blue", se = FALSE) +
+     labs(title = "Monthly Bills vs Anxiety",
+          x = "Monthly Bills",
+          y = "Anxiety") +
+     theme_minimal()
`geom_smooth()` using formula = 'y ~ x'
> 
> # Yearly holidays vs Anxiety
> ggplot(data, aes(x = yearly_holidays, y = anxiety)) +
+     geom_point() +
+     geom_smooth(method = "lm", color = "red", se = FALSE) +
+     labs(title = "Yearly Holidays vs Anxiety",
+          x = "Yearly Holidays",
+          y = "Anxiety") +
+     theme_minimal()
`geom_smooth()` using formula = 'y ~ x'
> 
> # Step 5: Use broom to get a tidy version of the regression coefficients
> tidy(model)
# A tibble: 3 × 5
  term            estimate std.error statistic   p.value
  <chr>              <dbl>     <dbl>     <dbl>     <dbl>
1 (Intercept)     -2.15     1.07        -2.01  0.0757   
2 monthly_bills    0.00591  0.000817     7.23  0.0000490
3 yearly_holidays  0.0680   0.112        0.604 0.561    
> 
> # Step 6: Diagnostic plots
> par(mfrow = c(2, 2))  # Set up for 4 diagnostic plots
> plot(model)
> 
> # Step 7: Predictions
> # Create new data for predictions
> new_data <- data.frame(
+     monthly_bills = c(1200, 1600, 1800),
+     yearly_holidays = c(3, 5, 7)
+ )
> predictions <- predict(model, newdata = new_data, interval = "confidence")
> 
> # Print predictions
> print(data.frame(new_data, predictions))
  monthly_bills yearly_holidays      fit      lwr       upr
1          1200               3 5.150763 4.595116  5.706409
2          1600               5 7.651852 7.006345  8.297359
3          1800               7 8.970370 7.897688 10.043053

