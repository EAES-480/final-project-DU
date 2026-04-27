---
title: "Final project analysis"

output:

  pdf_document:

    latex_engine: xelatex

author: "Wen Du"

date: "2026-04-27"
---



``` r
library(tidyverse)
library(broom)
```

1. Load Data


``` r
df <- read_csv("../data/data_final_project.csv")
```

```
## Rows: 6868 Columns: 26
## -- Column specification --------------------------------------------------------
## Delimiter: ","
## chr  (6): source, pedon_id, order, latlon_q, horizon, horizon_type
## dbl (19): lat, lon, horizon_number, depth2, depth1, depth, bulk_density, bd_...
## lgl  (1): pedon_start
## 
## i Use `spec()` to retrieve the full column specification for this data.
## i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
glimpse(df)
```

```
## Rows: 6,868
## Columns: 26
## $ source         <chr> "SANBORN & MASSICOTTE 2010", "SANBORN & MASSICOTTE 2010~
## $ pedon_id       <chr> "BC09-04", "BC09-04", "BC09-04", "BC09-04", "BC09-04", ~
## $ order          <chr> "Gleyed Dystric Brunisol", "Gleyed Dystric Brunisol", "~
## $ lat            <dbl> 54.07572, 54.07572, 54.07572, 54.07572, 54.07572, 54.07~
## $ lon            <dbl> -131.6861, -131.6861, -131.6861, -131.6861, -131.6861, ~
## $ latlon_q       <chr> "HIGH", "HIGH", "HIGH", "HIGH", "HIGH", "HIGH", "HIGH",~
## $ horizon_number <dbl> 1, 2, 3, 4, 5, 6, 7, 1, 2, 3, 4, 5, 6, 7, 8, 1, 2, 3, 4~
## $ horizon        <chr> "Lv/S", "Fm", "Hh", "Aeg", "Bmg", "BC", "Cg", "Lv", "Fm~
## $ horizon_type   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,~
## $ depth2         <dbl> 7, 6, 2, 0, -3, -35, -60, 13, 12, 11, 0, -7, -25, -55, ~
## $ depth1         <dbl> 6, 2, 0, -3, -35, -60, -110, 12, 11, 0, -7, -25, -55, -~
## $ depth          <dbl> 1, 4, 2, 3, 32, 25, 50, 1, 1, 11, 7, 18, 30, 30, 50, 1,~
## $ bulk_density   <dbl> 0.16, 0.16, 0.16, 1.48, 1.59, 1.59, 1.47, 0.16, 0.16, 0~
## $ bd_method      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0~
## $ cf             <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0~
## $ cf_method      <dbl> 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2~
## $ cconc          <dbl> 53.95, 21.77, 41.50, 0.47, 0.21, 0.16, 0.18, 53.33, 50.~
## $ cconc_method   <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0~
## $ mineral_d      <dbl> 100, NA, NA, NA, NA, NA, NA, 100, NA, NA, NA, NA, NA, N~
## $ ff_d           <dbl> 7, NA, NA, NA, NA, NA, NA, 13, NA, NA, NA, NA, NA, NA, ~
## $ total_d        <dbl> 107, NA, NA, NA, NA, NA, NA, 113, NA, NA, NA, NA, NA, N~
## $ ccontent       <dbl> 863, 1393, 1328, 209, 1068, 636, 1323, 853, 812, 8779, ~
## $ total_c        <dbl> 6820, NA, NA, NA, NA, NA, NA, 13978, NA, NA, NA, NA, NA~
## $ ccontent_1m    <dbl> 863, 1393, 1328, 209, 1068, 636, 1058, 853, 812, 8779, ~
## $ total_c_1m     <dbl> 65.55, NA, NA, NA, NA, NA, NA, 138.16, NA, NA, NA, NA, ~
## $ pedon_start    <lgl> TRUE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, F~
```

2. Create Pedon-level Dataset

``` r
pedon_df <- df %>%
  filter(pedon_start == TRUE) %>%
  mutate(
    order_group = case_when(
      str_detect(order, regex("Histosol|Folisol|Mesisol|Humisol|Cryo", ignore_case = TRUE)) ~ "Organic soils",
      str_detect(order, regex("Podzol", ignore_case = TRUE)) ~ "Podzol",
      str_detect(order, regex("Brunisol|Brunisolic", ignore_case = TRUE)) ~ "Brunisol",
      str_detect(order, regex("Gleysol|Gleysolic", ignore_case = TRUE)) ~ "Gleysol",
      str_detect(order, regex("Regosol|Regosolic", ignore_case = TRUE)) ~ "Regosol",
      TRUE ~ "Other"
    )
  ) %>%
  select(pedon_id, order, order_group, total_c_1m, lat) %>%
  filter(!is.na(total_c_1m), !is.na(order_group))

# check structure
glimpse(pedon_df)
```

