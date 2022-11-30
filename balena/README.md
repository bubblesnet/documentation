# Balena

Balena is an IoT device management platform based on Yocto, a container host OS that runs on IoT devices like the Pi.
I use it for:
* the ability to write components in a bunch of different languages. For example, there is good sample code in Python and Go various sensors.  
* the flexibility to use whatever stack works best/easiest for a particular task without beating my brains in 
on version conflicts among components.  
* allow the sensing and store-and-forward functions on the edge-controller to focus on what they do well.  
* Increase resilience by forcing clean interfaces and providing restart-on-error.

The controller relies on two "blocks" that provide ActiveMQ and Postgres functionality.  
These are in adjacent repositories and are deployed independently:

Postgresql - https://github.com/bubblesnet/postgresql-11-block

ActiveMQ - https://github.com/bubblesnet/activemq-5_17-block