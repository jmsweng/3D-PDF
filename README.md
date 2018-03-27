# 3D-PDF

## Modified hampel filter:
    -Shows current best filter used for preprocessing data before calculating PDF
    -Detects bragg peaks by checking the center value a cube shaped moving window if it is
     an outlier or not
    -Outliers are determined to be 3σ away from the median value in the window, σ is estimated
     as 1.4826*MAD (median absolute deviation)
    -Points determined to be outliers are set to 3.5σ from the median
    -All other values are unmodified

###  Notes:
    -Simply removing the bragg peaks by replacing them with the median seems to create strange 
     negative correlations where positive correlations are expected
    -Moving window size was selected by using width of the diffuse features determined by visual
     inspection, 5 is used for the PMN data. There is likley some better way to do this.
    -Weird center pattern seems to be unique to the material and changes slightly based on 
     experiment conditions. Also doesn't seem to have significant features that are created by
     edge effects (see relevant jupyter notebook)

## Punch and fill:
    -Quick (and probably incorrect) implementation of a punch and fill algorithm where bragg
     peaks are punched out and the volume is convolved with a gaussian
     
## Edge effect test:
    -Shows the FT of whatever 3D shape is created by the detector coverage to see edge effects
     created in the center of the FT volume

## Filter comparisons:
    -Has a bunch of filters that were tested for preprocessing the data, basically all of these
     are useless. Only included in case something is later found to be useful
