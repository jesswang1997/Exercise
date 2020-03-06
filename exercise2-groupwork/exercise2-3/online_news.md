Exercise 2-3
============

-   Course: Data Mining and Statistical Learning (ECO395M)
-   Name: Xuechun Wang (xw5996)、Hanqi Liu(hl27963)
-   Date: March 6th, 2020
-   Data Source: online\_news.csv

Preparation for model building
==============================

Before building model, we need to choose the right form of “shares”.
Here is a histogram of “shares”:

![](online_news_files/figure-markdown_strict/unnamed-chunk-1-1.png)

From the histogram, we can see that “shares” is hugely skewed, so we
probably want a log transformation for “shares”. Here is a histogram of
“log(shares)”:

![](online_news_files/figure-markdown_strict/unnamed-chunk-2-1.png)

First approach: build the best model
====================================

-   First, build a baseline model with log(shares) versus all variables
    except url

<!-- -->


    Call:
    lm(formula = log(shares) ~ . - url - viral, data = online_news)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -7.6606 -0.5657 -0.1781  0.4112  6.0146 

    Coefficients: (2 not defined because of singularities)
                                    Estimate Std. Error t value Pr(>|t|)    
    (Intercept)                    7.897e+00  4.360e-02 181.133  < 2e-16 ***
    n_tokens_title                 3.314e-03  2.167e-03   1.529  0.12628    
    n_tokens_content               1.352e-05  1.394e-05   0.970  0.33209    
    num_hrefs                      6.175e-03  4.990e-04  12.374  < 2e-16 ***
    num_self_hrefs                -1.074e-02  1.363e-03  -7.880 3.36e-15 ***
    num_imgs                       4.508e-03  6.343e-04   7.108 1.20e-12 ***
    num_videos                     3.243e-03  1.176e-03   2.757  0.00583 ** 
    average_token_length          -6.305e-02  7.276e-03  -8.667  < 2e-16 ***
    num_keywords                   1.658e-02  2.464e-03   6.730 1.72e-11 ***
    data_channel_is_lifestyle     -1.874e-01  2.345e-02  -7.993 1.35e-15 ***
    data_channel_is_entertainment -4.419e-01  1.642e-02 -26.916  < 2e-16 ***
    data_channel_is_bus           -2.683e-01  1.808e-02 -14.840  < 2e-16 ***
    data_channel_is_socmed         5.294e-02  2.299e-02   2.303  0.02129 *  
    data_channel_is_tech          -1.220e-01  1.723e-02  -7.081 1.45e-12 ***
    data_channel_is_world         -4.990e-01  1.742e-02 -28.647  < 2e-16 ***
    self_reference_min_shares      7.542e-07  5.814e-07   1.297  0.19455    
    self_reference_max_shares      2.516e-07  3.151e-07   0.798  0.42459    
    self_reference_avg_sharess     1.812e-06  8.065e-07   2.247  0.02462 *  
    weekday_is_monday             -2.349e-01  2.030e-02 -11.572  < 2e-16 ***
    weekday_is_tuesday            -2.930e-01  2.000e-02 -14.651  < 2e-16 ***
    weekday_is_wednesday          -2.948e-01  1.999e-02 -14.742  < 2e-16 ***
    weekday_is_thursday           -2.847e-01  2.004e-02 -14.205  < 2e-16 ***
    weekday_is_friday             -2.214e-01  2.076e-02 -10.664  < 2e-16 ***
    weekday_is_saturday            1.004e-02  2.477e-02   0.405  0.68522    
    weekday_is_sunday                     NA         NA      NA       NA    
    is_weekend                            NA         NA      NA       NA    
    global_rate_positive_words     2.474e-01  3.218e-01   0.769  0.44192    
    global_rate_negative_words    -6.403e-01  5.076e-01  -1.262  0.20713    
    avg_positive_polarity          2.259e-01  8.173e-02   2.764  0.00572 ** 
    min_positive_polarity         -2.753e-01  8.532e-02  -3.226  0.00125 ** 
    max_positive_polarity         -2.889e-02  3.262e-02  -0.885  0.37592    
    avg_negative_polarity         -3.613e-01  9.022e-02  -4.005 6.21e-05 ***
    min_negative_polarity          2.407e-02  3.507e-02   0.686  0.49259    
    max_negative_polarity          1.364e-01  7.889e-02   1.729  0.08387 .  
    title_subjectivity             3.950e-02  2.001e-02   1.974  0.04835 *  
    title_sentiment_polarity       6.976e-02  1.915e-02   3.643  0.00027 ***
    abs_title_sentiment_polarity   3.276e-02  3.052e-02   1.073  0.28321    
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.8895 on 39609 degrees of freedom
    Multiple R-squared:  0.08689,   Adjusted R-squared:  0.0861 
    F-statistic: 110.9 on 34 and 39609 DF,  p-value: < 2.2e-16

