---
seo:
  title: Queue
title: Queue
weight: 80
layout: page
navigation:
  show: true
---
{% info %}
Queues in SystemVerilog tend to be over-used since Systemverilog serves you half-cooked dynamic arrays. For example Systeverilog dynamic arrays require explicit resizing if you want to increase their size. D has a very sound container in dynamic arrays. Dynamic arrays in D have support for auto-resize. One of the strongest features with D arrays is slices.

When you pick a Vlang Queue container, you should consider whether you really need it or would a dynamic array suffice. In most cases it would.
{% endinfo %}

Vlang implements container type Queue and its operation is similar to a Systemverilog Queue container.

{% codeblock lang:d %}
import esdl.data.queue;
void main() {
  import std.stdio;
  Queue!int foo = [1, 7, 2, 9];	// explicit initialization
  foo.pushBack(12);
  foo ~= [1, 2, 3, 4];		// concat a list into
  int bar;
  foo.popFront(bar);
  writeln(foo, " ", foo.length);
}	
{% endcodeblock %}

## Queue Implementation

In Vlang, the `Queue` container is implemented as a ring buffer. The buffer itself is a dynamic array. In addition to the buffer the `Queue` stores the index and the size.
