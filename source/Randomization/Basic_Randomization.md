---
seo:
  title: Basic Randomization
title: Basic Randomization
weight: 100
layout: page
navigation:
  show: true
---
Vlang aims to replicate SystemVerilog randomization behavior.

To make a Class randomizable, you need to inherit the class from base class `Randomizable`. To randomize given randomizable object, you will need to call function `randomize` on that object.

You also need to mark the elements of the class that you want to be randomized. This is done by adding `@rand` [User Defined Attribute](http://dlang.org/attribute.html)(UDA) to the elements that require randomization.

## The Randomization Mixin

Under the hood, Vlang randomization engine uses  [reflections](http://dlang.org/traits.html) provided by D Programming language to list out all the elements of the class that are tagged with `@rand` UDA. Since code reflections in D is a compile time feature, by default only the compile time type information type of a given object is considered.

{% codeblock lang:d %}
class Foo: Randomizable {
  @rand Bit!32 data;
  @rand Bit!24 addr;
}
class Bar: Foo {
  @rand Bit!12 flags;
}
void main() {
  Foo foo = new Bar();
  // surprize! this will not randomize flags
  foo.randomize();
}	
{% endcodeblock %}


To give polymorphic characteristics to `randomize` function, you are required to `mixin Randomization` into the derived class that you want to randomize.

{% codeblock lang:d %}
class Foo: Randomizable {
  @rand Bit!32 data;
  @rand Bit!24 addr;
}
class Bar: Foo {
  mixin Randomization;
  @rand Bit!12 flags;
}
void main() {
  Foo foo = new Bar();
  // Allright now flags get randomized
  foo.randomize();
}	
{% endcodeblock %}

In fact the Randomization Mixin is overloaded. There is another scenario wherein you must `mixin Randomization`. There would be those odd scenarios where the class you want to make Randomizable is already inheriting from another class. Since D does not support C++ like multiple inheritance, Vlang provides you another syntax to allow you to still make your class Randomizable.

{% codeblock lang:d %}
class Foo {
  Bit!32 data;
  Bit!24 addr;
}
class Bar: Foo, RandomizableIntf {
  // required for implementing RandomizableIntf
  mixin Randomization;
  @rand Bit!12 flags;
}
void main() {
  Foo foo = new Bar();
  foo.randomize();
}	
{% endcodeblock %}

In this code example, the base class Foo has not been declared Randomizable. But the derived class Bar is still randomizable by virtue of implementing RandomizableIntf. In this case too, it is a must to mixin Randomization, which in fact provides the implementation of the RandomizableIntf methods.
