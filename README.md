General Strategies:
1. Ask for details / assumptions
String questions: uppercase vs lowercase? ASCII vs unicode? White space?

2. First, deal with obvious case
E.g. 2 strings of different lengths cannot be permutation of one another

1. Arrays and Strings

String Manipulation Questions
- Ask if it's a ASCII or Unicode string
ASCII:
Unicode:

Number of Unique characters in ASCII / Unicode

bit vector

Bit Manipulation:
- N * 2 = N << 1
- N * 4 = N << 2
so on and so forth...

- N & 10 = Clearing the last bit
- N & 100 = Clearing the last two bits
so on and so forth...

x ^ 0s = x
x ^ 1s = ~x
x ^ x = 0

x & 0s = 0
x & 1s = x
x & x = x

x | 0s = x
x | 1s = 1s
x | x = x

Two's Complement: need (N+1) bits to store a number with magnitude of N bits

Positive numbers: Append 0 to the beginning (e.g. 5 = 101, append 0 to the beginning = 0101)

Negative numbers: Write down the absolute value of the number in 2's complement, flip all bits, then add 1 (e.g. -5's absolute value is 5, 2's complement representation of 5 = 0101, flip bit = 1010, add one = 1011)

Negative number:
Arithmetic shift >> : Shifting only the value bits
Logical shift: >>> : Shifting the whole number (sign bit + value bits)

For i = 1, 2, 3, ... length of N:
Get i-th bit of N: N & (1 << i) != 0
Set i-th bit of N: N | (1 << i)
Clear i-th bit of N: N & ~(1 << i)
Clear all bits from most significant bit to i (inclusive) of N: N & ( (1 << i) - 1 )
Clear all bits from i to least significatn bit (inclusive) of N: N & ( -1 << (i+1) )

Check if N has exactly one 1: N & (N-1) == 0

Update i-th bit of N:
