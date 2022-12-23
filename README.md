# straycat
 Yet another WORLD-based UTAU resampler.

# How to use (development version)
 You need to have Python installed. This was made using Python 3.8.10.
 
 You must install the needed libraries first, which are numpy, scipy, resampy, and pyworld. To do that, you may run a regular pip installation:
 
 ```pip install numpy scipy resampy pyworld```
 
## Running in UTAU
 Download the `straycat.py` file and put it somewhere. Open your `.ust` file as a text file with whichever text editor. Change the `Tool2` resampler to the path of `straycat.py`. You can now open the `.ust` and use `straycat.py` as a resampler. You may need to press cancel for the project properties when UTAU shows the project properties panel.
 
## Running throught terminal
 Most resamplers can take arguments to render a sample. This resampler only reads the terminal arguments.
 
```usage: straycat in_file out_file pitch velocity [flags] [offset] [length] [consonant] [cutoff] [volume] [modulation] [tempo] [pitch_string]

Resamples using the WORLD Vocoder.

arguments:
	in_file		Path to input file.
	out_file	Path to output file.
	pitch		The pitch to render on.
	velocity	The consonant velocity of the render.

optional arguments:
	flags		The flags of the render.
	offset		The offset from the start of the render area of the sample. (default: 0)
	length		The length of the stretched area in milliseconds. (default: 1000)
	consonant	The unstretched area of the render in milliseconds. (default: 0)
	cutoff		The cutoff from the end or from the offset for the render area of the sample. (default: 0)
	volume		The volume of the render in percentage. (default: 100)
	modulation	The pitch modulation of the render in percentage. (default: 0)
	tempo		The tempo of the render. Needs to have a ! at the start. (default: !100)
	pitch_string	The UTAU pitchbend parameter written in Base64 with RLE encoding. (default: AA)```
	
# straycat flags

 This is the main documentation of the flags available in straycat.

## Valued flags

### fe, fl, fo, fv
 Fake vocal fry flag (and glottal stop flag).

 fe(-inf, +inf) is the main flag to enable this feature. fe is the length of the vocal fry in milliseconds. It adds the vocal fry so that the end of it is at the consonant point of the oto.

 fl[1, +inf) is the length of the transition to vocal fry in milliseconds. Default is 75.

 fo(-inf, +inf) is the offset of the vocal fry in milliseconds. Negative values move it earlier. Default is 0.

 fv[0, 100] is the volume of the vocal fry in percentage. Default is 10. Turning it into 0 makes a glottal stop.

### ve, vo
 Flag for fixing voicing.

 ve(-inf, +inf) is the main flag to enable this feature. ve is the length of the transition from voiced to unvoiced centered at the consonant point. Positive values make the area after the consonant unvoiced and negative values make the area before the consonant unvoiced.

 vo(-inf, +inf) is the offset of the transition from the consonant in milliseconds. Negative values move it earlier. Default is 0.

### B[0, 100]
 Breathiness flag. Values lower than 50 lowers breathiness, but it does not have much effect. Values higher than 50 mixes an unvoiced render in, with 100 as being only the unvoiced render. Default is 50.

### A(-inf, +inf)
 Tremolo flag. This flag tries to isolate the vibrato from the pitchbend and modulates the volume based on this isolated vibrato, which means it may also react on drawn vibrato and more. Default is 0.

### P[0, 100]
 Peak normalization flag. This flag normalizes the sample to have the same peak volume for each volume. At 0, this flag does not touch the volume of the render at all. Default is 100.

### t(-inf, +inf)
 Pitch offset flag. One unit in this flag is a cent offset. It offsets the whole note so it might result in bad crossfades for VCV voicebanks. Default is 0

## Option flags
### G
 Force feature rerendering. This rerenders the cached file straycat reads which is the ".sc.npz" file. It is a regular Numpy compressed array file.