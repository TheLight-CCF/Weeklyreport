## Methods for Limits

1. 尝试使用替换法
2. 若替换出现 $\frac{b}{\pm \infty}$ 时，该极限为 0，即：

$$
\frac{b} {\pm \infty} = 0
$$

3. 替换后形式为 $\frac{b}{0}$ 且 $b \neq 0$ 时，说明函数有垂直渐近线，这时要讨论它的双侧极限。
4. 若不是上述情况，那只有 $\frac{0}{0}$ 的情况，首先看它是否为导数形式，如果可以将形式改为：

$$
\lim_{h \to 0} \frac{f(x + h) - f(x)}{h}
$$

此时极限为 $f^{\prime}(x)$，也可以使用洛必达法则。

5. 若极限存在根号，考虑分子有理化或分母有理化。
6. 若有绝对值，化为分段函数后再处理。
7. 利用不同函数特性解决问题，“无穷小”意味着趋于0，“无穷大”意味着趋于 $\pm \infty$；以下是关于多项式，三角函数，指数函数，对数函数之总结：

**(1)多项式和多项式型函数**

+ 一般方法：尝试使用因式分解，将公因式约掉；利用事实：

$$
\lim_{x \to \infty} = \frac{C}{x^n} = 0
$$

+ 大讨论，最大次数的项决定该极限的值。

以下用两个例题L1，L2来说明这两种方法。

**L1:** 
$$
\lim_{x \to \infty} \frac{3x^3 - 1000x^2 + 5x - 7}{3x^3} \\
= \lim_{x \to \infty} (1 - \frac{1000}{x} + \frac{5}{3x^2} - \frac{7}{3x^3}) \\
= \ \ \ 1
$$
**L2:**
$$
\lim_{x \to \infty} \frac{(x^4 + 3x - 99) (2 - x ^5)}{(18x^7 + 9x^6 - 3x^2 - 1)(x + 1)} \\
= \lim_{x \to \infty}\frac{(\frac{x ^ 4 + 3x - 99}{x^4} \cdot x^4) (\frac{2 - x^5}{- x^5} \cdot (-x^5))}{(\frac{18x^7 + 9x^6 - 3x^2 - 1}{18x^7} \cdot (18x^7))(\frac{x + 1}{x} \cdot x)} \\
= \lim_{x \to \infty} \frac{(1 + \frac{3}{x^3} - \frac{99}{x^4})(\frac{2}{(-x^5)} + 1)}{(1 + \frac{1}{2x} - \frac{1}{6x^5} - \frac{1}{18x^7})(1 + \frac{1}{x})}\times \frac{(x^4)(-x^5)}{(18x^7)(x)} \\
= \frac{(1 + 0 - 0) (0 + 1)}{{(1 + 0 - 0 - 0)(1 + 0)}} \times \lim_{x \to \infty} -\frac{x}{18} \\
= - \infty
$$
**(2)三角函数和反三角函数：**

+ 一般方法：记住图像和特点。
+ 小讨论：当 $A$ 很小时， $sin(A)$ 与 $A$ 的值相近，有如下事实：

$$
\lim_{x \to 0} \frac{sin(小数)}{同样的小数} = 1(tan函数也同理)
$$

可以化为 $\frac{sin(A)}{A} \times A ， tanA$ 也可用；但 $cosA$ 不可用，因为当 $x \to 0$ 时，$\cos{x} \to 1$

**L3：**
$$
\lim_{x \to 0} \frac{ \sin^3{2x} \cos{5x^{19}} }{x \tan{5x^2}} \\
= \lim_{x \to 0} \frac{[\frac{sin(2x)}{2x} \times 2x]^3 \times cos(5x^{19})}{x [\frac{ \tan{5x^2}}{5x^2} \times 5x^2]} \\
= \lim_{x \to 0} \frac{(\frac{ \sin{2x}}{2x}) ^ 3 \cos{5x^{19}}}{\frac{\tan{5x^2}}{5x^2}}\times \frac{8x^3}{5x^3} \\
= \lim_{x \to 0} 1 \times \frac{8}{5} = \frac{8}{5}
$$

