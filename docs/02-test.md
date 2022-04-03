

```r
renv::activate(profile = "02-test")
```


# A test for renv profiles


```r
packageVersion("dplyr")
#> [1] '0.8.5'
library(dplyr)
#> 
#> Attaching package: 'dplyr'
#> The following objects are masked from 'package:stats':
#> 
#>     filter, lag
#> The following objects are masked from 'package:base':
#> 
#>     intersect, setdiff, setequal, union
sessionInfo()
#> R version 4.1.3 (2022-03-10)
#> Platform: x86_64-apple-darwin17.0 (64-bit)
#> Running under: macOS Big Sur/Monterey 10.16
#> 
#> Matrix products: default
#> BLAS:   /Library/Frameworks/R.framework/Versions/4.1/Resources/lib/libRblas.0.dylib
#> LAPACK: /Library/Frameworks/R.framework/Versions/4.1/Resources/lib/libRlapack.dylib
#> 
#> locale:
#> [1] en_AU.UTF-8/en_AU.UTF-8/en_AU.UTF-8/C/en_AU.UTF-8/en_AU.UTF-8
#> 
#> attached base packages:
#> [1] stats     graphics  grDevices datasets  utils    
#> [6] methods   base     
#> 
#> other attached packages:
#> [1] dplyr_0.8.5
#> 
#> loaded via a namespace (and not attached):
#>  [1] Rcpp_1.0.8.3     bslib_0.3.1      compiler_4.1.3  
#>  [4] pillar_1.7.0     jquerylib_0.1.4  tools_4.1.3     
#>  [7] digest_0.6.29    downlit_0.4.0    jsonlite_1.8.0  
#> [10] evaluate_0.15    memoise_2.0.1    tibble_3.1.6    
#> [13] lifecycle_1.0.1  pkgconfig_2.0.3  rlang_1.0.2     
#> [16] cli_3.2.0        rstudioapi_0.13  yaml_2.3.5      
#> [19] xfun_0.30        fastmap_1.1.0    stringr_1.4.0   
#> [22] xml2_1.3.3       knitr_1.38       vctrs_0.4.0     
#> [25] fs_1.5.2         sass_0.4.1       tidyselect_1.1.2
#> [28] glue_1.6.2       R6_2.5.1         fansi_1.0.3     
#> [31] rmarkdown_2.13   bookdown_0.25    purrr_0.3.4     
#> [34] magrittr_2.0.3   htmltools_0.5.2  ellipsis_0.3.2  
#> [37] assertthat_0.2.1 renv_0.15.4      utf8_1.2.2      
#> [40] stringi_1.7.6    cachem_1.0.6     crayon_1.5.1
```