-   Second, drop things that seem (nearly) perfectly collinear with
    other variables

<!-- -->


    Call:
    lm(formula = log(shares) ~ . - url - n_tokens_content - self_reference_max_shares - 
        weekday_is_saturday - weekday_is_sunday - is_weekend - max_positive_polarity - 
        min_negative_polarity - viral, data = online_news)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -7.6635 -0.5651 -0.1783  0.4124  6.0075 

    Coefficients:
                                    Estimate Std. Error t value Pr(>|t|)    
    (Intercept)                    7.902e+00  4.173e-02 189.353  < 2e-16 ***
    n_tokens_title                 3.260e-03  2.165e-03   1.506 0.132117    
    num_hrefs                      6.234e-03  4.840e-04  12.880  < 2e-16 ***
    num_self_hrefs                -1.043e-02  1.330e-03  -7.840 4.60e-15 ***
    num_imgs                       4.614e-03  6.063e-04   7.611 2.78e-14 ***
    num_videos                     3.372e-03  1.167e-03   2.890 0.003853 ** 
    average_token_length          -6.365e-02  7.249e-03  -8.781  < 2e-16 ***
    num_keywords                   1.654e-02  2.463e-03   6.714 1.91e-11 ***
    data_channel_is_lifestyle     -1.856e-01  2.317e-02  -8.014 1.14e-15 ***
    data_channel_is_entertainment -4.403e-01  1.613e-02 -27.305  < 2e-16 ***
    data_channel_is_bus           -2.660e-01  1.762e-02 -15.092  < 2e-16 ***
    data_channel_is_socmed         5.540e-02  2.273e-02   2.438 0.014784 *  
    data_channel_is_tech          -1.199e-01  1.677e-02  -7.147 9.00e-13 ***
    data_channel_is_world         -4.969e-01  1.677e-02 -29.626  < 2e-16 ***
    self_reference_min_shares      4.178e-07  3.966e-07   1.054 0.292086    
    self_reference_avg_sharess     2.399e-06  3.237e-07   7.411 1.28e-13 ***
    weekday_is_monday             -2.401e-01  1.657e-02 -14.490  < 2e-16 ***
    weekday_is_tuesday            -2.980e-01  1.620e-02 -18.398  < 2e-16 ***
    weekday_is_wednesday          -2.998e-01  1.620e-02 -18.508  < 2e-16 ***
    weekday_is_thursday           -2.897e-01  1.626e-02 -17.818  < 2e-16 ***
    weekday_is_friday             -2.263e-01  1.714e-02 -13.205  < 2e-16 ***
    global_rate_positive_words     1.911e-01  3.110e-01   0.614 0.538945    
    global_rate_negative_words    -7.000e-01  4.954e-01  -1.413 0.157645    
    avg_positive_polarity          1.822e-01  6.173e-02   2.951 0.003167 ** 
    min_positive_polarity         -2.586e-01  7.897e-02  -3.275 0.001057 ** 
    avg_negative_polarity         -3.143e-01  5.282e-02  -5.951 2.68e-09 ***
    max_negative_polarity          1.145e-01  6.535e-02   1.752 0.079814 .  
    title_subjectivity             3.970e-02  2.000e-02   1.985 0.047184 *  
    title_sentiment_polarity       6.962e-02  1.913e-02   3.640 0.000273 ***
    abs_title_sentiment_polarity   3.286e-02  3.051e-02   1.077 0.281364    
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.8895 on 39614 degrees of freedom
    Multiple R-squared:  0.08683,   Adjusted R-squared:  0.08616 
    F-statistic: 129.9 on 29 and 39614 DF,  p-value: < 2.2e-16

-   Third, use stepwise selection to build a “best” model. In order to
    save the running time of the program，we choose steps = 2 here, to
    improve just a little bit from the baseline model

