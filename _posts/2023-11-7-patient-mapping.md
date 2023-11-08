---
layout: distill
title: Patient Mapping
tags: maps, R, plotly, ggplot
date: 2023-11-7
featured: true

# NOTES:
#   - make sure that TOC names match the actual section names
toc:
  - name: Overview
  - name: Wrangling the Data
  - name: Mapping the Data
    subsections:
      - name: Plotting Different Types of Patients
  - name: Conclusion

---
## Overview

This post aims to illustrate the differences in the geographic
distribution of patients by county from a specific clinic in Northeast
Wisconsin. We will use R to both process the data and then create
visually appealing and highly-functional maps using the graphing library
Plotly.

------------------------------------------------------------------------

## Wrangling the Data

In order to create the maps that we want, the data has to be in a format
where we can extract the relevant information from it. The original
dataset is layed out below.

| Primary Diagnosis | All Diagnoses        | visit_number | department     | appt_date | visit_type | provider_type | gender |
|-------------------|----------------------|--------------|----------------|-----------|------------|---------------|--------|
| A100              | \[A100, B100, C100\] | 5000000      | Department One | 10/29/23  | follow_up  | physician     | male   |

| age  | city      | zip_code | county | distance | lag_days | patient_id   | provider_id |
|------|-----------|----------|--------|----------|----------|--------------|-------------|
| 4.25 | Green Bay | 54229    | Brown  | 10       | 100      | 100-100-1000 | 3           |

As we can see, there are many columns that are irrelevant to our task.
The two diagnoses columns, visit_number, department, appt_date,
visit_type, provider_type, gender, age, lag_days, and provider_id can
all be dropped. Next, we have to group the number of individual patients
by zip code.

Firstly, we have to only include individual patients, not individual
visits. We have to include what each patient’s zip code is, and then
find the number of patients by zip code. This can be accomplished in R
in one line by using the Tidyverse package.

`zip.pats = wrangle.pats %>% group_by(patient_id, zip_code) %>% summarise() %>%  group_by(zip_code) %>% summarise(n=n())`

The wrangle.pats dataset is the original dataset with the unneeded
columns dropped. We first want to group by patient id and their
corresponding zip code. Then we can find the sum of all the patients
that live in each zip code by grouping by zip code. The resulting
dataset is shown below (where `n` is the number of patients).

| zip_code |   n |
|:---------|----:|
| 48894    |   1 |
| 49801    |  10 |
| 49802    |   6 |
| 49807    |   2 |
| 49815    |   1 |
| 49821    |   1 |

Table with patients by Zip Code

Next, we have to get the proper county for each zip code. This can be
done using `reverse_zipcode()` function from the zipcodeR package. This
function takes a zip code and returns its state, county, and other
relevant information about it. After using this function across the
dataset, the proper county is now present in the dataset.

Finally, we have to include map data into out dataset so we can properly
define the counties in our map. (See next section for what this map data
entails). After a simple merge, we can see the final dataset below.

| county       | state | patients | subregion | region   |      long |      lat | group | order |
|:-------------|:------|---------:|:----------|:---------|----------:|---------:|------:|------:|
| Alger County | MI    |        1 | alger     | michigan | -86.63122 | 46.16321 |  1199 | 38526 |
| Alger County | MI    |        1 | alger     | michigan | -87.12396 | 46.16321 |  1199 | 38527 |
| Alger County | MI    |        1 | alger     | michigan | -87.12969 | 46.51271 |  1199 | 38528 |
| Alger County | MI    |        1 | alger     | michigan | -87.10677 | 46.51271 |  1199 | 38529 |
| Alger County | MI    |        1 | alger     | michigan | -87.06094 | 46.52417 |  1199 | 38530 |
| Alger County | MI    |        1 | alger     | michigan | -87.02083 | 46.53563 |  1199 | 38531 |

Full Dataset with Map and Patient Data

------------------------------------------------------------------------

## Mapping the Data

We can use the `map_data` function from ggplot to get the required
county data for creating our maps. Looking at where the patients come
from in the dataset, we can see that they either come from Wisconsin or
the Upper Penninsula of Michigan. (There is one patient that comes from
Alaska, but this is an outlier that we do not need to show in our map).
Now getting all of the counties from Wisconsin is pretty easy using
Tidyverse. But in order to get just the Michigan counties from the UP
(Upper Penninsula), we have to first specify which counties are located
in the UP. After this is done, we can then create a base map of just
Wisconsin and the UP with their respective county outlines. (All code
can be seen below in the appendix). This map can be seen below.

