# EMG
MATLAB functions and scripts for processing electromyography (EMG) signals.  
Originally written for data from a CleveMed BioRadio comparing the tricep and 
deltoid during dumbbell press and perfect pushup, so some things (such as the 
`process` script) are specific to that, but most things will work for any EMG 
application.  

## Data Structures
### EMG Structs
These scripts make use of structs to store and pass EMG data between functions. 
An EMG struct has the following elements:  

|:--------|:-------|
|`signal`|A list of the EMG samples|
|`time`|A list of time values corresponding to the samples, starting from 0|
|`l`|the length (number of samples) of the signal|
|`starts`|A list of the indexes of 'start' event markers|
|`stops`|A list of the indexes of 'stop' event markers|
|`n`|The number of pairs of 'start' and 'stop' event markers|
|`fs`|The sampling frequency in Hz|

### 4-dimensional array of all subjects, exercises, reps, and muscles
The `process` script stores data in four-dimensional arrays with the first 
index corresponding to subject number, the second corresponding to exercise, the
third corresponding to rep number, and the fourth corresponding to muscle 
number; i.e. `movingrms(subject, exercise, rep, muscle)`.  To get all of a 
certain index (e.g. all reps for a specific subject, exercise, and muscle) use 
a colon (`:`) in place of the index (e.g. array(subject, exercise, :, muscle).  

## Signal Processing Functions
### importemg
imports emg data from a csv file with the following columns:  
deltoid, tricep, start events, stop events

e.g.: 
```
deltoid,            tricep,             Start,  End,
-0.035858154296875, 0.444793701171875,  0,      0,
0,                  0.428009033203125,  0,      0,
-0.018310546875,    0.516510009765625,  0,      0,
...
```

Usage: `[delt, tri] = importemg(filename, fs);` where `filename` is the name of the 
file from which the data is to be imported and `fs` is the sampling frequency.  

`delt` and `tri` are EMG structs

### timebasis
Creates a list of time values starting from zero given a list of samples and the
sampling frequency.  

#### Usage: 
```matlab
t = timebasis(signal, fs);
```
Where `signal` is the signal for which the time basis is to be created and `fs` 
is the sampling frequency in Hz.  `t` is a list of time values corresponding to 
the samples in `signal`.  

### filteremg
Butterworth band-pass filters an EMG struct with a passband of 20--400 Hz 
unless cutoff frequencies are specified.  
#### Usage: 
```matlab
filtered = filteremg(unfiltered);
```
or
```matlab
filtered = filteremg(unfiltered,fl,fh);
```
where `fl` is the low cutoff, and `fh` is the high cutoff of the passband in Hz.

### deMainsEMG
Notch-filters an EMG struct at 60 Hz to remove interference from electrical 
systems.  

#### Usage: 
```matlab
filtered = deMainsEMG(unfiltered);
```

### crop
Extracts a portion of an EMG signal using the start and stop endpoints
#### Usage:
```matlab
exerpt = exerpt(emg, n);
```
Where `n` specifies which pair of start/stop event markers to use.  For example,
`excerpt(emg, 3);` returns the signal between the third 'start' event marker, 
and the third 'stop' event marker.  Problems may occurr if the event markers 
don't line up, for example if there's an extra 'start' at the beginning of the 
data.  

### movingRMS

## process

## Plotting Functions
### plotemg

### colorplotemg

### compareplot

### spectrumemg
