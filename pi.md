# Will we ever know if $\pi^{\pi^{\pi^{\pi}}}$ is an integer ?

I stumbled upon Matt Parker's incredible video: [Why π^π^π^π could be an integer](https://www.youtube.com/watch?v=BdHFLfv-ThQ), and it really piqued my interest. I guess that like everybody else, I thought "That's a bunch of nonsese, just calculate it to a few digits after the decimal place". But this question is, in some sense, at the limit of what is feasible to compute, and I find that interesting. On the one hand, it's a simple arithimetic calculation, but on the other hand, it just as well may be that we will never be able to compute solve it, at least in the "brute-force" method.

After understanding that this may not be feasible, the interesting question then is, well, how many digits of $\pi$ do you need? And this is what I intend on answering in this page.

First, let's start by denoiting $\alpha \coloneqq \pi^{\pi^{\pi}}$. In order to know whether $\pi^{\pi^{\pi^{\pi}}}$ is an integer, we would like to calculate $\pi^\alpha$ to some degree of precision, where $\alpha \approx 10^{18}$ is a huge number.

More generally, we want to calculate a value $a^b$ to some degree of percision. In our case, both numbers are irrational.

Say we only use the first $d$ digits (in decimal) of $b$. That is, instead of $b$, we are actually raising $a$ to the power of $\tilde{b} \coloneqq b-\varepsilon$, where $\varepsilon \approx 10^{-d}$. Always keep in mind that in our case $b$ is ridiculously large, and $\varepsilon$ is ridiculously tiny.

Now, we want to know how far this is from the actual result. That is, we would like to bound the following expression:

$$ |a^b - a^{b - \varepsilon}| < E $$

We could choose $E$ to be something rea sonable, say, $E=10^{-2}$. That way, if we calculate $a^{b-\varepsilon}$, and see that it is not within a distace of $0.01$ of an integer, then we would know that $\pi^\alpha$ is not an integer either.
If it is, we could make $E$ smaller. We will shortly see that this doesn't really matter anyway.

Let's start crunching away:


$$ |a^b - a^{b - \varepsilon}| < E $$

$$ a^b(1 - a^{-\varepsilon}) < E $$

$$ (1 - a^{- \varepsilon}) < a^{-b}E $$

$$ a^{- \varepsilon} > 1 - a^{-b}E $$

$$ - \varepsilon > \log_a(1- a^{-b}E) $$

Now, recall the taylor series expansion of $\log_a(1-x)$:

$$ \log_a(1-x) = -\sum_{n=1}^{\infty} \frac{x^n}{n\ln(a)} $$

In our case, $x=a^{-b}E$ is so incredibly tiny that we could probably use a first approximation. Therefore, we get:

$$ - \varepsilon > -\frac{a^{-b}E}{\ln(a)} $$

But we want to know about how many digits of $\varepsilon$ we want. So, let us rewrite this with $\varepsilon \approx 10^{-d}$. And so: 

$$ 10^{-d} < \frac{a^{-b}E}{ln(a)} $$

$$ -d < -b \log_{10}(a) + log_{10}(E) - \log_{10}(\ln(a)) $$

Also, let's write $E=10^{-\tilde{E}}$, because that would look a bit nicer. So, we have:

$$ d > b \log_{10}(a) + \tilde{E} - \log_{10}(\ln(a)) $$

Recalling that in our case $a$ is $\pi$ which is relatively small, and quoting the famous namesless theorem from Computer Science that the function $f(x) = \log(\log(x))$ is constant, we can write this a bit more nicely as:

$$ d > b \log_{10}(a) + \tilde{E} $$

Notice that the first term (which is obviously the dominant) is just the number of digits in base 10 of $a^b$, which is interesting.

So, to summarize, we discovered that in order for $a^{b-\varepsilon}$ to be within $10^{-\tilde{E}}$ of $a^b$, we need to take the $(b \log_{10}(a) + \tilde{E})$ most significat digits of $b$.

The acute reader would notice that this was not what we originally set out to find. We wanted to know how many digits of $\pi$ we would need to calculate in order to determine if $\pi^{\pi^{\pi^{\pi}}}$ is an integer.

Using our formula for $a=\pi$, and $b=\alpha$, we discovered that we need to know the first $d_\alpha \coloneqq (\alpha \log_{10}(\pi) + \tilde{E})$ digits of $\alpha$.

But knowing the first $d_\alpha$ is equivalent to calculating $\pi^{(\pi^{\pi})}$ to an approximation of $\tilde{E}=d_\alpha$ digits. That is, we can use our formula recursively, and discover that for that, we need:

$$d_{\pi^{\pi}} = \pi^\pi \log_{10}(\pi) + d_\alpha$$

digits of $\pi^\pi$, which in turn, implies that we need approximatly:

$$\pi \log_{10}(\pi) + \pi^\pi \log_{10}(\pi) + d_\alpha$$

digits of $\pi$.
And so, to summarize, in order to know $\pi^{\pi^{\pi^{\pi}}}$ within $\tilde{E}$ digits after the decimal point, we need to calculate:

$$\pi \log_{10}(\pi) + \pi^\pi \log_{10}(\pi) + \pi^{\pi^\pi} \log_{10}(\pi) + \tilde{E} $$

digits of $\pi$. Now, recall the fundamental theorem of physics, which state that $\pi^2 \approx 10$, and so, $\log_{10}(\pi) \approx \frac{1}{2}$.

And finally, we have that we need about:

$$ \frac{1}{2} (\pi + \pi^\pi + \pi^{\pi^\pi}) + \tilde{E} \approx \pi^{\pi^\pi} \approx 5 \cdot 10^{17} $$

digits of $\pi$ to know whether $\pi^{\pi^{\pi^{\pi}}}$ is an integer. The current record for digits of $\pi$ is about $2\cdot 10^{14}$, which means we need approximatly $2500$ times more digits.

We can test our results for $\pi^{\pi^\pi}$. Our formula says that in order to know $\pi^{\pi^\pi}$ within $\tilde{E}$ digits (the acute reader would notice that this is not accurate, as what we calculate implies that we are within a distance of $10^{-\tilde{E}}$ of the correct number. This **probably** means that we have $\tilde{E}$ correct digits, but definitely does not imply it for certain. Think of the number $0.999\dots$), we need to know $\frac{1}{2} (\pi + \pi^\pi ) + \tilde{E} \approx 20 + \tilde{E}$ digits of $\pi$. 

```python
import mpmath
mpmath.mp.dps=1024 # Choose a stupidly large percision

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