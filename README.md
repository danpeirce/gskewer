# About Gskewer
Gskewer is a tool to skew transform gcode file coordinates to account for axis misalignment of a 3D printer.

This is a fork of a repository I found a link to in some forum. I don't recall which. A new search found this page whis is likly simular or the same as what I first saw.

[https://www.thingiverse.com/groups/cura/forums/general/topic:31706](https://www.thingiverse.com/groups/cura/forums/general/topic:31706)

In order to use Gskewer you will need to print a test ~~cube~~ squares, take accurate measurements of the ~~cube~~ squares, then input those measurements as arguments 
for Gskewer. 

~~The G-code file to be modified, the measured error (in mm), and the distance from zero where the measurement was taken is then entered into skew.py before being run.~~

The method I used was different that what I struck out above. Note that I have a version of Python on my laptop and run scripts from the command line.

# Preparing to use Gskewer
I devised my own way to use the calibration square. First I should point out the skew error I was concerned about was in XY 
as the XY skew became apparent after printing a box with a tight fitting lid. The square for the XY plain that I printed looked like this:

![](calibration_square.png)

** *working on edit* **

![MechanizedMedic](https://github.com/MechanizedMedic/gskewer/raw/master/gskewer_measuring1.png "Positive skew error.")
![MechanizedMedic](https://github.com/MechanizedMedic/gskewer/raw/master/gskewer_measuring2.png "Negative skew error.")

These measurements are to be taken for each of the three axis pairs of the cube: XY, YZ, and ZX.

You will end up with six measurements/arguments: xylen, xyerr, yzlen, yzerr, zxlen, and zxerr.

The initial six measurements can be simplified to a tangent argument by dividing the error by length. (ie: xyerr/xylen=xytan) The three tangent arguments are: xytan, yztan, and zxtan.


# Using Gskewer
`gskewer [arguements] file`

Gskewer will automatically generate a new gcode file with "-skewed" added to the file name. If the output file name already exists gskewer will delete the existing file and write a new one.

### Examples

`gskewer --xyerr 1.2 --xylen 80 Cube80mm.gcode` will adjust the X coordinate proportionally by -1.2 mm for every 80mm in the Y coordinate. Also, only the XY plane is affected as other arguements are not used. The output file will be "Cube80mm-skewed.gcode".

`gskewer --xytan 0.015 Cube80mm.gcode` is equivalent to the example above, as `xytan = xyerr / xylen`.

### Gskewer Arguments
`--xyerr`
	Error in the X-axis for the XY pair in mm. (This argument cannot be used with "xytan")

`--xylen`
	Length of the test cube side where the "xyerr" measurement was taken.

`--xytan`
	The error in the ZX pair as a tangent. (zxerr/zxlen)

`--yzerr`
	Error in the Y-axis for the YZ pair in mm. (This argument cannot be used with "yztan")

`--yzlen`
	Length of the test cube side where the "yzerr" measurement was taken.

`--yztan`
	The error in the YZ pair as a tangent. (yzerr/yzlen)

`--zxerr`
	Error in the Z-axis for the ZX pair in mm. (This argument cannot be used with "zxtan")

`--zxlen`
	Length of the test cube side where the "zxerr" measurement was taken.

`--zxtan`
	The error in the ZX pair as a tangent. (zxerr/zxlen)
