---
layout: post
title: "JSON is our standard gauge railway"
category: journal
original_url: https://quaran.to/json-is-our-standard-gauge-railway
---

![via Linda Hall Library](/images/journal/json-is-our-standard-gauge-railway/01-untitled.png)

*via [Linda Hall Library](https://railroad.lindahall.org/essays/rails-guage.html)*

I used to think for a long time my job as a programmer was mostly about translating numbers or strings, into different numbers or strings. My job looked something this:

```
       +-------+      Data      +-----------+      Data      +-----------+ 
       | Human |   --------->   |   Metal   |   --------->   |  Human(s) |
       +-------+      Code      +-----------+      Code      +-----------+
```

Roughly speaking:

> 💡
>
> **Human** inputs **Data** (via **Code)**; stored in **Metal.** Eventually this **Data** is retrieved (via **Code**), and then shown to more **Humans**.

Sometimes the **Metal** has many loops, it can have little H→M→H loops in itself too. It's turtles all the way down!

For most applications we are building a communication system between **Humans**. The **Data** asked for and retrieved changes rapidly, the **Code** shifts in waves depending on the velocity of those (also **Humans** with real biases) working on it. The **Metal** (hopefully) shifts glacially. Unless, of course:

![This never gets old.](/images/journal/json-is-our-standard-gauge-railway/02-untitled.png)

*This never gets old.*

**Humans** and **Code** are going to vary drastically from system to system. The **Data** though: chances are it's going to be JSON at some point in the process. JSON as the major **Data** transport format is here to stay, possibly for the rest of our careers. Yes, there have been precursors: SGML, CSV, XML. Upstarts like YAML (*gasp!*) and [MsgPack](https://msgpack.org/index.html) continue to challenge the status quo, but only JSON remains.

I'm certain that 10, 20, 30+ years from now, JSON will not only still exist, but will continue to be the [standard gauge](https://en.wikipedia.org/wiki/Track_gauge_in_North_America) for all data. What can you do on a daily basis to make sure that you know how to best work with our industry's standard gauge? Here's what I'm doing:

1. Discover what standard tools exist for JSON ([json_pp](https://www.systutorials.com/docs/linux/man/1-json_pp/) on every OSX for example)
2. Learn and use [jq](https://stedolan.github.io/jq/) to slice and dice JSON data
3. Implement new static configuration in JSON wherever possible
4. Prefer databases that have [native JSON types](https://www.quackit.com/json/tutorial/list_of_json_databases.cfm)
5. Know your language(s)' JSON libraries APIs well (and why they can be slow!)

I'm sure that I'm missing more tools here, and I'd love to know what they are ([please @ me](https://twitter.com/qrush)). Also if you're reading this in 2050 and we aren't writing JSON, please tell me why; and how any of us managed to get through the year 2020.

---

¤ [@qrush](https://twitter.com/qrush)
