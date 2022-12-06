# History (i.e. where the bugs come from)

## Why?
In 2016, Massachusetts voters legalized possession of recreational marijuana via
a ballot initiative that the Democratic-controlled state government almost
universally opposed.
The original legislation legalized possession and growing, but left recreational
sales trapped behind a regulation-writing process that was intentionally slow-walked
by the state legislature. There was no reasonable expectation that recreational sales
would ever be opened up, and indeed recreational sales only began in late 2018. 
This left growing your own as the only avenue, so I walked
that path.

# Where did the project come from?
This instance of the project is a port and integration of an existing set of unrelated personal projects.  This
original set of projects included:
* Server written in Java J2EE as APIs that write JSON files to filesystem
* File processors written in Java that read/write ActiveMQ, move file data into databases and archive files
* A MySQL database
* An Android/Android Things edge device that itself was a rewrite of a RPi python edge device

## How this version of the project was developed
This project has, from day one, been iteration on a working system with deployments to production at least weekly
and often multiple times per day.  I made features work to the point where they solved the problem at hand, then
moved on.  The downside of this can be seen most clearly in the calibration screen on the user interface.  I had what
seemed like a calibration problem and started the process of dealing with calibration, then discovered that the issue
was something else, so truncated that feature in-place and moved on.

