# Addition

Category: MISC

Points: xxx

Solves: xxx

>My little brother is learning math, can you show him how to do some addition problems?

### Solution

```py
import time
import random

questions = int(input("how many questions do you want to answer? "))

for i in range(questions):
    a = random.randint(0, 10)
    b = random.randint(0, 10)

    yourans = int(input("what is " + str(a) + ' + ' + str(b) + ' = '))

    print("calculating")

    totaltime = pow(2, i)

    print('.')
    time.sleep(totaltime / 3)
    print('.')
    time.sleep(totaltime / 3)
    print('.')
    time.sleep(totaltime / 3)

    if yourans != a + b:
        print("You made my little brother cry ðŸ˜­")
        exit(69)

f = open('/flag.txt', 'r')
flag = f.read()
print(flag[:questions])
```

It asks us to answer `n` questions for the first `n` chars of the flag. However, we can notice that the delay grows exponentially, and doing more than 8 questions is a pain. Luckily, our question count isn't filtered and we can enter a negative amount of questions to answer. Entering, `-1` allows us to bypass answering questions and gives us the whole flag through python's negative indexing.

![remote server](/images/Addition.png)

### Flag

```n00bz{m4th_15nt_4ll_4b0ut_3qu4t10n5}```