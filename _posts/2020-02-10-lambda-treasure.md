---
layout: post
title: Lambda Treasure
subtitle: My most fun project at Lambda
gh-repo: Team-Nowhere/CS-Build-Week-2
gh-badge: [star, fork, follow]
tags: [graphs, computer science, API, blockchain]
comments: true
---

The [Lambda Treasure project](https://colab.research.google.com/drive/1paReyy2r0PrJuhF355_TdHyx5CPw3yfj) is a combonation of the the things we learned in
the Computer Science portion of [Lambda School](https://lambdaschool.com)

This project includes the use of:

- Blockchain Technology
- Graphs
- Computer Architecture
- API endpoints
- Advanced Algorithms

You can find most of the code [here](https://colab.research.google.com/drive/1paReyy2r0PrJuhF355_TdHyx5CPw3yfj)
on a Google Colab or on our teams [Github Repo](https://github.com/Team-Nowhere/CS-Build-Week-2)

### What is the project about

During the week we were given to work on this, we were supposed to catch as many
Golden Snitches as possible.

In order to catch a Golden Snitch, you have to be able to get to the Dark World.

To go to the Dark World, you need boots, a jacket, and the ability to warp.

To get boots and a jacket, you need to mine Lambda Coin.

To get Lambda Coin you need to buy a name.

To buy a name you need to have 1000 gold.

To get 1000 gold you need to traverse the map and find treasure.

So, we'll go ahead and start there.

## The Map

When it comes to traversing the map, there are a few speed bumps.
Everytime you do something, for example move to another room, there is a
cooldown. If you don't wait for this cooldown and try to move again, you will
stay in the same room and your cooldown will increase. So, as you can imagine
traversing this map can be very time consuming.

There are ways to help with this however.

If you know the ID of the room you are moving to, it will reduce your
cooldown by 50%. This is called **Wise-man**

So, we are going to want to keep track of what rooms are connected and which
direction they are. This will help with our cooldowns a lot.

![Map of the overworld](/img/overworld_map.png)

## Getting a name

Once we have the map traversed, we can work towards getting our name.
The approach I went with when trying to get my name was to just randomly walk
around and find treasure then head back to the shop and sell it. It didn't take
long as the smallest treasure I found was still worth 100 gold.

After getting 1000 gold, I went to Pirate Ry's in room 467. That's when I
bought the name 'Chauncey'

## Abilities

After getting a name, you unlock a whole new world of possibilites. While
traversing the map I noticed a bunch of unique rooms. So I went to each of
them to see what I could do.

Here are some abilities I accrued:

- **Recall**: The ability to bring my self back to room 0. Useful for getting
back to the shop quickly.
- **Dash**: The ability to travel through a straight line of room without having
to cooldown each time.
- **Fly**: Being able to fly instead of walk, it's faster then walking in all
situations except in a cave terrain.
- **Warp**: The ability to warp to an alternate dimension. The Dark World.
- **Carry**: Gives me a friendly ghost to help me carry an item when
I am over-encumbered.

## Traveling

Now that I have multiple means of quick travel. I can start doing some more
advanced stuff. I created a function called `fast_travel` to help me get around
quicker. It uses the abilites in the right situation and uses breadth-first
traversal to find the shortest path to where I want to go.

## Lambda Coin

Time to start getting some coin. I've found out pretty quickly that you could
examine the well. When you do, it gives back a bunch of numbers, binary
specifically.

```python
10000010
00000001
01000110
01001000
00000001
10000010
00000001
01101001
01001000
00000001
10000010
00000001
01101110
01001000
00000001
10000010
00000001
01100100
01001000
00000001
10000010
00000001
00100000
01001000
00000001
10000010
00000001
```

Using a 8-bit computer that we built the previous week, I was able to decipher
this code and have it print out `Mine in room 103`

Now this is some useful information. If you tried to mine in a random room,
you would get a huge cooldown and a message saying `There is no coin here`.

So, I traveled to this room and got the last proof on the blockchain and
submitted a new proof, and got a coin. However, when I tried it again, I got
the message saying `There is no coin here` again. So I went back to the well.
Turns out, each time you mine a coin, the room changes. So I create a function
that would automatically travel to the well, get the room, travel to said
room, then mine a coin and repeat.

Now that we have some coins, we can use them!

## Getting Gear

On the map there is something called the Transmogriphier. When you examine it
It tells you that you need some treasure and a Lambda Coin. So, I went and
gathered some more treasure. Then starting transmogriphying it. Each time it
would give me either boots or a jacket. They all had different tiers though.

The boots all had different speed ratings depending on the tier.

The jackets all had different strength ratings also depending on the tier.

So the boots made my cooldowns less, and the jacket allowed me to carry more.
Now I am faster than ever before!

## The Dark World

Now that I had gear and the ability to warp, I decided to go to the Dark World.
When I warped, I got to a room that was just Darkness. I manually walked around
and couldn't find anything. So I decided it was time to map out the underworld
with our traversal algorithm.

![Dark World Map](/img/dark_map.png)

## The Dark well

The only room in the Dark World, was another well at room 555.

So I examined it and got back some more binary code. This time, it read

`The Snitch is in room 788`

So, I went there, but the room was empty. So I went back to the well.

`The Snitch is in room 842`

That's when I realized there was only one snitch, and everybody was chasing
for it at the same time.

Now I've got to create another algorithm that will do this as fast as
possible, automatically.

## Catching Gold Snitches

When it came to catching the Golden Snitches, the trick was to sit at the well
and wait for someone to grab the snitch. As soon as the well showed the new room
that the snitch was in, it was an all out sprint, finding the shortest path and
using all possible abilites in the right situations so that you got there first.

## The Result

At the end of the week, I ended up with 716 Golden snitches.
I came in second place by just 30.

However, it was a win-win as we both broke the previous record for most golden
snitches caught.

This week long project taught a lot about algorithmic and computational
efficiency as well as teamwork. We all came together to create extremely
efficient algorithms while also taking first place in other categories.