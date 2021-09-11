---
layout: post
author: juanrgar
---

Recently I was presented with the [Counter
Game](https://www.hackerrank.com/challenges/counter-game/problem) from
HackerRank. This problem can be easily solved iteratively in O(n), where *n*
would be the number of turns. This might become computationally expensive with
input numbers that require lots of turns to decide the game. There is an
approach that gets a solution in O(1). Here's how.

## Binary representation is key

The two possible operations the game allows are the following:
* Divide by 2. This is basically shifting the number right one bit.
* Substract the next power of 2. Powers of 2 are all of the form `b100...00`,
  i.e., a single bit set. Thus, this operation consists in removing the most
significant bit set of the number.

## Counting 1s and 0s

Let's use `n = 132` as in the program page. Given the above operations, it
makes sense to represent this number in binary form, i.e., `b10000100`. It's
not a power of 2 because it has more than one bit set; thus, the next operation
would be to substract a power of 2. The next power of 2 is `b10000000`, i.e.,
the most significant bit set followed by zeroes; in decimal, it happens to be
128. The operation result is `b10000100 - b10000000 = b100`; or, as we put it
previously, removing the most significant bit set.

At this point, we have reached a power of 2. It took 1 turn. Basically,
reaching a power of 2 takes `N - 1` turns, where `N` is the number of bits set.
This is because we have to remove all bits set, starting from the most
significant one, until we have just one.

The next operation is to divide by 2: `4 / 2 = 2`. Or, as we put it previously,
just shift the number right one bit, i.e., `b100 >> 1 = b10`. There is still
one more shift required before we reach number 1, which finishes the game. For
number `b100`, it takes 2 turns to get 1, ending the game. In a general sense,
it will take `M` turns, where `M` is the number of trailing zeroes.

With this example, we can see that the total number of turns would be `N - 1 +
M`, i.e., number of bits set minus one, plus number of zeroes after the least
significant bit set. In this example, `n = 132 = b10000100`, it takes 3 turns
to end the game. Since Louise always starts, she will win this game, i.e.,
Louise will play the first and third turns, and Richard would play the second
turn.

This same approach can be applied to the other example given, `n = 6`. In
binary representation, it is `b110`. Thus, we can predict 2 turns, with Richard
winning.

Once we have the number of turns, determining who wins is as easy as checking
if the number is odd or even: odd means Louise wins, and even means Richard
wins. How to determine if a number is odd or even is as simple as checking if
the least significant bit is set or not.

## Going a bit beyond

The above solution requires two counts. We can improve the solution and have
only one count. One might think that counting only zeroes is not suitable
because leading zeroes are moved, so would first have to negate the number, ...
Well, we can count only bits set. We can see that, in binary form, substracting
1 from a number means flipping all zeroes before the least significant bit set;
then, that bit is also flipped and becomes 0. As an example, `4 - 1 = b100 - 1
= b011`. If we substract 1 from the initial number, trailing zeroes become
ones, and one bit set is removed. That is, the number of bits set is the total
number of turns.

## Python code

```python
def counterGame(n):
    nb = bin(n - 1)
    n1 = nb.count('1')
    if n1 & 1:
        return 'Louise'
    else:
        return 'Richard'
```