<!-- -->

    lm(formula = log(shares) ~ n_tokens_title + num_hrefs + num_self_hrefs + 
        num_imgs + num_videos + average_token_length + num_keywords + 
        data_channel_is_lifestyle + data_channel_is_entertainment + 
        data_channel_is_bus + data_channel_is_socmed + data_channel_is_tech + 
        data_channel_is_world + self_reference_min_shares + self_reference_avg_sharess + 
        weekday_is_monday + weekday_is_tuesday + weekday_is_wednesday + 
        weekday_is_thursday + weekday_is_friday + global_rate_positive_words + 
        global_rate_negative_words + avg_positive_polarity + min_positive_polarity + 
        avg_negative_polarity + max_negative_polarity + title_subjectivity + 
        title_sentiment_polarity + abs_title_sentiment_polarity + 
        self_reference_min_shares:self_reference_avg_sharess + num_self_hrefs:num_imgs, 
        data = online_news)


    Call:
    lm(formula = log(shares) ~ n_tokens_title + num_hrefs + num_self_hrefs + 
        num_imgs + num_videos + average_token_length + num_keywords + 
        data_channel_is_lifestyle + data_channel_is_entertainment + 
        data_channel_is_bus + data_channel_is_socmed + data_channel_is_tech + 
        data_channel_is_world + self_reference_min_shares + self_reference_avg_sharess + 
        weekday_is_monday + weekday_is_tuesday + weekday_is_wednesday + 
        weekday_is_thursday + weekday_is_friday + global_rate_positive_words + 
        global_rate_negative_words + avg_positive_polarity + min_positive_polarity + 
        avg_negative_polarity + max_negative_polarity + title_subjectivity + 
        title_sentiment_polarity + abs_title_sentiment_polarity + 
        self_reference_min_shares:self_reference_avg_sharess + num_self_hrefs:num_imgs, 
        data = online_news)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -7.6725 -0.5606 -0.1768  0.4092  5.9380 

    Coefficients:
                                                           Estimate Std. Error
    (Intercept)                                           7.873e+00  4.150e-02
    n_tokens_title                                        3.508e-03  2.151e-03
    num_hrefs                                             6.583e-03  4.813e-04
    num_self_hrefs                                        7.302e-04  1.636e-03
    num_imgs                                              9.320e-03  7.669e-04
    num_videos                                            3.281e-03  1.159e-03
    average_token_length                                 -7.335e-02  7.218e-03
    num_keywords                                          1.504e-02  2.451e-03
    data_channel_is_lifestyle                            -1.654e-01  2.304e-02
    data_channel_is_entertainment                        -4.105e-01  1.608e-02
    data_channel_is_bus                                  -2.294e-01  1.758e-02
    data_channel_is_socmed                                6.817e-02  2.259e-02
    data_channel_is_tech                                 -9.313e-02  1.670e-02
    data_channel_is_world                                -4.560e-01  1.676e-02
    self_reference_min_shares                             9.759e-06  6.053e-07
    self_reference_avg_sharess                            2.327e-06  3.218e-07
    weekday_is_monday                                    -2.418e-01  1.647e-02
    weekday_is_tuesday                                   -2.993e-01  1.610e-02
    weekday_is_wednesday                                 -3.016e-01  1.610e-02
    weekday_is_thursday                                  -2.896e-01  1.615e-02
    weekday_is_friday                                    -2.283e-01  1.703e-02
    global_rate_positive_words                            4.035e-01  3.093e-01
    global_rate_negative_words                           -5.720e-01  4.925e-01
    avg_positive_polarity                                 1.325e-01  6.138e-02
    min_positive_polarity                                -2.474e-01  7.851e-02
    avg_negative_polarity                                -2.755e-01  5.252e-02
    max_negative_polarity                                 9.923e-02  6.493e-02
    title_subjectivity                                    4.193e-02  1.988e-02
    title_sentiment_polarity                              7.090e-02  1.900e-02
    abs_title_sentiment_polarity                          2.621e-02  3.031e-02
    self_reference_min_shares:self_reference_avg_sharess -1.749e-11  8.701e-13
    num_self_hrefs:num_imgs                              -9.212e-04  8.907e-05
                                                         t value Pr(>|t|)    
    (Intercept)                                          189.713  < 2e-16 ***
    n_tokens_title                                         1.631 0.102880    
    num_hrefs                                             13.677  < 2e-16 ***
    num_self_hrefs                                         0.446 0.655375    
    num_imgs                                              12.152  < 2e-16 ***
    num_videos                                             2.829 0.004665 ** 
    average_token_length                                 -10.161  < 2e-16 ***
    num_keywords                                           6.137 8.50e-10 ***
    data_channel_is_lifestyle                             -7.179 7.14e-13 ***
    data_channel_is_entertainment                        -25.526  < 2e-16 ***
    data_channel_is_bus                                  -13.049  < 2e-16 ***
    data_channel_is_socmed                                 3.018 0.002550 ** 
    data_channel_is_tech                                  -5.575 2.49e-08 ***
    data_channel_is_world                                -27.202  < 2e-16 ***
    self_reference_min_shares                             16.121  < 2e-16 ***
    self_reference_avg_sharess                             7.231 4.89e-13 ***
    weekday_is_monday                                    -14.686  < 2e-16 ***
    weekday_is_tuesday                                   -18.590  < 2e-16 ***
    weekday_is_wednesday                                 -18.733  < 2e-16 ***
    weekday_is_thursday                                  -17.930  < 2e-16 ***
    weekday_is_friday                                    -13.406  < 2e-16 ***
    global_rate_positive_words                             1.304 0.192141    
    global_rate_negative_words                            -1.161 0.245451    
    avg_positive_polarity                                  2.158 0.030904 *  
    min_positive_polarity                                 -3.151 0.001629 ** 
    avg_negative_polarity                                 -5.247 1.56e-07 ***
    max_negative_polarity                                  1.528 0.126452    
    title_subjectivity                                     2.110 0.034895 *  
    title_sentiment_polarity                               3.731 0.000191 ***
    abs_title_sentiment_polarity                           0.865 0.387240    
    self_reference_min_shares:self_reference_avg_sharess -20.103  < 2e-16 ***
    num_self_hrefs:num_imgs                              -10.342  < 2e-16 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.8837 on 39612 degrees of freedom
    Multiple R-squared:  0.09867,   Adjusted R-squared:  0.09796 
    F-statistic: 139.9 on 31 and 39612 DF,  p-value: < 2.2e-16

