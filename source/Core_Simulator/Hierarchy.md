---
seo:
  title: Entity and Hierarchy
title: Entity and Hierarchy
weight: 100
layout: page
navigation:
  show: true
---

ESDL uses object composition to model hardware hierarchy. The hierarchy is built and elaborated before the actual simulation starts.

ESDL provides a template wrapper construct `Inst` to instantiate an Entity inside another entity. Though the use of `Inst` is optional, it makes sure that the hierarchical instance is not modified by accident during the simulation run.

Static type information is used by ESDL to automatically build and elaborate the hierarchy. The function `elaborate` is actually a template function that looks at the different components of the top level Entity and then recursively elaborates each of the components in the hierarchy.

## The Elaboration Mixin

In some situations you might want to create polymorphic Entities and use one of the derived models. A specific use case may be to dynamically choose between loosely timed and approximately timed model at the time of elaboration. Automatic elaboration process in Vlang would want to automatically instantiate each of the components in the hierarchy. But to support user customization, Vlang allows manual intervention through the `doBuild` function. You can define a function by name `doBuild` in a given entity and this function would be called before the automatic elaboration kicks in for the entity. If certain components of the hierarchy are instantiated by the `doBuild` function, the automatic elaboration process will not try to instantiate these components.

Now consider the scenario where you have instantiated a derived Entity for a given component and the type specification of this component points to the base entity. In such a scenario, the automatic elaboration process would use compile time reflections (a feature provided by the D language) to find the subcomponents of given base component. If the derived Entity that was actually instantiated holds some extra components, these would be ignored by the automatic elaboration process. Since this would result in a major disruption in the elaboration process, and such a disruption is difficult to fix by `doBuild` function, Vlang provides another option. Vlang allows you to add polymorphism to the automatics elaboration process by just adding `mixin Elaboration;` to the derived Entities. Once this mixin is added to the derived entity, the automatic elaboration process would magically be able to look at any extra component that the derived Entity type would have defined and would elaborate all such components in addition to any component defined in the base Entity.

In some other situations, it may not be possible for a user to inherit from the base class `Entity`. Unlike C++, the D programming language does not allow multiple inheritance and takes the `interface` approach (much like Java). This gives rise to use cases where you may want to inherit from some other class and still want to make the resulting class an `Entity`. In such a scenario, you could inherit from an interface defined by Vlang named `EntityIntf`. Since the interface does not include implementation, in this case too, you will have to `mixin Elaboration;` in your entity definition. The `Elaboration` mixin is overloaded and would automatically make out from the context it is being used in.
