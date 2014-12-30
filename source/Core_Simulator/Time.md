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

To help model time, ESDL defines a class `Time`. The class `Time` stores absolute time as a pair of values representing time unit and value. Time unit is stored in terms of `TimeUnit` enum which is defined in the time module.
{% codeblock lang:d %}
protected enum TimeUnit: byte
  {   SEC  = 0,
      MSEC = -3,
      USEC = -6,
      NSEC = -9,
      PSEC = -12,
      FSEC = -15
      }
{% endcodeblock %}

To help specify literal time values, ESDL Time module defines functions that return `Time`. These functions provide an elegant way (thanks to UFCS) to specify a time value. For example `Time(4, TimeUnit.NSEC)` can be specified by simply saying `4.nsec`.

ESDL allows the end-user to specify time precision and time unit at various levels of hierarchy. A set of User Defined Attributes are made available by ESDL to make it possible to specify precision and unit for an Entity or its specific instance.

## Time Precision

Precision is actually a property of a simulator. ESDL allows you to specify precision only as powers of 10. If multiple different precisions are specified at various levels of hierarchy in the simulation model, ESDL picks the least of the given precision values and assigns it to the simulator. The simulator uses the precision value as the least measurable time in the given simulation. So, even if you specify different precision values for different Entities or instances, at run time, the same precision is used across the simulation.

## Time Unit

ESDL allows you to tag a time unit with an Entity and/or an instance. If both an Entity and its instance are tagged the value tagged with the instance overrides. Also if no time unit is specified for a given hierarchy, its value is automatically inherited from the upper hierarchy. Time unit need not be a power of 10.

Time Unit configuration makes it possible to specify time as an absolute number within a given entity. When the timing information is registered with the simulator, the given number is multiplied by the time unit applicable to the given instance of the Entity. As a result, it becomes possible to use the same Entity definition with different units of time in different scenarios.

## Specifying Time Precision and Unit
{% codeblock lang:d %}
class Foo: Entity {
  // ..
}
@timeUnit(1.nsec)
@timePrecision(1.psec)
class Top: Entity {
  // ..
  @timeUnit(10.psec)
  @timePrecision(1.psec)
  Inst!Foo foo;
  // ..
}
void main() {
  simulate!Top("root", 4, 0);
}
{% endcodeblock %}

To specify the time unit or precision, you just have to tag the Entity definitions or the instance declaration with `@timeUnit()` and/or `@timePrecision()` UDPs, as shown in the above code snippet.