First approach: asses the “best” model
======================================

In order to asses the “best” model, we will report the confusion matrix,
overall error rate, true positive rate, and false positive rate for the
best model here. To get more accurate result, we will average these
quantities across multiple train/test splits. And we will use three
different method to get the average results in this model and choose 1
method to apply to all other models.

-   method 1: collect all predictions and corresponding “viral” value
    from all test set and put them in one confusion matrix, calculate
    overall error rate, true positive rate, and false positive rate base
    on this confusion matrix. Here is the confusion matrix:

<!-- -->

           0      1
    0 135401 265396
    1  57326 334777

overall error rate:

    [1] 0.4070148

true positive rate:

    [1] 0.8537986

false positive rate:

    [1] 0.6621706

-   method 2: Calculate the overall error rate, true positive rate, and
    false positive rate for each of the confusion matrix (for different
    test set), and average these quantities

overall error rate:

    [1] 0.4069441

true positive rate:

    [1] 0.8537392

false positive rate:

    [1] 0.6612914

-   method 3: For each of the confusion matrix (for different test set),
    we have four elements and denote them as \[1,1\], \[1,2\], \[2,1\],
    \[2,2\]. We average these four elements respectively across mulitple
    test sets and build an average confusion matrix, calculate overall
    error rate, true positive rate, and false positive rate base on this
    confusion matrix.

average confusion matrix:

            0       1
    0 1354.01 2653.96
    1  573.26 3347.77

overall error rate:

    [1] 0.4070148

true positive rate:

    [1] 0.8537986

false positive rate:

    [1] 0.6621706

First approach: build the baseline model
========================================

