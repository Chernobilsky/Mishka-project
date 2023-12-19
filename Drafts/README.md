`change_the_color_and_display.ipynb` - cells 9 and 10 show some experiments with `uint8`

**Speaking of cell 9**, `255*x` and `255-x` are not quite the same, but usually they are very close to each other, which explains the effect:

https://chat.openai.com/share/e2fbfb75-e0e3-418a-a882-bdcdc1ab541e

More specifically, it looks like `255*x` (modulo 256 since this is uint8) is `1+255-x` (modulo 256, so `x == 0` is a special case as `255+1 == 0` (modulo 256))

If one wants to prove this by induction, one can start with observing `255*0 (mod 256) == 256-0 (mod 256) == 0` and
`255*1 (mod 256) == 256-1 (mod 256) == 255`, and then one can assume `255*x (mod 256) == 256-x (mod 256)` and consider `x+1`.

Then one should just observe that `255*x + 255 (mod 256) == 255*x - 1 (mod 256)`.

**Speaking of cell 10:**

```python
>>> def f(x, k):
...     return (x, (k*x)%256)
...
>>> [f(x, 255) for x in x_values]
[(0, 0), (10, 246), (20, 236), (30, 226), (40, 216), (50, 206), (60, 196), (70, 186), (80, 176), (90, 166), (100, 156), (110, 146), (120, 136), (130, 126), (140, 116), (150, 106), (160, 96), (170, 86), (180, 76), (190, 66), (200, 56), (210, 46), (220, 36), (230, 26), (240, 16), (250, 6)]
>>> [f(x, 258) for x in x_values]
[(0, 0), (10, 20), (20, 40), (30, 60), (40, 80), (50, 100), (60, 120), (70, 140), (80, 160), (90, 180), (100, 200), (110, 220), (120, 240), (130, 4), (140, 24), (150, 44), (160, 64), (170, 84), (180, 104), (190, 124), (200, 144), (210, 164), (220, 184), (230, 204), (240, 224), (250, 244)]
```
