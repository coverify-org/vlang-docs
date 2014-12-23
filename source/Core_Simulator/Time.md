---
seo:
  title: Concept of Time
title: Concept of Time
weight: 100
layout: page
navigation:
  show: true
---
Like most Discrete Event Simulators, ESDL conceptualizes simulator time in terms of absolute time and delta time. Absolute time is used to model the actual time passed in the system being modelled. The delta time is useful to determine the sequence of events where the time is either indeterminate or when its modelling is inconsequential or otherwise being avoided in order to enhance the simulation performance.

ESDL allows the end-user to specify time precision and time unit at various levels of hierarchy. A set of User Defined Attributes are made available by ESDL to make it possible to specify precision and unit for an Entity or its specific instance.

## Time Precision

Precision is actually a property of a simulator. ESDL allows you to specify precision only as powers of 10. If multiple different precisions are specified at various levels of hierarchy in the simulation model, ESDL picks the least of the given precision values and assigns it to the simulator. The simulator uses the precision value as the least measurable time in the given simulation. So, even if you specify different precision values for different Entities or instances, at run time, the same precision is used across the simulation.

## Time Unit

ESDL allows you to tag a time unit with an Entity and/or an instance. If both an Entity and its instance are tagged the value tagged with the instance overrides. Also if no time unit is specified for a given hierarchy, its value is automatically inherited from the upper hierarchy. Time unit need not be a power of 10.

Time Unit configuration makes it possible to specify time as an absolute number within a given entity. When the timing information is registered with the simulator, the given number is multiplied by the time unit applicable to the given instance of the Entity. As a result, it becomes possible to use the same Entity definition with different units of time in different scenarios.
