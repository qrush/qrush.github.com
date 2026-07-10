---
layout: post
title: "BigDecimal#round always, please"
category: journal
original_url: https://quaran.to/bigdecimal-round-always-please
---

TIL that 27 digits of significance are possible in Ruby...why not? Here's something that should just be `0.009`:

```ruby
>> 19120.01.to_d / 2124445
=> 0.9000002353555869886017289e-2
```

Say you were estimating how many significant digits this way:

```ruby
>> (19120.01.to_d / 2124445).to_s.split(".")[1].size
=> 27
```

This digit estimation works great for small, cleaner divisions. But it falls apart, since floating point math never truly escapes us. As usual, `#round` to the rescue:

```ruby
>> (19120.01.to_d / 2124445).round(5)
=> 0.9e-2
>> (19120.01.to_d / 2124445).round(5).to_s
=> "0.009"
```

Just a friendly reminder that you'll probably, always, want to `#round` your `BigDecimal`'s when dividing. Thank me later.

---

¤ [@qrush](https://twitter.com/qrush)
