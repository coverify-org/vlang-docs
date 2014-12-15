---
seo:
  title: Constraints
title: Constraints
weight: 90
layout: page
navigation:
  show: true
---
By default, Vlang would randomize any given element of a Randomizable class that has been tagged `@rand`, uniformly across all the possible values of the random variable. Vlang also allows you to add constraints on the randomizable class objects. Constraints are often useful for:
* Keeping a given element within the legal limits
* Describing the relationships between the different elements of the same transaction object

{% codeblock lang:d %}
impport esdl.data;
class Foo: Randomizable {
  Bit!12 addr;
  ubyte data[];
  Constraint! q{
    addr < 32;
    addr[0..2] == 0;
    data.length <= 4;
  }
  void print() {
    import std.stdio;
    writeln("addr: ", addr);
    writeln("data: ", data);
  }
}
void main() {
  Foo foo = new Foo();
  foo.randomize();
  foo.print();
}
{% endcodeblock %}


Constraints in Vlang are implemented as a template class that takes a string as a parameter. This string is parsed at compile time by Vlang and converted into a function that executes a set of BDD equations. These BDD equations are formed based on the contents of the constraint string.

Given the set of constraints, Vlang first creates independent groups of `@rand` elements. The groups are created in such a way that elements of a given group are interrelated, but there is no relation between any element of a given group with any element that belongs to any other group. Individual constraints are also tagged with the group they apply on. Vlang then processes randomization separately on each of these groups.

When a Randomizable object is randomized, Vlang first creates a BDD tree for given set of constraints and the `@rand` elements -- operating sequentially on all the groups, one at a time. Each node of the BDD tree is then annotated (bottom up) with a floating point number that depicts the probability of the two decision branches that originate from the given node. Finally a random number is generated and the BDD tree is navigated comparing the random number against the annotated nodes of the tree. The random number thus decides the path of the BDD that is traversed. The node values on this path are then back-annotated into the various `@rand` numbers in the given object.

