---
seo:
  title: Bit and Logic Vectors
title: Bit and Logic Vectors
weight: 90
layout: page
navigation:
  show: true
---
Vlang implements Bit and Logic vectors in `bvec` module.

{% codeblock lang:d %}
import esdl.data.bvec;
import std.stdio;
void main() {
  // Signed Bit Vector
  Bit!24 foo = 12;
  foo[12] = 1;	       // setting index
  if(foo[2]) {
    // Special syntax for assigned X and Z logic values
    // String is parsed at compile time
    Logic!32 bar = hex!"0Z0Z_0X0X";
    writeln(bar);
  }
}

{% endcodeblock %}

Bit and Logic vectors support most bit, logic and arithmetic operations and a compatible with native D integral types.

Additionally Bit/Logic vectors also support bit-slice operation.

{% codeblock lang:d %}
import esdl.data.bvec;
import std.stdio;
void main() {
  // Signed Bit Vector
  Bit!32 foo = 0xDEADBEEF;
  auto bar = foo[0..16];
  auto frop = foo[16..32];
  writeln("bar is: ", bar);
  writeln("frop is: ", frop);
}
{% endcodeblock %}

{% info %}
The slice operation in Vlang behaves very different from similar operation in Verilog or VHDL. First of all the slice operation always maintains the endian-ness of the input. Secondly, the first index of the slice must always be the LSB and the second index MSB. The range of the slice is kept closed on the LSB but open on the MSB -- that means the MSB will not be included in the result.

Lastly both the LSB and the MSB are allowed to be variables (need not be compile time constants). The result from the slice operation would have the same type as the bit-vector it is applied on.

The thinking about such behavior of the slice operation is to maintain consistency with how slices work with D arrays.
{% endinfo %}

{% info %}
To change the endianness of a bit-vector, Vlang provides with the method `reverse`.
{% endinfo %}