We build the baseline model with log(shares) versus all variables except
url


    Call:
    lm(formula = log(shares) ~ . - url - viral, data = online_news)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -7.6606 -0.5657 -0.1781  0.4112  6.0146 

    Coefficients: (2 not defined because of singularities)
                                    Estimate Std. Error t value Pr(>|t|)    
    (Intercept)                    7.897e+00  4.360e-02 181.133  < 2e-16 ***
    n_tokens_title                 3.314e-03  2.167e-03   1.529  0.12628    
    n_tokens_content               1.352e-05  1.394e-05   0.970  0.33209    
    num_hrefs                      6.175e-03  4.990e-04  12.374  < 2e-16 ***
    num_self_hrefs                -1.074e-02  1.363e-03  -7.880 3.36e-15 ***
    num_imgs                       4.508e-03  6.343e-04   7.108 1.20e-12 ***
    num_videos                     3.243e-03  1.176e-03   2.757  0.00583 ** 
    average_token_length          -6.305e-02  7.276e-03  -8.667  < 2e-16 ***
    num_keywords                   1.658e-02  2.464e-03   6.730 1.72e-11 ***
    data_channel_is_lifestyle     -1.874e-01  2.345e-02  -7.993 1.35e-15 ***
    data_channel_is_entertainment -4.419e-01  1.642e-02 -26.916  < 2e-16 ***
    data_channel_is_bus           -2.683e-01  1.808e-02 -14.840  < 2e-16 ***
    data_channel_is_socmed         5.294e-02  2.299e-02   2.303  0.02129 *  
    data_channel_is_tech          -1.220e-01  1.723e-02  -7.081 1.45e-12 ***
    data_channel_is_world         -4.990e-01  1.742e-02 -28.647  < 2e-16 ***
    self_reference_min_shares      7.542e-07  5.814e-07   1.297  0.19455    
    self_reference_max_shares      2.516e-07  3.151e-07   0.798  0.42459    
    self_reference_avg_sharess     1.812e-06  8.065e-07   2.247  0.02462 *  
    weekday_is_monday             -2.349e-01  2.030e-02 -11.572  < 2e-16 ***
    weekday_is_tuesday            -2.930e-01  2.000e-02 -14.651  < 2e-16 ***
    weekday_is_wednesday          -2.948e-01  1.999e-02 -14.742  < 2e-16 ***
    weekday_is_thursday           -2.847e-01  2.004e-02 -14.205  < 2e-16 ***
    weekday_is_friday             -2.214e-01  2.076e-02 -10.664  < 2e-16 ***
    weekday_is_saturday            1.004e-02  2.477e-02   0.405  0.68522    
    weekday_is_sunday                     NA         NA      NA       NA    
    is_weekend                            NA         NA      NA       NA    
    global_rate_positive_words     2.474e-01  3.218e-01   0.769  0.44192    
    global_rate_negative_words    -6.403e-01  5.076e-01  -1.262  0.20713    
    avg_positive_polarity          2.259e-01  8.173e-02   2.764  0.00572 ** 
    min_positive_polarity         -2.753e-01  8.532e-02  -3.226  0.00125 ** 
    max_positive_polarity         -2.889e-02  3.262e-02  -0.885  0.37592    
    avg_negative_polarity         -3.613e-01  9.022e-02  -4.005 6.21e-05 ***
    min_negative_polarity          2.407e-02  3.507e-02   0.686  0.49259    
    max_negative_polarity          1.364e-01  7.889e-02   1.729  0.08387 .  
    title_subjectivity             3.950e-02  2.001e-02   1.974  0.04835 *  
    title_sentiment_polarity       6.976e-02  1.915e-02   3.643  0.00027 ***
    abs_title_sentiment_polarity   3.276e-02  3.052e-02   1.073  0.28321    
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.8895 on 39609 degrees of freedom
    Multiple R-squared:  0.08689,   Adjusted R-squared:  0.0861 
    F-statistic: 110.9 on 34 and 39609 DF,  p-value: < 2.2e-16

First approach: asses the baseline model
========================================

In order to asses the baseline model, we will report the confusion
matrix, overall error rate, true positive rate, and false positive rate
for the best model here. To get more accurate result, we will average
these quantities across multiple train/test splits. And we will use the
method 1 metioned above, which is collecting all predictions and
corresponding “viral” value from all test set and put them in one
confusion matrix, calculating overall error rate, true positive rate,
and false positive rate base on this confusion matrix. Here is the
oncusion matrix:

           0      1
    0 130495 271024
    1  56159 335222

overall error rate:

    [1] 0.4126409

true positive rate:

    [1] 0.8565107

false positive rate:

    [1] 0.6749967

First approach: summary
=======================

According to the overall error rate, true positive rate and false
positive rate we report (we only compares the result for method 1), the
best model we build have lower overall error rate and false positive
rate. As we only choose steps = 2 when using stepwise selection, so the
true positive rate seems not improve a lot, and it’s about the same for
two models. But in general, the best model we build performs better. As
we only choose steps = 2, I believe using stepwise selection and choose
higher/appropriate steps will help us build a better model.

Second approach: build the best model
=====================================

-   First, try on a model with viral versus all variables except url and
    shares

