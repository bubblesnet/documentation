# Notes

If you haven't yet, read [the history of this project](History.md).

In no particular order, here are the things I'm thinking about at the end of
the most recent grow.

## Calibration
Speaking of calibration, if this were a commercial or scientific project calibration management would have been one of the first
features off the backlog. I check frequently and what I found is that the commercial sensors I've used were mostly 
"good enough for the purpose at-hand" as-is and that drift hasn't 
been a problem.  So calibration as an issue has languished at the bottom of the backlog.

## Wiring
Most of the wiring in this project was done using prototype 2.54mm jumpers, which was less than satisfactory in production.
The top of the backlog item right now is to produce a PCB that puts as much of that on solid mounting as possible.  My goal
there would be to allow the commercial modules we use to be plugged directly into the PCB and to have everything else use
secure commercial connectors.

## Power Usage
Minimizing power usage was not a [requirement](Requirements.md). Power monitoring was a nice-to-have that never moved
up the backlog.

## Coding style, particularly naming
This project includes significant chunks of SQL, Go, Python and NodeJS code as well as various build systems and test
frameworks. Going back and forth between them is often a whiplash experience for me as a coder. You can see this
most clearly in file/method/variable names.  Snake-case, camel-case, occasionally Pascal-case almost every container
has "names" that are appropriate for a different language.  I've
tried to follow [PEP 8](https://peps.python.org/pep-0008/) for Python (enforced in my editor) while Go and NodeJS have
generally just been lint-free. 

## Space management
The edge-device manages its free space well, has no known leaks and in the last several versions has
never had a free-space issue.

The controller collects data and thus leaks by-design.  This is addressed by moving database backups, pictures
and log files to the mounted flash drive at /dev/sda1.  I manage space by occasionally shutting down the device
removing the flash drive and moving the files off it to permanent storage.  So ... the controller does leak free
space but only in the Postgresql database.  Dealing with this is a high priority.

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
too-hot air directly on a plant. It works okay when it's only used occasionally in 
short bursts.
Next grow I will try something else, either multiple, smaller radiant heaters mounted
internally, or a more powerful heater mounted externally.

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

## Door-open sensors
The cabinet-door and external-door were implemented, and work, but mostly weren't used in production.

## Current Sensing/Power Consumption Sensing
Never got around to it, though it shows up in schematics.

## Autonomous Devices
The water heater I use has its own thermostat. I wanted to incorporate "autonomous" as an attribute of an acoutlet device but never finished.
This is bottom-of-the-backlog issue as it can be worked around by setting parameters that
have the device always powered.

## Stages
IDLE, VEGETATIVE and BLOOMING stages are well tested, the rest mostly not used. The cabinet
as built was not suitable for germination.

## Store-and-forward Queue Clogging
If the edge-devices store-and-forward container is disconnected from the controller API for a long period - say 24 hours - 
it will take a long time to drain the BoltDB of messages needing to be forwarded.  In a development
environment, the BoltDB is cleared every time the container restarts specifically to get 
around this.   

## Multiple Stations
The data structures and messaging are designed to support multiple stations(cabinets) from a 
single controller instance.  The user interface does not support it yet.  This is middle-of-the-backlog
issue.

## NodeRed
Why didn't I use NodeRed for this instead of ActiveMQ? Simple answer is that I started this
project in 2017 and NodeRed wasn't a thing then. ActiveMQ never failed me (except in its
exorbitant memory usage) so I never had an excuse to move away from it.

## Extensibility
The components of the system are behind APIs that should allow users to add
on to an installation of Bubbles.

## Commit history around the go-public date
This project was made public around 12/1/2022. In order to "get it out the door" I pushed a number of commits
directly to the develop branch to resolve what I expected to be minor CI issues. It turned
out that a number of tests were broken/obsolete and it took a string of crappy commits
to figure that out. Going forward, pushing directly to develop is verboten.

## Operating Systems
Most, if not all, of the balena containers in this system run Debian Buster. The Github
Actions CI runner uses Ubuntu. I developed and tested mostly on Windows (hence the bat and cmd files) but 
also on Mac (hence the darwin build directives in Go).

## SBC
SBC (single board computer) is a relic of a historical project that still exists in some
code in the system. Ignore.

## ICEBREAKER
ICEBREAKER is a project that this project inherited some code from. Ignore.

### Examples:

A user could add a new container to the edge-device that
simply sends properly formatted gRPC messages to the store-and-forward container
and they will end up in the database and accessible as events in the UI.

A user could send properly formatted messages to the Edge/Server API that
would end up in the database and UI.

## Mea Culpa

Testing - my tests are insufficient. Testing IoT is painful, especially device-dependent code like the majority of the edge-device code.
Automation - CI is in-place, but usually broken because of broken tests.  This may be a focus going forward.


