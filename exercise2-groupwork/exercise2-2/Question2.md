Question 2
================

Let’s model recall versus risk factors + radiologist, the coefficients
of each variables are shown as below. From this generalized model, we
can see for radiologist 34, he has exp(-0.52) ≈ 0.6 to recall a patient,
holding all other factors constant. radiologist 89 has exp(0.46) ≈ 1.58
times to recall a patient, holding all other factors constant.
radiologist66 has exp(0.35) ≈ 1.42 times to recall a patient, holding
all other factors constant. So we can say radiologist 89 is more
conservative while radiologist 34 is less conservative among
radiologists, holding all other factors
constant.

``` 
             (Intercept) radiologistradiologist34 radiologistradiologist66 
             -3.27515411              -0.52170556               0.35465994 
radiologistradiologist89 radiologistradiologist95               ageage5059 
              0.46376020              -0.05219281               0.11120701 
              ageage6069             ageage70plus                  history 
              0.15683414               0.10781791               0.21588355 
                symptoms    menopausepostmenoNoHT menopausepostmenounknown 
              0.72928116              -0.19341850               0.40266830 
        menopausepremeno          densitydensity2          densitydensity3 
              0.34207511               1.22015056               1.41906920 
         densitydensity4 
              1.00033739 
```

Then we can consider which radiologist is more accurate by seeing the
confusion matrix. For FDR, for radiologist 89, it is 33/38, while for
radiologist 34, it is 13/17, which the radiologist 89 has the higher
one. However, for FDR, it is lower for radiologist 89, as 2/7 is lower
than 3/7 for radiologist 34. So considering the confusion matrix, it is
not clear to say for the accuracy.

``` 
      recall
cancer   0   1
     0 177  13
     1   3   4
```

``` 
      recall
cancer   0   1
     0 157  33
     1   2   5
```

For part 2 in this question. Firstly, for Model A, we regress the cancer
outcome on the recall decision. The coefficients and the significance
level of the model is shown as below. We can say that as recall increase
by 2 unit, the cancer will increase by exp(2.26) = 9.58.

    (Intercept)      recall 
      -4.006120    2.260881 

``` 

Call:
glm(formula = cancer ~ recall, family = binomial, data = brca)

Deviance Residuals: 
    Min       1Q   Median       3Q      Max  
-0.5673  -0.1900  -0.1900  -0.1900   2.8370  

Coefficients:
            Estimate Std. Error z value Pr(>|z|)    
(Intercept)  -4.0061     0.2605 -15.378  < 2e-16 ***
recall        2.2609     0.3482   6.493 8.43e-11 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 315.59  on 986  degrees of freedom
Residual deviance: 274.88  on 985  degrees of freedom
AIC: 278.88

Number of Fisher Scoring iterations: 6
```

And then we regress the Model B for the cancer on the recall decision
and family history.The coefficients and the significance level of the
model is shown as below.

    (Intercept)      recall     history 
     -4.0447843   2.2567837   0.2060725 

``` 

Call:
glm(formula = cancer ~ recall + history, family = binomial, data = brca)

Deviance Residuals: 
    Min       1Q   Median       3Q      Max  
-0.6115  -0.2064  -0.1863  -0.1863   2.8503  

Coefficients:
            Estimate Std. Error z value Pr(>|z|)    
(Intercept)  -4.0448     0.2743 -14.745  < 2e-16 ***
recall        2.2568     0.3484   6.478 9.28e-11 ***
history       0.2061     0.4231   0.487    0.626    
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 315.59  on 986  degrees of freedom
Residual deviance: 274.65  on 984  degrees of freedom
AIC: 280.65

Number of Fisher Scoring iterations: 6
```

If the radiolist were appropriately accounting for a patients’ family
history for the cancer, I think the regression result of Model B would
be better than Model A. Because for model B, it takes the ommited
variable in Model A into consideration. However, by seeing the p-value,
AIC index and the R-squared, Model B is not better than Model A, because
AIC for Model B is higher and p-value is not significant. From this
result, we can say for radiologist, the family history is not a main
reason for this cancer to justify the existence of cancer and recall.
After that, we should also consider other reasons for radiologists to
recall patients. To be specific, we can model the cancer result on the
risk
factors

``` 
             (Intercept) radiologistradiologist34 radiologistradiologist66 
            -5.475183784              0.019053992             -0.369522198 
radiologistradiologist89 radiologistradiologist95                   recall 
            -0.233148295             -0.384848670              2.335523469 
              ageage5059               ageage6069             ageage70plus 
             0.477892952              0.398328363              1.436442550 
                 history                 symptoms    menopausepostmenoNoHT 
             0.247483851             -0.008199087             -0.173097080 
menopausepostmenounknown         menopausepremeno          densitydensity2 
             0.819953395              0.230477972              0.718016350 
         densitydensity3          densitydensity4 
             0.834955961              1.998087115 
```

From the regression result, we can see that patients and older have
exp(1.44) ≈ 4.2 the probability of having cancer, holding all else
fixed. So it is reasonable for the radiologists to pay more attention to
the patients who are 70 and older. Also, patients with tissue density
classification 4 have exp(2) ≈ 7.4 times the probability of having
cancer as patients with density 1, holding all else fixed. For
radiologists, in order to make decisions more precisely, we should also
consider the error rates, shown as below. For patiens who are 70 and
older, the FDR, it is 70%. And for the patients with density 4, the FDR
is 0.73.

    , , age = age4049
    
          recall
    cancer           0           1
         0 0.978991597 0.918367347
         1 0.021008403 0.081632653
    
    , , age = age5059
    
          recall
    cancer           0           1
         0 0.983333333 0.863636364
         1 0.016666667 0.136363636
    
    , , age = age6069
    
          recall
    cancer           0           1
         0 0.994152047 0.857142857
         1 0.005847953 0.142857143
    
    , , age = age70plus
    
          recall
    cancer           0           1
         0 0.973684211 0.703703704
         1 0.026315789 0.296296296

    , , density = density1
    
          recall
    cancer          0          1
         0 1.00000000 0.75000000
         1 0.00000000 0.25000000
    
    , , density = density2
    
          recall
    cancer          0          1
         0 0.98591549 0.85416667
         1 0.01408451 0.14583333
    
    , , density = density3
    
          recall
    cancer          0          1
         0 0.98153034 0.87654321
         1 0.01846966 0.12345679
    
    , , density = density4
    
          recall
    cancer          0          1
         0 0.95604396 0.73333333
         1 0.04395604 0.26666667