+ 大讨论：对 $\sin 、 \cos $，可利用 $\lvert \sin(any) \rvert \leq 1$ 和 $\lvert \cos(any) \rvert \leq 1$，可以用此与三明治定理一起使用；另外有其他特性：

$$
\lim_{x \to \infty } tan^{-1}{x = \frac{\pi}{2}}\ \  和\ \ \lim_{x \to -\infty}tan^{-1}(x) = - \frac{\pi}{2}
$$

**L4：**
$$
\lim_{x \to \frac{\pi}{2}}\frac{\cos{x}}{x - \frac{\pi}{2}} \\
令 t = x - \frac{\pi}{2}，则x = t + \frac{\pi}{2} \\
故 原式 = \lim_{t \to 0}\frac{cos(t + \frac{\pi}{2})}{t} \\
由于 cos(x + \frac{\pi}{2}) = -sin(x) \\
所以:cos(t + \frac{\pi}{2}) = -sin(t) \\
由原式得: \lim_{t \to 0}\frac{cos(t + \frac{\pi}{2})}{t} = \frac{- \sin{t}}{t} = -1 \\
证毕
$$
**(3) 指数函数**

+ 一般方法：记住 $y = e^x$ 的图像和 2 个重要极限：

$$
\lim_{h \to 0} (1 + hx)^{\frac{1}{h}} = e^x 和 \lim_{n \to \infty} (1 + \frac{x}{n})^{n} = e^x
$$

**L5：**
$$
\lim_{h \to 0}(1 - 5h^3)^{\frac{2}{h^3}} \\
= \lim_{h \to 0}((1 - 5h^3)^{\frac{-10}{-5h^3}}) \\
= \lim_{5h^3 \to 0}((1 - 5h^3)^{\frac{1}{-5h^3}})^{-10}
= e^{-10}
$$

+ 小讨论： $e^0 = 1$，可用 "1" 替代，极限中有和或差时，考虑洛必达法则，或导数的定义。

**L6：**
$$
\lim_{x \to \infty} \frac{2x^2 + 3x - 1}{e^{\frac{1}{x}}(x^2 - 7)} \\
= \lim_{x \to \infty} \frac{1}{e^{\frac{1}{x}}} \times \frac{(\frac{2x^2 + 3x - 1}{2x^2})2x^2}{\frac{x^2 - 7}{x^2} \cdot x^2} \\
= \lim_{x \to \infty} \frac{1}{e^{\frac{1}{x}}} \times \frac{(1 + \frac{3}{2x} - \frac{1}{2x^2})}{(1 - \frac{7}{x^2})} \times \frac{2x^2}{x^2} \\
= \frac{1}{1} \times \frac{1 + 0 - 0}{1 - 0} \times 2 = 2
$$
**L7：**
$$
\lim_{h \to 0} \frac{e^h - 1}{h} \\
= \lim_{h \to 0} \frac{e^{x + h} - e^x}{h} \\
所以当f(x) = e^x 时，f^{\prime}(x) = \lim_{h \to 0} \frac{e^{x + h} - e^x}{h} = e^x \\
当 x = 0 时，满足题目条件，故 \lim_{h \to 0} \frac{e^h - 1}{h} = e^x = 1 
$$

+ 大讨论：两个重要极限和一个事实

$$
\lim_{x \to \infty} e^x = \infty \ \ \ 和 \ \ \ \lim_{x \to -\infty}e^x = 0 \\
\lim_{x \to \infty} \frac{多项式}{e^x} = 0
$$

**(4)对数函数**

+ 一般方法：记住 $\ln{x}$ 的图像及对数的运算法则
+ 小讨论：

$$
\lim_{x \to 0^+} \ln{x} = -\infty \\
\lim_{x \to 0^+} \ln{x} \cdot x^a = 0
$$

+ 大讨论：

$$
\lim_{x \to \infty} \ln{x} = \infty \\
当 a > 0 时: \lim_{x \to \infty} \frac{\ln{x}}{x^a} = 0 
$$

+ 在 ” $1$ “ 附近的情况： $\ln{1} = 0$ 或 考虑洛必达法则.

8. 当”走投无路“时，考虑使用洛必达法则，在使用 ” $l'H$ “ 之后总会得到一个新的极限，此时，考虑以上任何方法或再次使用洛必达法则。