<!-- -->


    Call:
    lm(formula = viral ~ . - url - shares, data = online_train)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -1.6552 -0.4415 -0.2435  0.4564  0.8123 

    Coefficients: (2 not defined because of singularities)
                                    Estimate Std. Error t value Pr(>|t|)    
    (Intercept)                    7.320e-01  2.620e-02  27.939  < 2e-16 ***
    n_tokens_title                -2.887e-03  1.302e-03  -2.218 0.026595 *  
    n_tokens_content               2.819e-05  8.375e-06   3.367 0.000762 ***
    num_hrefs                      2.616e-03  2.962e-04   8.834  < 2e-16 ***
    num_self_hrefs                -6.317e-03  8.078e-04  -7.820 5.44e-15 ***
    num_imgs                       1.593e-03  3.807e-04   4.185 2.86e-05 ***
    num_videos                     9.498e-04  7.055e-04   1.346 0.178212    
    average_token_length          -2.901e-02  4.365e-03  -6.647 3.04e-11 ***
    num_keywords                   9.230e-03  1.476e-03   6.253 4.07e-10 ***
    data_channel_is_lifestyle     -4.625e-02  1.413e-02  -3.272 0.001068 ** 
    data_channel_is_entertainment -1.996e-01  9.823e-03 -20.316  < 2e-16 ***
    data_channel_is_bus           -6.414e-02  1.083e-02  -5.920 3.25e-09 ***
    data_channel_is_socmed         1.424e-01  1.383e-02  10.295  < 2e-16 ***
    data_channel_is_tech           2.613e-02  1.031e-02   2.536 0.011231 *  
    data_channel_is_world         -2.175e-01  1.042e-02 -20.881  < 2e-16 ***
    self_reference_min_shares      2.699e-07  3.543e-07   0.762 0.446267    
    self_reference_max_shares      6.966e-08  2.001e-07   0.348 0.727751    
    self_reference_avg_sharess     7.833e-07  4.981e-07   1.573 0.115828    
    weekday_is_monday             -1.572e-01  1.219e-02 -12.892  < 2e-16 ***
    weekday_is_tuesday            -1.881e-01  1.201e-02 -15.656  < 2e-16 ***
    weekday_is_wednesday          -1.829e-01  1.202e-02 -15.212  < 2e-16 ***
    weekday_is_thursday           -1.715e-01  1.205e-02 -14.233  < 2e-16 ***
    weekday_is_friday             -1.320e-01  1.246e-02 -10.587  < 2e-16 ***
    weekday_is_saturday            4.715e-02  1.492e-02   3.161 0.001575 ** 
    weekday_is_sunday                     NA         NA      NA       NA    
    is_weekend                            NA         NA      NA       NA    
    global_rate_positive_words     1.850e-01  1.929e-01   0.959 0.337448    
    global_rate_negative_words    -3.302e-01  3.047e-01  -1.084 0.278506    
    avg_positive_polarity          6.685e-02  4.889e-02   1.367 0.171519    
    min_positive_polarity         -1.311e-01  5.119e-02  -2.562 0.010412 *  
    max_positive_polarity          7.035e-03  1.952e-02   0.360 0.718491    
    avg_negative_polarity         -1.455e-01  5.406e-02  -2.692 0.007115 ** 
    min_negative_polarity          3.077e-02  2.101e-02   1.464 0.143131    
    max_negative_polarity          6.819e-02  4.736e-02   1.440 0.149914    
    title_subjectivity             2.222e-02  1.197e-02   1.856 0.063452 .  
    title_sentiment_polarity       4.800e-02  1.146e-02   4.187 2.83e-05 ***
    abs_title_sentiment_polarity   9.768e-03  1.826e-02   0.535 0.592724    
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.477 on 31680 degrees of freedom
    Multiple R-squared:  0.09076,   Adjusted R-squared:  0.08978 
    F-statistic:    93 on 34 and 31680 DF,  p-value: < 2.2e-16

-   Second, drop things that seem (nearly) perfectly collinear with
    other variables

<!-- -->


    Call:
    lm(formula = viral ~ . - url - self_reference_min_shares - self_reference_max_shares - 
        weekday_is_sunday - is_weekend - avg_positive_polarity - 
        max_positive_polarity - min_negative_polarity - max_negative_polarity - 
        abs_title_sentiment_polarity - shares, data = online_train)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -1.6042 -0.4416 -0.2445  0.4569  0.8124 

    Coefficients:
                                    Estimate Std. Error t value Pr(>|t|)    
    (Intercept)                    7.343e-01  2.617e-02  28.054  < 2e-16 ***
    n_tokens_title                -2.919e-03  1.301e-03  -2.243  0.02489 *  
    n_tokens_content               2.851e-05  7.242e-06   3.938 8.24e-05 ***
    num_hrefs                      2.666e-03  2.945e-04   9.053  < 2e-16 ***
    num_self_hrefs                -6.338e-03  7.897e-04  -8.025 1.05e-15 ***
    num_imgs                       1.639e-03  3.797e-04   4.317 1.59e-05 ***
    num_videos                     1.090e-03  7.013e-04   1.555  0.11999    
    average_token_length          -2.598e-02  4.053e-03  -6.410 1.48e-10 ***
    num_keywords                   9.305e-03  1.475e-03   6.308 2.86e-10 ***
    data_channel_is_lifestyle     -4.608e-02  1.413e-02  -3.262  0.00111 ** 
    data_channel_is_entertainment -1.994e-01  9.814e-03 -20.316  < 2e-16 ***
    data_channel_is_bus           -6.559e-02  1.082e-02  -6.065 1.34e-09 ***
    data_channel_is_socmed         1.408e-01  1.382e-02  10.194  < 2e-16 ***
    data_channel_is_tech           2.485e-02  1.029e-02   2.415  0.01576 *  
    data_channel_is_world         -2.207e-01  1.032e-02 -21.384  < 2e-16 ***
    self_reference_avg_sharess     1.060e-06  1.139e-07   9.307  < 2e-16 ***
    weekday_is_monday             -1.571e-01  1.218e-02 -12.897  < 2e-16 ***
    weekday_is_tuesday            -1.884e-01  1.201e-02 -15.687  < 2e-16 ***
    weekday_is_wednesday          -1.830e-01  1.202e-02 -15.227  < 2e-16 ***
    weekday_is_thursday           -1.719e-01  1.205e-02 -14.264  < 2e-16 ***
    weekday_is_friday             -1.321e-01  1.246e-02 -10.603  < 2e-16 ***
    weekday_is_saturday            4.729e-02  1.491e-02   3.171  0.00152 ** 
    global_rate_positive_words     3.047e-01  1.802e-01   1.691  0.09083 .  
    global_rate_negative_words    -3.233e-01  2.785e-01  -1.161  0.24562    
    min_positive_polarity         -8.697e-02  4.210e-02  -2.066  0.03886 *  
    avg_negative_polarity         -7.046e-02  2.395e-02  -2.942  0.00327 ** 
    title_subjectivity             2.714e-02  8.721e-03   3.112  0.00186 ** 
    title_sentiment_polarity       5.081e-02  1.065e-02   4.773 1.83e-06 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.477 on 31687 degrees of freedom
    Multiple R-squared:  0.09051,   Adjusted R-squared:  0.08973 
    F-statistic: 116.8 on 27 and 31687 DF,  p-value: < 2.2e-16

