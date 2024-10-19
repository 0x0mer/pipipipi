---
layout: default
title: ""
---
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/katex.min.css">
<script src="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/katex.min.js" defer></script>
<script src="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/contrib/auto-render.min.js" defer
onload="renderMathInElement(document.body, {
    delimiters: [
    {left: '$$', right: '$$', display: true},  // Block math
    {left: '$', right: '$', display: false},   // Inline math
    {left: '\\[', right: '\\]', display: true} // Block math (LaTeX-style)
    ]
});"></script>

# Will we ever know if $\pi^{\pi^{\pi^{\pi}}}$ is an integer?

I stumbled upon Matt Parker's incredible video: [Why π^π^π^π could be an integer](https://www.youtube.com/watch?v=BdHFLfv-ThQ), and it really piqued my interest. My initial thoughts were, "Well, that's just nonsense. Just calculate it to a few digits after the decimal place", which is what I guess everyone who saw that video thought. But this question is, in some sense, at the limit of what is feasible to compute, and I find that interesting. On the one hand, it's a simple arithmetic calculation, but on the other, it just as well may be that we will never be able to compute it.

After understanding that this may not be feasible, the interesting question then is, well, how many digits of $\pi$ do you need? And this is what I intend on answering on this page.

First, let's start by denoting $\alpha \coloneqq \pi^{\pi^{\pi}}$. In order to know whether $\pi^{\pi^{\pi^{\pi}}}$ is an integer, we would like to calculate $\pi^\alpha$ to some degree of precision, where $\alpha \approx 10^{18}$ is a huge number.

Let's try to generalize a tiny bit. Let's look at two positive real values $a,b\in \mathbb{R}_+$ and try to calculate $a^b$ to some degree of precision. Recall that in our case both may be irrational ($a=\pi$ is for sure, and $b=\pi^{\pi^{\pi}}$ may be as well).

Say we only use the first $d$ digits (in decimal) of $b$. That is, instead of $b$, we are actually raising $a$ to the power of $\tilde{b} \coloneqq b-\varepsilon$, where $\varepsilon \approx 10^{-d}$. Always keep in mind that in our case $b$ is ridiculously large and $\varepsilon$ is ridiculously tiny.

Now, we want to know how far this is from the actual result. That is, we would like to bound the following expression:

\\[
    |a^b - a^{b - \varepsilon}| < E
\\]

