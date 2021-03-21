Final Multilevel Model
================

The lme4 library allows us to build and to analyze data using the
hierarchical linear models.

``` r
library('lme4')
```

    ## Loading required package: Matrix

The following command loads the preprocessed *data.csv* file containg
836 datapoints gathered from our first study.

``` r
df <- read.csv('data.csv', header=TRUE)
```

  - The column named as *layout\_images* was added to the original
    *rawData.csv*. It represents a combination of the two most
    informative view parameters: *images* and *layout*. The *layout\_images* column contains following values:

      - *gw* - representing *gridView* layout with images
      - *ln* - representing *largeCards* layout without images
      - *lw* - representing *largeCards* layout with images
      - *mw* - representing *miniCards* layout with images
      - *xn* - representing *xLargeCards* without images
      - *xw* - representing *xLargeCards* without images


  - For the purpose of observing users behaviour whilst using a different
    application theme in different levels of environemntal brightness we
    have categorized *enviornmental\_brightness* parameter into 4
    different categories:
    
      - Label *L1* represents dark environments and groups sensor values
        which are lying between 0 lux and 20 lux.
      - Label *L2* represents public areas with dark surroundings and
        groups sensor values lying between 20 lux and 50 lux.
      - Label *L3* represents surroundings smillar to family living
        rooms and groups sensor values ranging from 50 lux to 100 lux
      - Label *L4* in our case represents bright environments and
        contains values greater than 100 lux.

While gradually increasing the complexity of multilevel models (as
described in our paper: Uncovering Personal and Context-Dependent
Display Preferences in Mobile Newsreader App) we concluded that the
following model can be used for describing the relationships between
contextual and news display
parameters:

``` r
final.model <- lmer(score ~ 1 + user.activity*font.size + theme*env.brightness + theme*battery.level + (1|layout_images), data=df)
```

Parameters such as: *user activity*, *font size*, and *theme* are
included as the first level predictors, whilst the parameter
*layout\_images* represents the second level predictor.

Additionally, the previously mentioned model also includes crosslevel
interaction terms, which are used to further investigate specific
relationships between:

  - *user activity* and *font size*
  - *theme* and *battery level*
  - *theme* and environmental brightness

The following command gives us values for each intercept which is
defined by our random effect parameter labled as *layout\_images*. It
also includes the values of each coefficient and their interaction terms
between the first level predcitors. Using these values our multilevel
model is able to predict the final users assesment of the configured UI.

``` r
coefficients(final.model)
```

    ## $layout_images
    ##    (Intercept) user.activityON_FOOT user.activitySTILL font.sizesmall-font
    ## gw   1.4820596             3.201156           4.013995          -0.1019035
    ## ln   1.3010936             3.201156           4.013995          -0.1019035
    ## lw   3.8098584             3.201156           4.013995          -0.1019035
    ## mw   2.9741858             3.201156           4.013995          -0.1019035
    ## xn  -0.3626649             3.201156           4.013995          -0.1019035
    ## xw   3.1800444             3.201156           4.013995          -0.1019035
    ##    themelight-theme env.brightnessL2 env.brightnessL3 env.brightnessL4
    ## gw       -0.9414926        0.5747706         -1.48338        -2.387914
    ## ln       -0.9414926        0.5747706         -1.48338        -2.387914
    ## lw       -0.9414926        0.5747706         -1.48338        -2.387914
    ## mw       -0.9414926        0.5747706         -1.48338        -2.387914
    ## xn       -0.9414926        0.5747706         -1.48338        -2.387914
    ## xw       -0.9414926        0.5747706         -1.48338        -2.387914
    ##    battery.level user.activityON_FOOT:font.sizesmall-font
    ## gw    -0.0406462                                -1.901058
    ## ln    -0.0406462                                -1.901058
    ## lw    -0.0406462                                -1.901058
    ## mw    -0.0406462                                -1.901058
    ## xn    -0.0406462                                -1.901058
    ## xw    -0.0406462                                -1.901058
    ##    user.activitySTILL:font.sizesmall-font themelight-theme:env.brightnessL2
    ## gw                             -0.4989576                        -0.1344448
    ## ln                             -0.4989576                        -0.1344448
    ## lw                             -0.4989576                        -0.1344448
    ## mw                             -0.4989576                        -0.1344448
    ## xn                             -0.4989576                        -0.1344448
    ## xw                             -0.4989576                        -0.1344448
    ##    themelight-theme:env.brightnessL3 themelight-theme:env.brightnessL4
    ## gw                         0.9142914                          2.265102
    ## ln                         0.9142914                          2.265102
    ## lw                         0.9142914                          2.265102
    ## mw                         0.9142914                          2.265102
    ## xn                         0.9142914                          2.265102
    ## xw                         0.9142914                          2.265102
    ##    themelight-theme:battery.level
    ## gw                      0.0158676
    ## ln                      0.0158676
    ## lw                      0.0158676
    ## mw                      0.0158676
    ## xn                      0.0158676
    ## xw                      0.0158676
    ## 
    ## attr(,"class")
    ## [1] "coef.mer"

The lme4 package also contains a *predict* function which accepts as an argument a specific multilevel model and a dataframe row. In our case using the predict function our model is able to calculate the final user's view assessment depending on the contextual and UI related parameters.  