-   Third, use stepwise selection to build a “best” model In order to
    save the running time of the program，we choose steps = 2 here, to
    improve just a little bit from the baseline model

<!-- -->

    lm(formula = viral ~ n_tokens_title + n_tokens_content + num_hrefs + 
        num_self_hrefs + num_imgs + num_videos + average_token_length + 
        num_keywords + data_channel_is_lifestyle + data_channel_is_entertainment + 
        data_channel_is_bus + data_channel_is_socmed + data_channel_is_tech + 
        data_channel_is_world + self_reference_avg_sharess + weekday_is_monday + 
        weekday_is_tuesday + weekday_is_wednesday + weekday_is_thursday + 
        weekday_is_friday + weekday_is_saturday + global_rate_positive_words + 
        global_rate_negative_words + min_positive_polarity + avg_negative_polarity + 
        title_subjectivity + title_sentiment_polarity + n_tokens_content:data_channel_is_bus + 
        n_tokens_content:num_keywords, data = online_train)


    Call:
    lm(formula = viral ~ n_tokens_title + n_tokens_content + num_hrefs + 
        num_self_hrefs + num_imgs + num_videos + average_token_length + 
        num_keywords + data_channel_is_lifestyle + data_channel_is_entertainment + 
        data_channel_is_bus + data_channel_is_socmed + data_channel_is_tech + 
        data_channel_is_world + self_reference_avg_sharess + weekday_is_monday + 
        weekday_is_tuesday + weekday_is_wednesday + weekday_is_thursday + 
        weekday_is_friday + weekday_is_saturday + global_rate_positive_words + 
        global_rate_negative_words + min_positive_polarity + avg_negative_polarity + 
        title_subjectivity + title_sentiment_polarity + n_tokens_content:data_channel_is_bus + 
        n_tokens_content:num_keywords, data = online_train)

    Residuals:
        Min      1Q  Median      3Q     Max 
    -1.5911 -0.4243 -0.2507  0.4532  0.8901 

    Coefficients:
                                           Estimate Std. Error t value
    (Intercept)                           8.171e-01  2.872e-02  28.446
    n_tokens_title                       -2.580e-03  1.298e-03  -1.987
    n_tokens_content                     -1.634e-04  2.419e-05  -6.755
    num_hrefs                             2.545e-03  2.942e-04   8.650
    num_self_hrefs                       -5.843e-03  7.902e-04  -7.395
    num_imgs                              2.336e-03  3.828e-04   6.102
    num_videos                            1.616e-03  7.015e-04   2.303
    average_token_length                 -2.401e-02  4.049e-03  -5.930
    num_keywords                         -3.320e-03  2.226e-03  -1.492
    data_channel_is_lifestyle            -3.516e-02  1.413e-02  -2.488
    data_channel_is_entertainment        -1.882e-01  9.832e-03 -19.139
    data_channel_is_bus                  -1.597e-01  1.372e-02 -11.646
    data_channel_is_socmed                1.493e-01  1.380e-02  10.815
    data_channel_is_tech                  3.152e-02  1.028e-02   3.065
    data_channel_is_world                -2.100e-01  1.033e-02 -20.322
    self_reference_avg_sharess            1.061e-06  1.136e-07   9.338
    weekday_is_monday                    -1.542e-01  1.216e-02 -12.680
    weekday_is_tuesday                   -1.835e-01  1.199e-02 -15.302
    weekday_is_wednesday                 -1.783e-01  1.200e-02 -14.857
    weekday_is_thursday                  -1.671e-01  1.203e-02 -13.885
    weekday_is_friday                    -1.277e-01  1.244e-02 -10.264
    weekday_is_saturday                   5.005e-02  1.488e-02   3.364
    global_rate_positive_words            2.859e-01  1.798e-01   1.590
    global_rate_negative_words           -2.475e-01  2.778e-01  -0.891
    min_positive_polarity                -8.884e-02  4.201e-02  -2.115
    avg_negative_polarity                -6.707e-02  2.390e-02  -2.807
    title_subjectivity                    2.766e-02  8.700e-03   3.179
    title_sentiment_polarity              5.094e-02  1.062e-02   4.797
    n_tokens_content:data_channel_is_bus  1.881e-04  1.700e-05  11.066
    n_tokens_content:num_keywords         2.129e-05  2.973e-06   7.160
                                         Pr(>|t|)    
    (Intercept)                           < 2e-16 ***
    n_tokens_title                       0.046921 *  
    n_tokens_content                     1.46e-11 ***
    num_hrefs                             < 2e-16 ***
    num_self_hrefs                       1.45e-13 ***
    num_imgs                             1.06e-09 ***
    num_videos                           0.021266 *  
    average_token_length                 3.06e-09 ***
    num_keywords                         0.135819    
    data_channel_is_lifestyle            0.012846 *  
    data_channel_is_entertainment         < 2e-16 ***
    data_channel_is_bus                   < 2e-16 ***
    data_channel_is_socmed                < 2e-16 ***
    data_channel_is_tech                 0.002181 ** 
    data_channel_is_world                 < 2e-16 ***
    self_reference_avg_sharess            < 2e-16 ***
    weekday_is_monday                     < 2e-16 ***
    weekday_is_tuesday                    < 2e-16 ***
    weekday_is_wednesday                  < 2e-16 ***
    weekday_is_thursday                   < 2e-16 ***
    weekday_is_friday                     < 2e-16 ***
    weekday_is_saturday                  0.000769 ***
    global_rate_positive_words           0.111903    
    global_rate_negative_words           0.372969    
    min_positive_polarity                0.034463 *  
    avg_negative_polarity                0.005010 ** 
    title_subjectivity                   0.001478 ** 
    title_sentiment_polarity             1.62e-06 ***
    n_tokens_content:data_channel_is_bus  < 2e-16 ***
    n_tokens_content:num_keywords        8.25e-13 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

    Residual standard error: 0.4758 on 31685 degrees of freedom
    Multiple R-squared:  0.09501,   Adjusted R-squared:  0.09418 
    F-statistic: 114.7 on 29 and 31685 DF,  p-value: < 2.2e-16