```
## Rows: 1,283
## Columns: 5
## $ pedon_id    <chr> "BC09-04", "BC09-01", "BC09-02", "BC09-03", "BC09-06", "BC~
## $ order       <chr> "Gleyed Dystric Brunisol", "Gleyed Dystric Brunisol", "Pla~
## $ order_group <chr> "Brunisol", "Brunisol", "Podzol", "Podzol", "Podzol", "Oth~
## $ total_c_1m  <dbl> 65.55, 138.16, 348.89, 336.43, 474.28, 0.00, 978.84, 869.9~
## $ lat         <dbl> 54.07572, 54.02631, 54.02253, 54.07275, 54.02200, 54.06908~
```

``` r
# check sample sizes
pedon_df %>% count(order)
```

```
## # A tibble: 56 x 2
##    order                               n
##    <chr>                           <int>
##  1 Brunisolic                        317
##  2 Duric Dystric Brunisol              2
##  3 Duric Ferro-Humic Podzol            7
##  4 Duric Humo-Ferric Podzol            3
##  5 Dystric Brunisol                    4
##  6 Ferro-Humic Podzol                 60
##  7 Folisol                            10
##  8 Folisol (but horizons like pod)     1
##  9 Gleyed Brunisolic Gray Luvisol      1
## 10 Gleyed Dystric Brunisol             2
## # i 46 more rows
```


3. Exploratory Analysis

Boxplot: Soil Carbon by Soil Order

``` r
ggplot(pedon_df, aes(x = order_group, y = total_c_1m, fill = order_group)) +
  geom_boxplot(outlier.shape = NA) +
  geom_jitter(width = 0.15, alpha = 0.1) +
  theme_classic() +
  labs(
    title = "Soil Carbon by Soil Group",
    x = "Soil Group",
    y = "Total Carbon (0-1 m)"
  )
```

![](Final-project-analysis_files/figure-latex/unnamed-chunk-3-1.pdf)<!-- --> 


4. ANOVA: Does Carbon Differ Across Soil Orders?


``` r
aov_model <- aov(total_c_1m ~ order_group, data = pedon_df)
summary(aov_model)
```

```
##               Df   Sum Sq Mean Sq F value Pr(>F)    
## order_group    5  6876360 1375272   52.64 <2e-16 ***
## Residuals   1277 33363385   26126                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Group Means


``` r
pedon_df %>%
  group_by(order) %>%
  summarise(
    mean_carbon = mean(total_c_1m, na.rm = TRUE),
    sd_carbon = sd(total_c_1m, na.rm = TRUE),
    n = n(),
    .groups = "drop"
  )
```

```
## # A tibble: 56 x 4
##    order                           mean_carbon sd_carbon     n
##    <chr>                                 <dbl>     <dbl> <int>
##  1 Brunisolic                             93.2      62.0   317
##  2 Duric Dystric Brunisol                161.       77.0     2
##  3 Duric Ferro-Humic Podzol              409.      259.      7
##  4 Duric Humo-Ferric Podzol              159.       56.1     3
##  5 Dystric Brunisol                      333.      177.      4
##  6 Ferro-Humic Podzol                    415.      271.     60
##  7 Folisol                               260.      258.     10
##  8 Folisol (but horizons like pod)       404.       NA       1
##  9 Gleyed Brunisolic Gray Luvisol        157.       NA       1
## 10 Gleyed Dystric Brunisol               102.       51.3     2
## # i 46 more rows
```

5. Regression: Carbon vs Latitude


``` r
pedon_df_lat <- pedon_df %>%
  filter(lat > 40, lat < 65)

lm_model <- lm(total_c_1m ~ lat, data = pedon_df_lat)
summary(lm_model)
```

```
## 
## Call:
## lm(formula = total_c_1m ~ lat, data = pedon_df_lat)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -275.19 -111.63  -46.66   61.64 1354.13 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -373.436     94.480  -3.953 8.16e-05 ***
## lat           10.952      1.819   6.021 2.26e-09 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 174.5 on 1278 degrees of freedom
## Multiple R-squared:  0.02759,	Adjusted R-squared:  0.02683 
## F-statistic: 36.26 on 1 and 1278 DF,  p-value: 2.258e-09
```

Plot Regression

``` r
# 5. Regression: Carbon vs Latitude + Soil Order (Improved Model)

# 先清理数据（过滤异常纬度）
pedon_df_mod <- pedon_df %>%
  filter(lat > 40, lat < 65)

# 建立多元回归模型（关键升级）
lm_model2 <- lm(total_c_1m ~ lat + order_group, data = pedon_df_mod)