![](/assets/map_post_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

After our map of the required counties is created, we can then apply our
patient data to this map to gain some insight into where the patients
are coming from. This map can be seen below.

![](/assets/map_post_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

Something to note is that we used a rainbow color scheme in order to
more easily differentiate the numbers of patients from each county. A
log10 was also used in order to more clearly show the differences
between the number of patients coming from each county.

Finally, we can quite easily create an interactive plotly map using
single line of code.
`county.plotly = ggplotly(county.ggplot, tooltip="text")` We can see the
results of this below.


<div class="l-page">
  <iframe src="{{ '/assets/map_post_files/plotly/countyPlotly.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>

------------------------------------------------------------------------

### Plotting Different Types of Patients

The clinic sees two different types of patients which could be mapped
out separately. Using the same method outlined above, we can map the two
patient types separately by using datasets that only include the
specific type of patient we are looking for. Two plotly plots for both
types of patients can be seen below

<div class="l-page">
  <iframe src="{{ '/assets/map_post_files/plotly/sidebyside.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>

------------------------------------------------------------------------

## Conclusions

In conclusion, we were able to use R to transform the clinic data into
usable map data in order to map where the clinics patients are coming
from. In doing so, we were able to gain some valuable insights into how
far some patients were willing to travel, where other similar clinics
may be located, and where new possible clinics could spring up. In
regards to what future work could be done, more maps would be created
based on other factors that were not included in this post, such as
gender, age, etc… We could also look into doing some modeling on
distance from the clinic and whether patients are not coming regularly
based on the distance they have travel. These and other analyses can be
done in order to get more valuable insight into how to best improve the
wellbeing and care for the patients of this particular clinic.

------------------------------------------------------------------------

## Appendix

### Source Code

``` r
new.pats = read.csv("data/pat_data.csv")
knitr::opts_chunk$set(include = F)
library(tidyverse)
library(zipcodeR)
library(plotly)
library(htmlwidgets)
library(knitr)
wrangle.pats = subset(new.pats, select=-c(prim_diag, visit_number, department, appt_date, visit_type, provider_type, gender, age, lag_days, provider_id, new_patient)
)
wrangle.pats$zip_code = as.factor(wrangle.pats$zip_code)

#getting the data in a format that has the zip code and the number of patients per zip code
zip.pats = wrangle.pats %>% group_by(patient_id, zip_code) %>% summarise() %>%  group_by(zip_code) %>% summarise(n=n())

#does the same as above but includes patient type as well - NOT DONE YET
zip.type.pats = wrangle.pats %>% group_by(patient_id, zip_code, patient_type, .name_repair="minimal") %>% summarise(n=n())
#creating two datasets for patient type and their corresponding zip code
zip.one.pats = zip.type.pats %>% filter(patient_type=="one") %>% group_by(zip_code) %>% summarise(n=n())
zip.two.pats = zip.type.pats %>% filter(patient_type=="two") %>% group_by(zip_code) %>% summarise(n=n())

#using the county data to create a map of wisconsin and the UP
counties = map_data("county")

#TODO maybe put these counties in a config file
up.counties = c("alger", "baraga", "chippewa", "delta", "dickinson", "gogebic", "houghton", "iron", "keweenaw", "luce", "mackinac", "marquette", "menominee", "ontonagon", "schoolcraft"
)

middie = counties %>% filter(region == "wisconsin")
up = counties %>% filter(region == "michigan") %>% filter(subregion %in% up.counties)

middie = rbind(middie, up)

#base map of wisconsin and the up to be used to map the patient data on top of
region= ggplot(data=middie, aes(x = long, y = lat, group = group)) + geom_polygon() + coord_fixed(1.3)

#example plot of wisconsin and UP counties
wisc.up.map = ggplot(data=middie) + geom_polygon(aes(x = long, y = lat, group = group), fill = NA, color = "black") + coord_fixed(1.3)
#TODO will want a better solution for state codes than just hard coding the used ones in
state.codes = c("WI"="wisconsin", "MI"="michigan", "AK"="alaska")

#creating a function that inputs zip codes and the patients per zip code and then outputs a ggplot object that shows a map of the patients and where they are from
#full.data is a bool which also returns the full dataset
to.map = function(dataset, full.data=F){
  #creating a new dataset with all the zipcode information and then grouping the patients by the county in which they live
  county.data = dataset %>% mutate(reverse_zipcode(as.character(zip_code)))
  county.data = county.data %>% group_by(county, state) %>% count(county, wt=n, name="patients")
  #changing some of the columns formats so that they can be mapped later
  county.data = county.data %>% mutate(subregion= str_split_1(tolower(county), " county")[1]) %>% mutate(region = state.codes[state])
  #creating full dataset used to create the county map by joining the dataset
  county.map = full_join(county.data, middie, by=c("region", "subregion"))
  #creates a ggplot objects which shows a map of where the patients are coming from
  county.ggplot = region + geom_polygon(data=county.map, aes(fill=patients, text=paste0(subregion, '\n', "Visits: ", patients)), color="white") + scale_fill_gradientn(colours = rev(rainbow(7)), trans = "log10")
  if(full.data){
    return(list(county.ggplot, county.map))
  }else{
    return(county.ggplot)
  }}

cc = to.map(zip.pats, T)
county.ggplot = cc[[1]]
county.data = cc[[2]]
county.one.map = to.map(zip.one.pats)
county.two.map = to.map(zip.two.pats)
county.plotly = ggplotly(county.ggplot, tooltip="text")

county.one.plotly = ggplotly(county.one.map, tooltip="text")
county.two.plotly = ggplotly(county.two.map, tooltip="text")

#side-by-side plot for both patient types by visits per county
side.by.side.plotly = subplot(county.one.plotly, county.two.plotly) %>% layout(annotations = list(
 list(x = 0.02 , y = 1.05, text = "Visits Per County for Patient Type 1", showarrow = F, xref='paper', yref='paper'),
  list(x = 0.95 , y = 1.05, text = "Visits Per County for Patient Type 2", showarrow = F, xref='paper', yref='paper'))
)

#writing the ploly maps to a file
saveWidget(county.plotly, "countyPlotly.html")
saveWidget(side.by.side.plotly, "sidebyside.html")
kable(head(zip.pats), format = "markdown", caption="Table with patients by Zip Code")
kable(head(county.data), format = "markdown", caption="Full Dataset with Map and Patient Data")
wisc.up.map
county.ggplot
sessionInfo()
```

### R Session Information

``` r
sessionInfo()
```

    ## R version 4.3.2 (2023-10-31)
    ## Platform: x86_64-pc-linux-gnu (64-bit)
    ## Running under: EndeavourOS
    ##
    ## Matrix products: default
    ## BLAS:   /usr/lib/libblas.so.3.11.0
    ## LAPACK: /usr/lib/liblapack.so.3.11.0
    ##
    ## locale:
    ##  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C
    ##  [3] LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8
    ##  [5] LC_MONETARY=en_US.UTF-8    LC_MESSAGES=en_US.UTF-8
    ##  [7] LC_PAPER=en_US.UTF-8       LC_NAME=C
    ##  [9] LC_ADDRESS=C               LC_TELEPHONE=C
    ## [11] LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C
    ##
    ## time zone: America/Chicago
    ## tzcode source: system (glibc)
    ##
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base
    ##
    ## other attached packages:
    ##  [1] knitr_1.45        htmlwidgets_1.6.2 plotly_4.10.3     zipcodeR_0.3.5
    ##  [5] lubridate_1.9.3   forcats_1.0.0     stringr_1.5.0     dplyr_1.1.3
    ##  [9] purrr_1.0.2       readr_2.1.4       tidyr_1.3.0       tibble_3.2.1
    ## [13] ggplot2_3.4.4     tidyverse_2.0.0
    ##
    ## loaded via a namespace (and not attached):
    ##  [1] gtable_0.3.4       raster_3.6-26      xfun_0.41          tigris_2.0.4
    ##  [5] lattice_0.22-5     tzdb_0.4.0         crosstalk_1.2.0    vctrs_0.6.4
    ##  [9] tools_4.3.2        generics_0.1.3     curl_5.1.0         proxy_0.4-27
    ## [13] fansi_1.0.5        RSQLite_2.3.3      highr_0.10         blob_1.2.4
    ## [17] pkgconfig_2.0.3    KernSmooth_2.23-22 data.table_1.14.8  uuid_1.1-1
    ## [21] lifecycle_1.0.3    farver_2.1.1       compiler_4.3.2     munsell_0.5.0
    ## [25] terra_1.7-55       codetools_0.2-19   maps_3.4.1.1       htmltools_0.5.7
    ## [29] class_7.3-22       lazyeval_0.2.2     yaml_2.3.7         pillar_1.9.0
    ## [33] crayon_1.5.2       ellipsis_0.3.2     classInt_0.4-10    cachem_1.0.8
    ## [37] tidyselect_1.2.0   rvest_1.0.3        digest_0.6.33      stringi_1.7.12
    ## [41] sf_1.0-14          labeling_0.4.3     fastmap_1.1.1      grid_4.3.2
    ## [45] colorspace_2.1-0   cli_3.6.1          magrittr_2.0.3     utf8_1.2.4
    ## [49] e1071_1.7-13       withr_2.5.2        scales_1.2.1       rappdirs_0.3.3
    ## [53] sp_2.1-1           bit64_4.0.5        timechange_0.2.0   rmarkdown_2.25
    ## [57] httr_1.4.7         bit_4.0.5          hms_1.1.3          memoise_2.0.1
    ## [61] evaluate_0.23      viridisLite_0.4.2  rlang_1.1.2        Rcpp_1.0.11
    ## [65] glue_1.6.2         DBI_1.1.3          xml2_1.3.5         rstudioapi_0.15.0
    ## [69] jsonlite_1.8.7     R6_2.5.1           tidycensus_1.5     units_0.8-4
