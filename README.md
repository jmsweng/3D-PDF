# 3D-PDF

##  Notes not specific to any particular filter:
    -Simply removing the bragg peaks by replacing them with the median seems to create strange 
     negative correlations where positive correlations are expected
    -Moving window size was selected by using width of the diffuse features determined by visual
     inspection, 5 is used for the PMN data. There is likley some better way to do this.
    -Weird center pattern seems to be unique to the material and changes slightly based on 
     experiment conditions. Also doesn't seem to have significant features that are created by
     edge effects (see relevant jupyter notebook). Center features also appear to remain the same
     for a given data set regardless of how peaks are modified.

## Modified hampel filter:
    -Shows current "best" filter used for preprocessing data before calculating PDF (might 
     be adding in data that wasn't there by the way the peaks are reconstructed)
    -Detects bragg peaks by checking the center value a cube shaped moving window if it is
     an outlier or not
    -Outliers are determined to be 3σ away from the median value in the window, σ is estimated
     as 1.4826*MAD (median absolute deviation)
    -Points determined to be outliers are set to 3.5σ from the median
    -All other values are unmodified

## Peak removal filter:
    -Filter which removes bragg peaks and reconstructs the diffuse by replacing missing values
    -Detection of peaks is same as with modified hampel filter
    -Points that are removed are replaced with a value of median+2.2*MAD to reconstruct the
     diffuse peak
    -MAD estimator has a 37% efficiency for gaussian distributed values

## Peak removal filter with S_n estimator
    -Same as peak removal filter except points are replaced with median+1.7697*S_n to 
     reconstruct the diffuse peak
    -Sn estimator doesn't assume a symmetrical distribution like MAD and has an efficiency of 
     58% for gaussian distributed values
    -Estimator is described in doi:10.2307/2291267
    -Parallelization is broken so it takes forever
    -Percent difference between pattersons calculated with MAD and S_n estimator peak at around
     0.25%, so MAD can probably be safely used to estimate scale for diffuse scattering unless
     data is significantly noisier than the test data set used

## Punch and fill:
    -Quick (and probably incorrect) implementation of a punch and fill algorithm where bragg
     peaks are punched out and the volume is convolved with a gaussian

# Everything below this is disorganized garbage and can probably be mostly ignored

## Edge effect test:
    -Shows the FT of whatever 3D shape is created by the detector coverage to see edge effects
     created in the center of the FT volume

## Filter comparisons:
    -Has a bunch of filters that were tested for preprocessing the data, basically all of these
     are useless. Only included in case something is later found to be useful
     
## Stupid filter:
    -Started developing a filter that locates the bragg peaks and only runs over them, currently
     useless and a somewhat incomprehensible mess.
    -May be useful and worth continuing in the future if data sets are found with a significant
     amount of noise; regions without peaks can be filtered with a regular hampel filter and
     bragg peaks can be removed with the modified hampel filter
    -Bragg peaks are identified by projecting the volume along each axis, locating peak regions 
     in 2D, and back projecting the regions over the volume. Peaks are located in regions where
     back projections intersect
