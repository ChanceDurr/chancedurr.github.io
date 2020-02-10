---
layout: post
title: Lambda Treasure
subtitle: My most fun project at Lambda
gh-repo: github.com/Team-Nowhere/CS-Build-Week-2
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

### What is the project about

During the week we were given to work on this, we were supposed to catch as many
Golden Snitches as possible. :trophy:

In order to catch a Golden Snitch, you have to be able to get to the Dark World.

To go to the Dark World, you need boots, a jacket, and the ability to warp.:black-circle:

To get boots and a jacket, you need to mine Lambda Coin.:shirt::shoe:

To get Lambda Coin you need to buy a name.

To buy a name you need to have 1000 gold. :moneybag:

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
