# Raspberry Pi Foundation Python Style Guide

Guidelines for writing Python code in learning resources

Largely based on guidelines in [PEP8](https://www.python.org/dev/peps/pep-0008)

## Why?

Good Python code should be easy to read, explicit not implicit, and things should be well named.

## But ... pedagogy

Sometimes it's appropriate to neglect better coding practices to accommodate for younger or less experienced programmers. This is ok. Some exercises start with simpler code and build up explaining new concepts, other exercises only use the simpler code, as they are aimed at young people or beginners and only basic concepts are used.

However, much of this guide is regarding presentation rather than actual use of code. Following the presentation guidelines should be encouraged.

## Good and Bad examples

Good Python code:

```python
import random
from time import sleep

def number_is_over_ten(n):
    return n > 10

numbers = [1, 5, 7, 11, 21]
number = random.choice(numbers)

for n in range(number):
    if number_is_over_ten(n):
        print("%s is over ten" % n)
    else:
        print("%s is not over ten" % n)
```

Bad Python code:

```python
import os, time, random, RPi.GPIO as GPIO, numpy as n, sys, picamera as dave
from pygame import *
def Bob(): print "Hello"
def checkIt(thing):
  if thing>0:
    if thing+1:
      return 1
myVAR=1
while myVAR<10:
  print myVar
  if checkIt(myVAR):print("ok")
  else:print("not ok")
  i=i+1
```

## Python 2 or 3

Python 3 should be the target version.

If a library is unavailable with Python 3, it should be reported to be investigated for porting. If Python 2 is used then code should be Python 3 styled where possible (e.g. `print("Hello world")` rather than `print "Hello world"`) as this still works in both.

## Imports

- Each import should be on a new line

    ```python
    import time
    import random
    ```

    not:

    ```python
    import time, random
    ```

- Line space between groups of imports if many imports are used

    ```python
    import time
    import random

    import picamera
    import RPi.GPIO

    import pygame.mixer
    import pygame.locals
    ```

- `from module import *` should be avoided

    Imports should be explicit, not implicit. Consider the example:

    ```python
    from module_a import *
    from module_b import *

    some_function()
    ```

    It is unclear whether `some_function` was defined in `module_a` or `module_b`.

    ```python
    from module_a import some_function, some_other_function
    from module_b import another_function

    some_function()
    ```

    Now it is clear `some_function` belongs to `module_a`.

    Alternatively, import the whole module so all functions are namespaced:

    ```python
    import module_a
    import module_b

    module_a.some_function()
    ```

    This example also makes it clear where the function comes from.

- Specific imports are preferred

    Consider the example:

    ```python
    from picamera import PiCamera
    ```

    While this is longer than simply `import picamera`, it allows following references to `PiCamera` to be shorter and less repetitive.

    ```python
    from picamera import PiCamera

    with PiCamera() as camera:
        ...
    ```

    is better than:

    ```python
    import picamera

    with picamera.PiCamera() as camera:
        ...
    ```

    But sometimes even if only one function is used, it's better left namespaced.

    ```python
    from time import sleep

    sleep(1)
    ```

    is ok, and preferable.

    ```python
    import time

    time.sleep(1)
    ```

    is ok, but less preferable.

    Whereas

    ```python
    import random

    numbers = [1, 5, 7]

    number = random.choice(numbers)
    ```

    is fine, but

    ```python
    from random import choice

    numbers = [1, 5, 7]

    number = choice(numbers)
    ```

    is less clear.

- Renaming an import is ok, but keep it sensibly named.

    ```python
    import numpy as np
    ```

    is ok.

    ```python
    import time as sleep
    ```

    is confusing.

- Avoid unnecessary renaming

    ```python
    import RPi.GPIO as GPIO
    import mcpi.minecraft as minecraft
    ```

    These are unnecessary renames as the module reference is being renamed to what it is already named. The (easier) preferred method is:

    ```python
    from RPi import GPIO
    from mcpi import minecraft
    ```

## Case

Case should adhere to the following style:

| **Type**                  | **Case**   | **Examples** |
|---------------------------|------------|--------------|
| Variables and all objects | Snake case | `led`, `elec_hi_snare`|
| Functions                 | Snake case | `setup_gpio` |
| Class names               | Title case | `Dog`, `GameWindow`, `GameOfLife` |
| Constants (variables intended not to be changed) | All caps   | `BLACK`, `WHITE`, `HEIGHT`, `WIDTH` |

## Magic numbers

Magic numbers are those used with no explanation. Variables should be used instead.

Bad:

```python
GPIO.setup(17, GPIO.IN, GPIO.PUD_UP)
GPIO.wait_for_edge(17, GPIO.FALLING)
```

Good:

```python
button = 17
GPIO.setup(button, GPIO.IN, GPIO.PUD_UP)
GPIO.wait_for_edge(button, GPIO.FALLING)
```

This means if the button pin is changed, it only needs changing in one place.

## Unused variables

Unused variables should be removed or corrected. They make code hard to read, and sometimes misleading. For example:

```python
def setup_button(pin):
    GPIO.setup(17, GPIO.IN)

setup_button(13)
```

The last line here claims to set up pin 13. The function takes `pin` as an argument but ignores it and sets up pin 17 regardless of what was passed in.

The code should read:

```python
def setup_button(pin):
    GPIO.setup(pin, GPIO.IN)

setup_button(13)
```

## Indentation

- Spaces not tabs

- Four spaces per tab.

    You don't have to hit space four times - you should configure your editor to insert four spaces when you hit the tab key.

    Example:

    ```python
    for i in range(10):
        if n < 2:
            return False
        if n == 2:
            return True
        if n % 2 == 0:
            return False
        for i in range(3, int(n**0.5) + 1, 2):
            if n % i == 0:
                return False
        return True
    ```

## Spaces around operators

- A single space should be used on each side of an operator for readability.

    Good:

    ```python
    a = 1
    b = 2
    c = a + b
    print(c % 3 == 0)
    ```

    Bad:

    ```python
    a=1
    b=2
    c=a+b
    print(c%3==0)
    ```

- Sometimes spaces can be left out to aid readability, for example in this case spaces would usually be around the `*` multiplication operator but can be left out for clarity of priority:

    ```python
    x*x + y*y
    ```

    is better than:

    ```python
    x * x + y * y
    ```

-  Avoid extra spaces immediately inside parentheses, brackets or braces:

    Bad:

    ```python
    call (1)
    dct [1]
    dct[ 1 ]
    ```

    Good:

    ```python
    call(1)
    dct[1]
    dct[1]
    ```

-  Also before a comma or colon:

    Bad:

    ```python
    if x == 4 :
        print(x , y)
    ```

    Good:

    ```python
    if x == 4 :
        print(x, y)
    ```

- Multiple spaces around operators should not be used unless necessary for alignment aiding readability.

    ```python
    a = 10
    b = 16
    angle = 2.51
    ```

    is fine

    ```python
    a     = 10
    b     = 16
    angle = 2.51
    ```

    is unnecessary.

    However, in this case

    ```python
    on = '10101010'
    off = '01010101'
    ```

    The misalignment makes comparison difficult and

    ```python
    on  = '10101010'
    off = '01010101'
    ```

    is better.

- The same applies to nested lists and such:

    ```python
    pixels = [[r, g, r], [g, g, g], [r, g, r]]
    ```

    is ok, but

    ```python
    pixels = [
        [r, g, r],
        [g, g, g],
        [r, g, r],
    ]
    ```

    is more readable as it's presented practically.

## Line length

Line length should not exceed 79 characters and usually should be shortened or rearranged to fit this limit to aid readability and avoid horizontal scrolling or line wrapping.

Bad:

```python
result = some_function_that_takes_arguments('foo', 'bar', 'spam', 'eggs', 'alice', 'bob')
```

Good:

```python
result = some_function_that_takes_arguments(
    'foo', 'bar', 'spam', 'eggs', 'alice', 'bob'
)
```

or:

```python
result = some_function_that_takes_arguments(
    'foo',
    'bar',
    'spam',
    'eggs',
    'alice',
    'bob'
)
```

## `with` keyword

The `with` keyword should be used where possible, for example when opening files. This ensures cleanup is completed.

Bad:

```python
f = open('file.txt', 'r')
text = f.read()
print(text)
f.close()
```

Good:

```python
with open('file.txt', 'r') as f:
    text = f.read()
    print(text)
```

## Booleans

xxx

## Exceptions

When catching exceptions, mention specific exceptions whenever possible instead of using a bare `except` clause.

Bad:

```python
try:
    a = dct['a']
except:
    a = lookup('a')
```

Good:

```python
try:
    a = dct['a']
except KeyError:
    a = lookup('a')
```

A bare `except` clause will catch `SystemExit` and `KeyboardInterrupt` exceptions, making it harder to interrupt a program with `Ctrl-C`, and can disguise other problems.

## Comments - and naming things

Use comments to explain code, but consider the following:

 - Are you only using the comment to repeat what the code says? Don't do that. Example:

    ```python
    import time # import the time module

    a = 1 # assign the value 1 to a
    ```

- Would a well named variable help?

    Bad:

    ```python
    GPIO.setup(17, GPIO.IN) # pin 17 is the button
    ```

    Good:

    ```python
    button = 17
    GPIO.setup(button, GPIO.IN)
    ```

    This also means if you change the button pin you only change it once in the whole program

- Would a function be better?

    Bad:

    ```python
    # check if the number is prime
    for i in range(3, int(101**0.5) + 1, 2):
        if n % i == 0:
            print("no")
            break
    print("yes")
    ```

    Good:

    ```python
    def is_prime(n):
        ...

    n = 101
    if is_prime(n):
        print("yes")
    else:
        print("no")
    ```

- If a function is used, would a better function name help? Examples:

    Bad:

    ```python
    # check if n is prime
    if my_function(n):
        print("yes")
    ```

    Good:

    ```python
    if is_prime(n):
        print("yes")
    ```

## If/Else

Dictionaries should be used in place of large if/else blocks

## Duplication

Don't repeat yourself.
