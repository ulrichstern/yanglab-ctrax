## Tracking and analyzing the sample video ##

This project has a 10-minute sample video (from our "UV on/off" experiments) that can be used to go through the tracking and analysis process.  The sample video is 16.1MB, 320x240 pixels, 7.5fps, grayscale, and in M-JPEG format.  (It is the first 10 minutes of an 8h video we recorded using 640x480 pixels, 15fps, color, and H.264 format and then converted using Avidemux and [convertStandard.js](https://code.google.com/p/yanglab-ctrax/source/browse/trunk/goodies/convertStandard.js).  The reason for the different formats and some typical file sizes are given in [this](https://groups.google.com/d/msg/ctrax/mB-xd0NPFpg/L8B_ZC5T-sEJ) Ctrax Google Group post.)

  * **Download the sample video**
    * In the directory where you installed run-ctrax, execute
```
svn co https://yanglab-ctrax.googlecode.com/svn/trunk/sample-video
```
  * **Track**
    * In your run-ctrax directory, execute
```
python runCtrax.py   # "python -u" can be required on Windows
```
> > (I usually create a shortcut for runCtrax.py and then use the shortcut.)  The "movie subdirectory" runCtrax.py asks for is `run-ctrax/sample-video` in this case.
    * Tracking for the sample video takes about 64s on an i7-4770 machine.
    * After tracking is done, the sample-video directory has several new files, including
      * An image showing the backgrounds calculated (ending in "bg.png").  Note that there are two backgrounds calculated in this case -- one for "UV off" and one for "UV on."
      * An image showing the template match used for the shadow detector (ending in "tm.jpg").
  * **Analyze**
    * Start MATLAB.
    * Go to your Ctrax-allmatlab-X.Y.Z/behavioralmicroarray directory and execute
```
setuppath
cd ../yanglab/
```
    * Load the Ctrax MAT-file (which contains the trajectory data) and analyze it:
```
[trx,matname,succeeded] = load_tracks
analyze_tracks                          % select "Cancel" when asked to choose an egg file
```
> > The first MATLAB figure should look like this:
> > ![http://yanglab.pbworks.com/w/file/fetch/83657839/borders-and-trajectories.png](http://yanglab.pbworks.com/w/file/fetch/83657839/borders-and-trajectories.png)
    * To plot the y position over time, edit analyze\_tracks.m to have `numPeriods = 1` (otherwise, the 10 minutes will be broken into 8 periods of 1.25 minutes each) and then execute:
```
YL.plotTitle = 'sample'
analyze_tracks
```
> > For fly 1, the y-position plot should look like this:  (Purple background indicates "UV on.")
> > ![http://yanglab.pbworks.com/w/file/fetch/83658310/fly1-y-position.png](http://yanglab.pbworks.com/w/file/fetch/83658310/fly1-y-position.png)

## Changing the parameters for our Ctrax extensions ##

The parameters for our extensions with default values are:
```
use_shadow_detector = False
shadow_detector_minarea = 50
recalc_bg_minutes = 60         # 0: do not recalculate
percentile_for_bg = 0          # 0: median
```
We change the parameters for our extensions via editing the settings file. (In fact, the parameters for our extensions can currently not be changed in the GUI.)  A sample settings file (settings.ann) is in the [run-ctrax](https://code.google.com/p/yanglab-ctrax/source/browse/#svn%2Ftrunk%2Frun-ctrax) directory.  Here the first few lines of the file:
```
Ctrax header
version:0.3.1
use_shadow_detector:1
recalc_bg_minutes:60
percentile_for_bg:20
bg_type:dark_on_light
...
```

## Features of the analysis scripts ##

Our lab's Wiki has a [page](http://yanglab.pbworks.com/analyze_tracks%20(public)) with more details on how to use our analyze\_tracks script.