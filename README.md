# EUCAIM - FL ML DEMO DATA AND MODEL

## Data description

### Datasource

The data comes from the demo data attached to [rexposome](https://www.bioconductor.org/packages/release/bioc/html/rexposome.html) R package.

A brief introduction to the data can be seen in its own [vignette](https://www.bioconductor.org/packages/release/bioc/vignettes/rexposome/inst/doc/exposome_data_analysis.html).

### Preparation

The data was prepared as seen in the following R code:

```r
# LOAD DATA DOWNLOADED FROM REXPOSOME GITHUB
x <- read.csv( "~/Downloads/exposures.csv", header = TRUE, stringsAsFactors = FALSE )
y <- read.csv( "~/Downloads/phenotypes.csv", header = TRUE, stringsAsFactors = FALSE )

# MERGE AS A SINGLE DATA.FRAME
z <- merge( x, y, by = "idnum" )

# CREATE RANDOM ORDER
o <- sample( seq( nrow( z ) ) )

# SPLIT IN THREE RANDOM SETS
z3.1 <- z[ o[  1:36  ], ]
z3.2 <- z[ o[ 37:72  ], ]
z3.3 <- z[ o[ 73:109 ], ]

write.csv( z3.1, file = "./data/three_dataseties_scenario/z3_1.csv", quote = FALSE, row.names = FALSE )
write.csv( z3.2, file = "./data/three_dataseties_scenario/z3_2.csv", quote = FALSE, row.names = FALSE )
write.csv( z3.3, file = "./data/three_dataseties_scenario/z3_3.csv", quote = FALSE, row.names = FALSE )

# SPLINT IN TWO RANDOM SETS
z2.1 <- z[ o[  1:54  ], ]
z2.2 <- z[ o[ 55:109 ], ]

write.csv( z2.1, file = "./data/two_datasites_scenario/z2_1.csv", quote = FALSE, row.names = FALSE )
write.csv( z2.2, file = "./data/two_datasites_scenario/z2_2.csv", quote = FALSE, row.names = FALSE )
```

Therefore:

 * There are two possible scenarios: two or three data-sites available
 * Each file contains a third or a half of the full data-sets
 * Each files contains the same variables (aka. columns)
 * Some variables have missing values codded as `NA`

## Model description

The model to test is the following:

```
blood presure = PM25 + age + sex + cbmi
```

It is read as:

The dependent variables is *blood pressure*, and it is explained by the independent variable *PM25*. The model is also corrected by the phenotipical co-variates *age*, *sex*, and *corrected BMI*.

### Ground truth

In order to validate the results of the FL ML exercise, we will take the following as the ground truth:

```
Call:
lm(formula = blood_pre ~ PM25 + age + sex + cbmi, data = z)

Residuals:
   Min     1Q Median     3Q    Max 
-7.404 -2.528  0.369  2.107  7.575 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) 74.49775   13.68248   5.445 3.60e-07 ***
PM25        37.75120    8.81887   4.281 4.22e-05 ***
age          0.15751    2.22120   0.071    0.944    
sexmale      0.28270    0.68322   0.414    0.680    
cbmi        -0.01247    0.21048  -0.059    0.953    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 3.452 on 102 degrees of freedom
  (2 observations deleted due to missingness)
Multiple R-squared:  0.1556,	Adjusted R-squared:  0.1225 
F-statistic:   4.7 on 4 and 102 DF,  p-value: 0.001602
```

Which is obtained from:

```r
m <- lm( blood_pre ~ PM25 + age + sex + cbmi, data = z )
summary( m )
```
