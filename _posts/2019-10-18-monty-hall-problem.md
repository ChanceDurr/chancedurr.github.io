---
layout: post
title: Monty Hall Problem
subtitle: How the Monty Hall problem works
tags: [statistics, probability]
comments: true
---

![monty hall picture](img/montyhall.jpg)

## What is the [Monty hall problem](https://en.wikipedia.org/wiki/Monty_Hall_problem)?

I learned about this problem a couple months back, and in all honesty it mad me so mad.
To make it quick and simple, you have got three doors in front of you. Two of the doors have goats behind them,
and the third door has your dream car. Now, you get to choose a door, let's say you choose door number 3.
Now that you have chosen door 3, you are shown that behind door 1 there is a goat. Now you know that the car is either behind door
number 2 or 3. You are given the option to either keep the door you chose or switch to the other one.

### What should you do?

So know that you have the option to switch doors, _do you_?

The answer is yes.

### Why though?

Initially you have 3 doors in front of you with 1/3 chance of the car being behind each one.
So you choose one of the doors. Now there is a 2/3 chance that the car is behind one of the other two doors.
After you are shown that the goat is behind one of the other ones, the 2/3 chance of the car being behind another door stays the same.
However, now there is only one other door. Now, the door you currently have is still a 1/3 chance that is has the car, however the other door now has a 2/3 chance.
Those are much better odds if you ask me. 

Now, if you're like me and didn't understand how it wasn't a 50/50, you're not alone.

### I can prove it

I used some good ol' fashioned python to check if this was actually true.

[The notebook I used to prove it](https://colab.research.google.com/drive/1uyXmpKeDYO9eShlW64C5XubdfvBmamzg)

Here you can see the creation of the doors and options 'variables'

```
import random
def create():
  # These are the doors
  global doors
  global options
  doors = {
      'door1' : '',
      'door2' : '',
      'door3' : ''
  }

  # These are the options
  options = [
      'car',
      'goat',
      'goat'
  ]
```

This will shuffle the options everytime and assign them to the doors

```
def assign_doors():
  random.shuffle(options) # Shuffle the options around then assign them to doors
  for option, door in enumerate(doors):
    doors[door] = options[option]
```

This next chunk of code is what will simulate someone choosing a door

```
def choose_door():
  choice = random.randint(0, 2) 
  choice_to_door_dict = {
      0 : 'door1',
      1 : 'door2',
      2 : 'door3'
  }

  door_chosen = choice_to_door_dict[choice]

  return door_chosen
```

Now that we have these functions, we can run a simulation, _in this case, 10000 of them_, to see how often we win
if we are not shown one of the doors and don't switch

```
win_count = 0
iterations = 10000
for i in range(iterations):
  assign_doors()
  choice = choose_door()

  if doors[choice] == 'car':
    win_count +=1

print(win_count / iterations, 'as expected')
```

#### The output
`0.3361 as expected`

As you can see, the output is about 1/3, which makes sense.

Now let's run a simulation where you choose a door, then are shown another door with a goat behind it, and decide to switch.

```
win_count = 0
iterations = 10000
for i in range(iterations):
  create()
  assign_doors()
  choice = choose_door()
  d = dict((k, v) for k, v in doors.items() if v == 'goat' and k != choice) # Remove the door that isn't your choice and isn't the car
  doors.pop([f'{key}' for key in d][0])
  choice = [item for item in doors if item != choice][0] # Choose the other door with new information
  if doors[choice] == 'car': # if door has car behind it add 1 to win count
    win_count += 1

print(win_count / iterations)
```

#### The output

`0.6663`

Yup, you're seeing that right. If you switch doors, you win about two-thirds of the time.

## Probability is weird but fun

This problem really got me interested in probabilities. At the same time though, made me hate probabilities.
Hopefully this little brain teaser was fun for you and you can see if your friends or family would switch doors!

If you have any questions feel free to message me on any of my social medias. Thank you!

