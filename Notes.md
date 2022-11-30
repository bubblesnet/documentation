# Notes

In no particular order, here are the things I'm thinking about at the end of
the most recent grow.

## The camera
I use a standard Pi camera with a VERY long cable and take pictures on a schedule
that I then upload to the controller.  The camera is unsatisfactory in that it doesn't
do well with the grow lights (everything looks pink). The fixed mount is very 
restrictive. At the least, I would change it to auto Pan/Tilt/Zoom which I've already done
for another project. Also, it should be VERY easy to add a "live" stream look-in
capability.

## Mounting for the heater
I use a cheap, low-power space heater like the ones office workers keep under
their desks.  There is no secure place to mount it in the cabinet where it isn't blowing
too-hot air directly on a plant. Next grow I will try something else, probably mounting
the heater externally.

## Mounting for the humidifier
When it's dry here, it's REALLY dry and the tiny humidifier that fits inside the cabinet
holds only about 12 ounces of water. Using it on auto ran it out of water way too fast.  Plan for next grow is to use the same humidifier, still 
mounted internally but replace the 12 oz reservoir with a remote feed from a much larger reservoir.

## Plant Height
The HCSR04 distance sensor, mounted above the plant wasn't the right solution.  Never worked well enough detecting 
the TOP of the plant which is what I needed to keep the light at the right distance.  
Display of variable plant height on the station control screen was never implemented.

## Grow Light
Keeping the grow light at the appropriate distance from the plants was a constant 
struggle. Ideally I'd have a jackscrew driven light mount from the top, but I don't
really even have a good sense of what that would look like or require.

## Current Sensing/Power Consumption Sensing
Never got around to it, though it shows up in schematics.

## Autonomous Devices
The water heater I use has its own thermostat. I wanted to incorporate "autonomous" as an attribute of an acoutlet device but never finished.

## Stages
IDLE, VEGETATIVE and BLOOMING stages are well tested, the rest mostly not used. The cabinet
as built was not suitable for germination.

## Extensibility
The components of the system are behind APIs that should allow users to add
on to an installation of Bubbles.

### Examples:

A user could add a new container to the edge-device that
simply sends properly formatted gRPC messages to the store-and-forward container
and they will end up in the database and accessible as events in the UI.

A user could send properly formatted messages to the Edge/Server API that
would end up in the database and UI.


