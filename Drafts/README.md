`change_the_color_and_display.ipynb` - cells 9 and 10 show some experiments with `uint8`

Speaking of cell 9, `255*x` and `255-x` are not quite the same, but usually they are very close to each other, which explains the effect:

https://chat.openai.com/share/e2fbfb75-e0e3-418a-a882-bdcdc1ab541e

More specifically, it looks like `255*x` (modulo 256 since this is uint8) is `1+255-x` (modulo 256, so `x == 0` is a special case as `255+1 == 0` (modulo 256))

If one wants to prove this by induction, one can start with observing `255*0 (mod 256) == 256-0 (mod 256) == 0` and
`255*1 (mod 256) == 256-1 (mod 256) == 255`, and then one can assume `255*x (mod 256) == 256-x (mod 256)` and consider `x+1`.

Then one should just observe that `255*x + 255 (mod 256) == 255*x - 1 (mod 256)`.
