---
seo:
  title: Native D Data Types
title: Native D Data Types
weight: 100
layout: page
navigation:
  show: true
---
Vlang inherits all the native D data types.

## Integral Types

D supports the following integral types:


<!---
#+ORGTBL: SEND sample orgtbl-to-gfm
| Type   | Signed? | Bits |
|--------+---------+------|
| <l>    |     <r> |  <r> |
| bool   |      No |    1 |
| byte   |     Yes |    8 |
| ubyte  |      No |    8 |
| short  |     Yes |   16 |
| ushort |      No |   16 |
| int    |     Yes |   32 |
| uint   |      No |   32 |
| long   |     Yes |   64 |
| ulong  |      No |   64 |
-->

<!-- BEGIN RECEIVE ORGTBL sample -->
| Type | Signed? | Bits |
|---|--:|--:|
| bool | No | 1 |
| byte | Yes | 8 |
| ubyte | No | 8 |
| short | Yes | 16 |
| ushort | No | 16 |
| int | Yes | 32 |
| uint | No | 32 |
| long | Yes | 64 |
| ulong | No | 64 |
<!-- END RECEIVE ORGTBL sample -->

{% info %}
Native integral types in D do not support bit vector operations like indexing etc. If you intend to access different bits from a variable using bit-indexing or bit-slicing, it may be a good idea to instead use bit-vector library provided by Vlang.
{% endinfo %}

## Other D Types

The D programming language also supports many other native types including string, floating types and complex numbers.