# 查看结果
summary(lm_model2)
```

```
## 
## Call:
## lm(formula = total_c_1m ~ lat + order_group, data = pedon_df_mod)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -365.52  -97.31  -26.90   50.07 1296.03 
## 
## Coefficients:
##                          Estimate Std. Error t value Pr(>|t|)    
## (Intercept)              -132.357     90.468  -1.463 0.143708    
## lat                         4.494      1.759   2.554 0.010755 *  
## order_groupGleysol         78.749     19.939   3.949 8.26e-05 ***
## order_groupOrganic soils  286.895     24.925  11.510  < 2e-16 ***
## order_groupOther          132.262     20.879   6.335 3.29e-10 ***
## order_groupPodzol         132.915     11.054  12.024  < 2e-16 ***
## order_groupRegosol         59.831     16.994   3.521 0.000446 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 161.4 on 1273 degrees of freedom
## Multiple R-squared:  0.1713,	Adjusted R-squared:  0.1673 
## F-statistic: 43.84 on 6 and 1273 DF,  p-value: < 2.2e-16
```

``` r
# 更清晰的系数表（可用于报告）
tidy(lm_model2)
```

```
## # A tibble: 7 x 5
##   term                     estimate std.error statistic  p.value
##   <chr>                       <dbl>     <dbl>     <dbl>    <dbl>
## 1 (Intercept)               -132.       90.5      -1.46 1.44e- 1
## 2 lat                          4.49      1.76      2.55 1.08e- 2
## 3 order_groupGleysol          78.7      19.9       3.95 8.26e- 5
## 4 order_groupOrganic soils   287.       24.9      11.5  3.10e-29
## 5 order_groupOther           132.       20.9       6.33 3.29e-10
## 6 order_groupPodzol          133.       11.1      12.0  1.28e-31
## 7 order_groupRegosol          59.8      17.0       3.52 4.46e- 4
```

``` r
# 按 soil group 分开看趋势（强烈推荐）
ggplot(pedon_df_mod, aes(x = lat, y = total_c_1m, color = order_group)) +
  geom_point(alpha = 0.2, size = 1) +
  geom_smooth(method = "lm", se = FALSE) +
  facet_wrap(~ order_group) +
  theme_classic() +
  theme(legend.position = "none")+
  labs(subtitle = "Slopes vary across soil groups, indicating weak but heterogeneous relationships")
```

```
## `geom_smooth()` using formula = 'y ~ x'
```

![](Final-project-analysis_files/figure-latex/unnamed-chunk-7-1.pdf)<!-- --> 

``` r
lm_model3 <- lm(total_c_1m ~ lat * order_group, data = pedon_df_mod)

summary(lm_model3)
```

```
## 
## Call:
## lm(formula = total_c_1m ~ lat * order_group, data = pedon_df_mod)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -364.39  -93.44  -28.28   49.75 1307.42 
## 
## Coefficients:
##                              Estimate Std. Error t value Pr(>|t|)  
## (Intercept)                   -17.961    196.005  -0.092   0.9270  
## lat                             2.259      3.826   0.590   0.5551  
## order_groupGleysol            421.780    441.413   0.956   0.3395  
## order_groupOrganic soils      473.669    625.839   0.757   0.4493  
## order_groupOther             -676.508    496.139  -1.364   0.1730  
## order_groupPodzol            -175.371    229.329  -0.765   0.4446  
## order_groupRegosol            942.224    367.650   2.563   0.0105 *
## lat:order_groupGleysol         -6.557      8.504  -0.771   0.4408  
## lat:order_groupOrganic soils   -3.213     11.405  -0.282   0.7782  
## lat:order_groupOther           15.626      9.580   1.631   0.1031  
## lat:order_groupPodzol           5.966      4.458   1.338   0.1810  
## lat:order_groupRegosol        -17.025      7.119  -2.391   0.0169 *
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 160.6 on 1268 degrees of freedom
## Multiple R-squared:  0.1824,	Adjusted R-squared:  0.1753 
## F-statistic: 25.71 on 11 and 1268 DF,  p-value: < 2.2e-16
```


Horizon Contribution in Mineral Soils


``` r
mineral_horizon_summary <- df %>%
  mutate(
    order_group = case_when(
      str_detect(order, regex("Histosol|Folisol|Mesisol|Humisol|Cryo", ignore_case = TRUE)) ~ "Organic soils",
      str_detect(order, regex("Podzol", ignore_case = TRUE)) ~ "Podzol",
      str_detect(order, regex("Brunisol|Brunisolic", ignore_case = TRUE)) ~ "Brunisol",
      str_detect(order, regex("Gleysol|Gleysolic", ignore_case = TRUE)) ~ "Gleysol",
      str_detect(order, regex("Regosol|Regosolic", ignore_case = TRUE)) ~ "Regosol",
      TRUE ~ "Other"
    )
  ) %>%
  filter(order_group != "Organic soils") %>%
  filter(!is.na(horizon_type)) %>%
  group_by(horizon_type) %>%
  summarise(
    total_carbon = sum(ccontent_1m, na.rm = TRUE),
    mean_carbon = mean(ccontent_1m, na.rm = TRUE),
    n = n(),
    .groups = "drop"
  ) %>%
  mutate(prop_carbon = total_carbon / sum(total_carbon))

mineral_horizon_summary
```

```
## # A tibble: 2 x 5
##   horizon_type total_carbon mean_carbon     n prop_carbon
##   <chr>               <dbl>       <dbl> <int>       <dbl>
## 1 Mineral          10678404       3620.  3169       0.632
## 2 Organic           6206694       2647.  2452       0.368
```


7. Notes

* Analysis is conducted at the pedon level (independent observations)
* Soil order is used instead of horizon-based classification
* ANOVA tests group differences
* Regression explores geographic trends
