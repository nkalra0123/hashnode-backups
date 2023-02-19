# Java >>> vs >>

**>> is arithmetic shift right  or Signed right shift**

For example: -2 represented in 8 bits would be 11111110 (because the most significant bit has negative weight). Shifting it right one bit using arithmetic shift would give you 11111111, or -1.


**>>> is logical shift right or Unsigned right shift **

Logical right shift, however, does not care that the value could possibly represent a signed number; it simply moves everything to the right and fills in from the left with 0s. Shifting our -2 right one bit using logical shift would give 01111111



```
jshell> int x = -1;
x ==> -1

jshell> int y = x>>>1;
y ==> 2147483647

jshell> int z = y >>>1;
z ==> 1073741823

jshell> int x = -1;
x ==> -1

jshell> int y = x>>1;
y ==> -1

jshell> int z = y>>1;
z ==> -1
```

