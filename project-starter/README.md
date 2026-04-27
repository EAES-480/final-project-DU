Project title  
Soil Carbon Variation Across Soil Types and Latitude in a Coastal Temperate Rainforest  

by Wen Du  

---

## Summary

This project examines how soil organic carbon varies across soil types and geographic gradients using a coastal temperate rainforest dataset from McNicol et al. (2019). The analysis focuses on three main questions: whether soil carbon differs across soil orders, whether there is a relationship between latitude and soil carbon, and whether this relationship varies among soil types. In addition, based on instructor feedback, the contribution of organic and mineral horizons within mineral soils is also evaluated.

To ensure independence of observations, the dataset was filtered to include only pedon-level measurements (`pedon_start == TRUE`). Soil classification was based on soil order rather than the presence of organic horizons, which avoids misclassifying mineral soils that commonly contain organic surface layers. Soil orders were grouped into six categories: Organic soils, Podzol, Brunisol, Gleysol, Regosol, and Other.

Differences in soil carbon across soil groups were examined using visualization and analysis of variance (ANOVA). The boxplot shows clear variation among groups, with Organic soils generally having higher carbon values. The ANOVA results confirm that these differences are statistically significant (p < 2e-16), indicating that soil type is an important factor controlling carbon storage.

The relationship between latitude and soil carbon was analyzed using linear regression. A statistically significant but weak positive relationship was found between latitude and soil carbon (p ≈ 2e-09, R² ≈ 0.03), suggesting that while carbon tends to increase slightly with latitude, latitude alone is not a strong predictor. This pattern may reflect slower decomposition rates at higher latitudes within this temperate rainforest region.

To improve the model, soil group was added as an additional predictor. The multiple regression model shows a clear increase in explanatory power (R² ≈ 0.18), suggesting that soil type accounts for much more variation than latitude alone. This supports the earlier ANOVA results.

An interaction term between latitude and soil group was also included. The results indicate that the relationship between latitude and carbon is not consistent across all soil types. A significant interaction was observed for Regosol (p ≈ 0.017), while most other groups did not show significant interactions. This suggests that geographic patterns vary across soil types but are not uniform.

Finally, carbon contributions from organic and mineral horizons within mineral soils were examined. After excluding organic soil orders, carbon was summarized by horizon type. The results show that about 63% of carbon is stored in mineral horizons and 37% in organic horizons. This indicates that organic horizons still contribute substantially to carbon storage, even in soils classified as mineral.

Overall, the results suggest that soil type is the primary driver of soil carbon variation, while latitude plays a secondary and relatively weak role. The interaction analysis shows that spatial trends differ among soil types, and the horizon analysis highlights the importance of internal soil structure. Together, these findings provide a clearer understanding of soil carbon patterns in this region.

---

## Presentation

The presentation slides can be found here.

---

## Data

McNicol, G. et al. (2019). Coastal temperate rainforest soil carbon [used the 2024 version]  
Retrieved April 2026. Link: https://datadryad.org/dataset/doi:10.5061/dryad.5jf6j1r 

---

## Reference
McNicol, G., Bulmer, C., D’Amore, D., Sanborn, P., Saunders, S., Giesbrecht, I., Gonzalez Arriola, S., Bidlack, A., Butman, D., & Buma, B. (2019). Large, climate-sensitive soil carbon stocks mapped with pedology-informed machine learning in the North Pacific coastal temperate rainforest. Environmental Research Letters, 14, 014004. https://doi.org/10.1088/1748-9326/aaed52 