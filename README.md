2D Virtual Acousmonium
======================

Introduction
------------

This is a collection of abstractions for Pd-extended which can be used together with reacTIVision to control sounds inside a virtual acousmonium, a virtual space in which a listener can physically navigate. Some of these abstractions may also be useful for more conventional usages of reacTIVision as well (i.e., using it with objects in a table).

The setup for this sound installation is simple, the necessary things are:

- an overhead webcam, preferably a wide-angle camera
- a wireless pair of headphones
- a large square in which the fiducial marker no. 0 is printed to (this has to be attached to the top of the headphones)

Because there are no sounds being reproduced via speakers (that is, this is a silent installation for everyone expect for the participant with the wireless headphones), I found out that it's interesting to use a headphone splitter/router and offer some other members of the audience the possibility of listening to what the active participant is listening (that is, routing the same 2 channels into a series of wired headphones). This also seem to prepare them better for experimenting the space.

For some examples of this installation, see: 

![image1](http://s7.postimg.org/yrdglf3vf/Screenshot_from_2015_05_28_13_38_05.png)

![image2](http://s7.postimg.org/st48oxaaj/DSC05107.jpg)

- photo album: http://imgur.com/a/uJhAp

- sample video: https://vimeo.com/141252952

For some more information (unfortunately only in Czech language), see: RATAJ, M., AGOSTINHO, G. et al, "Dotknout se zvuku", in: _Opus Musicum_, Vol. 47, No. 4, Brno 2015, pp. 58-81, ISSN 0862-8505

Requirements
------------

This collection of abstractions should work under Linux, OS X and Windows, though it has been tested only in the first two. It requires two other objects to be installed, both included in this package. They are:

- TuioClient : the object which receives the TUIO data from reacTIVision inside Pure Data. Simply copy these files to your Pd-extended path and you should be good to go. For more info on reacTIVision, see: http://reactivision.sourceforge.net/

- +binaural~ : an external from the Soundhack. If you are using Windows, OS X or Linux 64-bit, simply use the correct external included in the folder +binaural~ (that is, +binaural~.dll, +binaural~.pd_darwin and +binaural~.pd_linux, respectively). If you are using Linux 32-bit, you must use the the file +binaural~.pd_linux32 (but first rename it to +binaural~.pd_linux). For more info on Soundhack, see: https://puredata.info/downloads/soundhack

Abstractions
------------

The abstractions contained in this library are:

- 2D.TUIO.data : receives the data from reacTIVision

- 2D.TUIO.mapping : maps the data received (which ranges between 0 and 1) into new values to be used by the abstractions. Angles are automatic converted from 0-2pi into 0-360, but space must be manually entered by the user via its two arguments. A reasonable value is 1000 for both x and y.

- 2D.limits : within this square of 1000x1000, one might want to fade the sound away when the participant comes close to the edge (so that if the marker disappears from the camera view there won't be any sound playing). This abstraction let one create a smaller rectangle inside the camera view in which the sounds will be active. Stepping out of this rectangle will result in an abrupt fade out. It take four arguments: xmin, ymin, xmax, ymax. It's also possible to mark this rectangle in the floor with tape, so that the participants know the area they can explore.

- 2D.dac : outputs the sound manipulated by the other abstractions.

- 2D.visualiser : shows the current position and angle of the listener according to the data received from reacTIVision. If 2D.TUIO.mapping's arguments are set to a different value than 1000 1000, then the grid object will display the position incorrectly as it cannot be dynamically set. The solution is to manually edit this abstraction by right clicking on the grid object, selecting Properties and setting the X max and Y max values to different values.

- 2D.overrider : let's one override the current values. This is particularly useful if the tracking gets momentously stuck (e.g. if light conditions change abruptly). I suggest having one of these at hand at all times.

- 2D.sound.omni : makes a sound omnipresent: that is, it will be played at the same volume and equally at all angles regardless of the position of the participant.

- 2D.sound.omnispot : the sound will be fixed in a post, and the closer the participant gets to it the louder it gets. Angle is ignored, so if the listener stays in a position and spins around it will sound the same. Takes two arguments: x and y position of the sound.

- 2D.sound.spot : similar to above, but both angle and distance affect the result. Takes two arguments: x and y position of the sound.

- 2D.sound.vector : creates a moving sound within in a line. Takes five arguments: x and y values of starting point (A), x and y values of ending point (B) and time (in ms) for the transition from A to B to A.

- 2D.sound.circle : creates a moving sound within in a circle. Takes four arguments: x and y values of the centre of the circle, circle radius and period for the spin (in ms). If the last argument (time) is positive, the movement is clockwise, if negative the movement is anticlockwise.

See the file example.pd for more information.

Pitfalls
--------

During the three sound installations I presented using this collection I encountered several problems:

- size of the fiducial markers: the larger the markers the more reliable the detection is, particularly if the space is to be large. See images above for a reasonable solution.

- light sensitivity: when working with light table or small objects under artificial light, reacTIVision is very reliable. When dealing with a large space, the whole setup becomes extremely sensible to the light conditions. A windowless place is always preferred as artificial light does not change. All the attempts of performing this outdoors failed and I would advise against it.

- frames per second: the camera and light intensity should be set to reach a minimal of 25 fps in order to track the markers properly. If the fps rate gets lower than that, the markers may become blurred when someone is moving and therefore the tracking ins affected.

- CPU intensive: the external +binaural~ is relatively CPU intensive and each instance of the five 2D.sound.* abstractions use one, so I recommend using them with care. Also, I recommend doing long tests with the setup to see CPU and memory use (let it run for a couple of hours); also, run these tests together with reacTIVision even if no fiducial marker is in sight, as reacTIVision is also a CPU intensive program.

Disclaimer
----------

This collection of abstractions is offered as is and I take no responsibility for any potential problems it may cause on your system. You are using this at your own risk.
