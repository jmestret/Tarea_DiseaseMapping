# Tarea Disease Mapping Aragón

Este repositorio proporciona un ejemplo de "disease mapping", que introduce un análisis de datos espaciales en una red de localización fija mediante una regresión de Poisson con interceptos aleatorios definidos con una correlación espacial. Disponemos de un banco de datos con casos de mortalidad en hombres por enfermedad isquémica en la provincia de Aragón. Se realizará inferencia bayesiana utilizando métodos MCMC (WinBUGS) e INLA para ajustar un modelo Besag-York-Mollié (BYM).

**Para reproducir los resultados, sigue una de las siguientes opciones:**

- **Opción rápida:** Si deseas replicar los resultados utilizando los mismos datos y las simulaciones de MCMC que hemos generado, es esencial descargar el archivo binario de la *release v0.1.0*. Este archivo no solo incluye el código, sino también un archivo *.Rdata* con la salida de WinBUGS. Puedes encontrarlo aquí: <https://github.com/jmestret/Tarea_DiseaseMapping/releases/download/v0.1.0/Tarea_DiseaseMapping_JorgeSebas.zip>

- **Opción lenta:** Por otro lado, si prefieres ejecutar todo el código y volver a realizar las simulaciones de MCMC, clona este repositorio de GitHub y ejecuta el archivo *.Rmd*.
  
## Requisitos

Las siguientes dependencias son necesarias para ejecutar el código:

1. Instalar R-INLA v23.10.17 (más información el siguiente [enalce](https://www.r-inla.org/))

```
install.packages("INLA",repos=c(getOption("repos"),INLA="https://inla.r-inla-download.org/R/stable"), dep=TRUE)
```

2. Instalar otros paquetes de R

```
# Lista de paquetes
paquetes <- c("tidyverse", "sf", "spdep", "ggthemes", "DOYPAColors", "INLA", 
              "R2WinBUGS", "ggpubr", "pander", "coda")

# Instalar paquetes que no están instalados
nuevos_paquetes <- paquetes[!(paquetes %in% installed.packages()[,"Package"])]
if(length(nuevos_paquetes)) install.packages(nuevos_paquetes, dependencies = TRUE)
```

\*3. En caso de error al ejecutar WinBUGS: configurar WinBUGS

Si estás utilizando un ordenador con Linux, será necesario instalar [wine](https://www.winehq.org/). Además, si es necesario, puedes añadir la ruta al directorio que contiene el ejecutable de WinBUGS en caso de no encontrarse en el sitio por defecto (`bugs.directory`):

```
mod_bugs <- bugs(model = model_winbugs, data = data,
                 inits = init_values, parameters = params,
                 n.chains = 4,
                 n.iter = 50000,
                 n.burnin = 5000,
                 n.thin = 30,
                 bugs.directory = "path/to/WinBUGS")
```

## sessionInfo

```
> sessionInfo()

R version 4.3.2 (2023-10-31)
Platform: x86_64-pc-linux-gnu (64-bit)
Running under: Ubuntu 20.04.6 LTS

Matrix products: default
BLAS:   /usr/lib/x86_64-linux-gnu/blas/libblas.so.3.9.0 
LAPACK: /usr/lib/x86_64-linux-gnu/lapack/liblapack.so.3.9.0

locale:
 [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
 [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
 [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
[10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       

time zone: Europe/Madrid
tzcode source: system (glibc)

attached base packages:
[1] stats     graphics  grDevices utils     datasets  methods   base     

other attached packages:
 [1] pander_0.6.5      ggpubr_0.6.0      R2WinBUGS_2.1-21  boot_1.3-28       coda_0.19-4      
 [6] INLA_23.10.17     sp_2.1-1          Matrix_1.6-2      DOYPAColors_0.0.1 ggthemes_5.0.0   
[11] spdep_1.2-8       spData_2.3.0      sf_1.0-14         lubridate_1.9.3   forcats_1.0.0    
[16] stringr_1.5.0     dplyr_1.1.3       purrr_1.0.2       readr_2.1.4       tidyr_1.3.0      
[21] tibble_3.2.1      ggplot2_3.4.4     tidyverse_2.0.0  

loaded via a namespace (and not attached):
 [1] gtable_0.3.4       xfun_0.40          rstatix_0.7.2      lattice_0.22-5     tzdb_0.4.0        
 [6] vctrs_0.6.4        tools_4.3.2        generics_0.1.3     parallel_4.3.2     proxy_0.4-27      
[11] fansi_1.0.5        pkgconfig_2.0.3    KernSmooth_2.23-22 lifecycle_1.0.3    farver_2.1.1      
[16] compiler_4.3.2     deldir_1.0-9       fmesher_0.1.2      MatrixModels_0.5-3 munsell_0.5.0     
[21] carData_3.0-5      htmltools_0.5.6.1  class_7.3-22       yaml_2.3.7         car_3.1-2         
[26] pillar_1.9.0       rsconnect_1.1.1    classInt_0.4-10    wk_0.8.0           abind_1.4-5       
[31] tidyselect_1.2.0   digest_0.6.33      stringi_1.7.12     labeling_0.4.3     splines_4.3.2     
[36] cowplot_1.1.1      fastmap_1.1.1      grid_4.3.2         colorspace_2.1-0   cli_3.6.1         
[41] magrittr_2.0.3     utf8_1.2.3         broom_1.0.5        e1071_1.7-13       withr_2.5.1       
[46] backports_1.4.1    scales_1.2.1       timechange_0.2.0   rmarkdown_2.25     gridExtra_2.3     
[51] ggsignif_0.6.4     hms_1.1.3          evaluate_0.22      knitr_1.44         s2_1.1.4          
[56] rlang_1.1.1        Rcpp_1.0.11        glue_1.6.2         DBI_1.1.3          rstudioapi_0.15.0 
[61] R6_2.5.1           units_0.8-4
```