We could choose $E$ to be something reasonable, say, $E=10^{-2}$. That way, if we calculate $a^{b-\varepsilon}$, and see that it is not within a distance of $0.01$ of an integer, then we would know that $a^b$ (or $\pi^\alpha$, in our case) is not an integer either.
If the calculated value is within the range of an integer (which implies that we can't prove that $a^b$ is not an integer), then we can just choose $E$ to be smaller, but we will shortly see that this doesn't really matter anyway.

The small caveat of this method is that we can only prove that $\pi^{\pi^{\pi^\pi}}$ is not an integer. That is, on the odd chance that this number actually **is** an integer, this method will not be able to prove it.

Essentially, this method tries to bound $a^b$ inside an $\varepsilon$-ball that does not contain an integer. But, sadly, if $a^b$ is an integer, then **any** $\varepsilon$-ball around it will always contain an integer - itself.

Anyways, let's start crunching away:

\\[
    |a^b - a^{b - \varepsilon}| < E
\\]

\\[
    a^b(1 - a^{-\varepsilon}) < E
\\]

\\[
    (1 - a^{- \varepsilon}) < a^{-b}E
\\]

\\[
    a^{- \varepsilon} > 1 - a^{-b}E
\\]

\\[
    - \varepsilon > \log_a(1- a^{-b}E)
\\]

Now, recall the Taylor series expansion of $\log_a(1-x)$:

\\[
    \log_a(1-x) = -\sum_{n=1}^{\infty} \frac{x^n}{n\ln(a)}
\\]

In our case, $x=a^{-b}E$ is so incredibly tiny that we could probably use a first approximation. Therefore, we get:

\\[
    - \varepsilon > -\frac{a^{-b}E}{\ln(a)}
\\]

But we want to know about how many digits of $\varepsilon$ we want. So, let us rewrite this with $\varepsilon \approx 10^{-d}$. And so: 

\\[
    10^{-d} < \frac{a^{-b}E}{ln(a)}
\\]

\\[
    -d < -b \log_{10}(a) + log_{10}(E) - \log_{10}(\ln(a))
\\]

Also, let's write $E=10^{-\tilde{E}}$, because that would look a bit nicer. So, we have:

\\[
    d > b \log_{10}(a) + \tilde{E} - \log_{10}(\ln(a))
\\]

Recalling that in our case $a$ is $\pi$ which is relatively small, and quoting the famous nameless theorem from Computer Science which states that the function $f(x) = \log(\log(x))$ is constant, we can write this a bit more nicely as:

\\[
    d > b \log_{10}(a) + \tilde{E}
\\]

All jokes aside, we are dealing here with numbers that taking their $\log\circ \log$ may not be negligible, that's why this is so hard in the first place. But ignoring that number is truly ok in this case since $a=\pi$ is first of all, a constant in our case, and secondly it is actually very small.

Now, notice that the first term (which is obviously the dominant one) is just the number of digits in base 10 of $a^b$, which is interesting.

So, to summarize, we discovered that in order for $a^{b-\varepsilon}$ to be within $10^{-\tilde{E}}$ of $a^b$, we need to take the $(b \log_{10}(a) + \tilde{E})$ most significant digits of $b$.

The acute reader would notice that this was not what we originally set out to find. We wanted to know how many digits of $\pi$ we would need to calculate in order to determine if $\pi^{\pi^{\pi^{\pi}}}$ is an integer.

Using our formula for $a=\pi$, and $b=\alpha$, we discovered that we need to know the first $d_\alpha \coloneqq (\alpha \log_{10}(\pi) + \tilde{E})$ digits of $\alpha$.

But knowing the first $d_\alpha$ digits of $\alpha$ is equivalent to calculating $\pi^{(\pi^{\pi})}$ to an approximation of $\tilde{E}=d_\alpha$ digits. That is, we can use our formula recursively, and discover that for that, we will need:

$$d_{\pi^{\pi}} = \pi^\pi \log_{10}(\pi) + d_\alpha$$

digits of $\pi^\pi$, which in turn, implies that we will need approximatley:

$$\pi \log_{10}(\pi) + (\pi^\pi \log_{10}(\pi) + d_\alpha)$$

digits of $\pi$.
And so, to summarize, in order to know $\pi^{\pi^{\pi^{\pi}}}$ within $\tilde{E}$ digits after the decimal point, we need to calculate:

\\[\pi \log_{10}(\pi) + \pi^\pi \log_{10}(\pi) + \pi^{\pi^\pi} \log_{10}(\pi) + \tilde{E}\\]

digits of $\pi$. Now, recall the fundamental theorem of physics, which states that $\pi^2 \approx 10$, and so, $\log_{10}(\pi) \approx \frac{1}{2}$.

And finally, we have that we need about:

\\[
    \frac{1}{2} (\pi + \pi^\pi + \pi^{\pi^\pi}) + \tilde{E} \approx \pi^{\pi^\pi} \approx 5 \cdot 10^{17}
\\]

digits of $\pi$ to know whether $\pi^{\pi^{\pi^{\pi}}}$ is an integer. The current record for digits of $\pi$ is about $2\cdot 10^{14}$, which means we need approximately $2500$ **times** more digits.

This number is so ridiculously large that even if we could store every digit in a bit, it would take approximately 500 petabytes to store that number.
And now, for the million dollar question - will we ever know if $\pi^{\pi^{\pi^{\pi}}}$ is an integer?
Well, even though it would cost a lot, 500 petabytes is not a real physical limit in the sense that it is actually possible to use that much memory.
We are talking about approximately $2^{59}$ digits in binary, and considering that the best known algorithm for computing digits of $\pi$ is asymptotically $O(n\log(n)^2)$, it would take approximately $2^{71}$ operations to compute the digits, which is, again, not a physical limit on computation as we know it.
Then, we actually need to perform the exponentiation, and I'm not sure what would be the best approach to that.

Well, my humble guess is that we probably won't live to see this specific question answered, at least not in this specific way.

But hey, cheer up, we still got a cool formula that we can try out for $\pi^{\pi^\pi}$:

The formula says that in order to know $\pi^{\pi^\pi}$ within $\tilde{E}$ digits, we need to know $\frac{1}{2} (\pi + \pi^\pi ) + \tilde{E} \approx 20 + \tilde{E}$ digits of $\pi$.
The acute reader would notice that this is not accurate, as what we calculate implies that we are within a distance of $10^{-\tilde{E}}$ of the correct number. This **probably** means that we have $\tilde{E}$ correct digits, but definitely does not imply it for certain. For example, think of the number $0.999\dots$

Some code:

```python
import mpmath
mpmath.mp.dps=1024 # Choose a stupidly large precision

def pi_n(n):
    return mpmath.mpf(mpmath.nstr(mpmath.mp.pi,n))

pi = mpmath.mp.pi

x = pi_n(22) # We want 21 digits after the decimal

pi**pi**pi - x**x**x  
# Result: mpf('-0.0483305631233250658...) 
# That is, we calculated pi^pi^pi to a correct 1 decimal place, as expected from the formula.

x = pi_n(23) # Take one more

pi**pi**pi - x**x**x  
# Result: mpf('0.005879528939608306...)
# Two correct decimal places, as expected.

x = pi_n(24) # And one more

pi**pi**pi - x**x**x  
# Result: mpf('0.00045851973331496...) 
# And so on
```

Actually, we just proved that $\pi^{\pi^\pi}$ is not an integer!