# Requirements

## Functional
* Provide automated and manual environmental control of an enclosed deep-water-culture hydroponics setup
* Provide notification of out-of-range events
* Provide near-realtime vision of the crop
* Generate a stream of environmental time-series data that can be correlated post-harvest with plant growth
* Automate as much of the entire seed to harvest pipeline as possible
* Provide manual control of dispensable liquids for control of water level, pH level and other nutrient levels
* Odor control
* All controllable components (light, heat ...) can be manually controlled from the user interface

## Non-functional
* As little physical intervention as possible
* Once collected, data is never lost
* Standard-of-care authentication
* Encryption in flight and at rest
* Graceful degradation - no component in the chain should crash/blow-up just because another component
  has failed or bogged down.