Second approach: asses the “best” model
=======================================

In order to asses the “best” model, we will report the confusion matrix,
overall error rate, true positive rate, and false positive rate for the
best model here. To get more accurate result, we will average these
quantities across multiple train/test splits. And we will use the method
1 metioned above, which is collecting all predictions and corresponding
“viral” value from all test set and put them in one confusion matrix,
calculating overall error rate, true positive rate, and false positive
rate base on this confusion matrix. Here is the confusion matrix:

           0      1
    0 249589 152311
    1 138864 252136

overall error rate:

    [1] 0.3672279

true positive rate:

    [1] 0.6448491

false positive rate:

    [1] 0.3789774

Summary: which one to choose?
=============================

First of all, we should declare that we use exactly the same way to
build the best model in both “regress first and threshold second” and
“threshold first and regress second” and use the exactly same way to
create the confusion matrix and calculate true positive rate, false
positive rate and overall error rate. Therefore, the quantities are
comparable.

Second, about which approach is better, “regress first and threshold
second” or “threshold first and regress second”. I would like to see
it’s hard to have an exact conclusion. It will depend on the purpose of
your model to choose one of these two approaches.

“Regress first and threshold second” has higher true positive rate, so
if our only purpose is to find out as much “viral” articles as we can,
and don’t care about errors model makes when predicting “not viral”
articles, then it would be better for us to choose approach 1.

However, “Threshold first and regress second” has lower overall error
rate and false positive rate, so it makes less mistakes in total. For
people who are more conservative and trying to make less mistakes when
predicting, the second approach will be better.

I would choose second approach in order to control the error rate of the
model to a lower level.
