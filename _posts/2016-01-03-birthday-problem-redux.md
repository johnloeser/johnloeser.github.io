---
layout: post
title:  "Birthday problem redux"
date:   2016-01-03 16:24:00
author: John Loeser
categories: math math2
tags: probability R
---

The birthday problem is a popular question often used to introduce a course in probability. 

> How many people do I need to have in a room for there to be at least a 50% chance that two have the same birthday?

To solve the problem, it helps instead to think about the same question, but in the opposite way.

> How many people do I need to have to have in a room for there to be a **less than** 50% chance that **no two people** have the same birthday?

To solve this, we can procede by induction - the probability that no two people have the same birthday in a room with one person is 1 (initial condition), and the probability that no two people have the same birthday in a room with $$n$$ people is $$\frac{365 - (n - 1)}{365}$$ times the probability that no two people have the same birthday in a room with $$n-1$$ people[^1] (inductive step).

Given this, we just need to find the smallest $$N$$ that solves the inequality $$ 1 \times \frac{364}{365} \times \frac{363}{365} \times \ldots \times \frac{365 - (N - 2)}{365} \times \frac{365 - (N - 1)}{365} < 0.5 $$. We can write this more compactly as $$ \prod_{i = 1}^{N} \frac{365 - (i - 1)}{365} < 0.5 $$. Solving this inequality is a little more difficult[^2] - lets use R instead.

{% highlight r %}
h <- function(Nmax = 50) {
  # gives us the each of the terms in the product
  p <- (365 - (1:Nmax - 1))/365
  # takes the cumulative product up to each term
  p <- cumprod(p)
  # returns the index of the first term for which
  #   the cumulative product is small enough
  return(which(p < 0.5)[1])
}
h()
# 23
{% endhighlight %}

So we only need 23 people in the room to have at least a 50% chance that there's a pair that share a birthday. This seems surprisingly small, so much so that this problem is often called the "birthday paradox".[^3] I was reminded of this the other day when I noticed that, directly adjacent, there were two days on Facebook where many of my friends had birthdays and no friends had birthdays, respectively. These two phenomena are related - both seem surprising because we all tend to expect random events to be less clumpy than they actually are, just as our instinct is to expect a head after a long run of tails, even if both heads and tails are equally likely. Which brought me to an alternative birthday problem:

> How many people do I need to have in a room in order to have a greater than 50% chance that every single birthday is represented in the room?

In other words, how many Facebook friends do I need in order to have a greater than 50% chance that every day is one of my friends's birthday? First, lets get an approximate answer by simulation. We'll select random birthdays, and count how long it takes for us to hit every birthday. Additionally, note that we're running only 1000 simulations below. It's better to run a larger number of simulations (since that will more accurately approximate the distribution), but the code I wrote here is pretty inefficient, so this already takes about a minute or two.

{% highlight r %}
f.sim <- function(m = 1000) {
  # runs m simulations
  sims <- replicate(m, {
    # x is the calendar
    x <- 1:365
    # count is the number of people in the room
    count <- 0
    # while there's still dates left
    while(length(x) > 0) {
      # add one person to the room
      count <- count + 1
      # xi is their birthday
      xi <- sample(1:365, 1)
      # remove xi from the calendar
      x <- subset(x, x != xi)
    }
    return(count)
  })
  return(sims)
}
# set the seed for reproducibility
set.seed(42)
sims <- f.sim()
median(sims)
# 2273
{% endhighlight %}

So in order to have a greater than 50% chance of having a friend with every single birthday, you need at least 2273 friends! What about for other percentages?

{% highlight r %}
library(ggplot2)
# script defining mytheme, myplot
source("Rstart.R")
# puts the sims in a data.frame
sims.g <- data.frame(x = sims/1000)
# plots the empirical cdf
g <- ggplot(sims.g, aes(x = x)) + stat_ecdf() + mytheme +
  labs(x = "number of friends (thousands)", y = "cumulative density") +
  scale_x_continuous(breaks = seq(from = 0, to = 5, by = 0.5)) +
  ylim(0, 1)
# saves the plot
myplot(g, filename = "bpr_ecdf.png")
{% endhighlight %}

![Estimated CDF for number of friends needed to fill calendar]({{ site.baseurl }}/assets/bpr_ecdf.png)

This shows us that we'll only have about a 25% chance of having a filled calendar with 2000 friends, and with 1500 friends our chances of having the calendar filled are close to 0. Check it out on Facebook if you're curious!

Now, if you're like me and think probability is fun, you want to write down the true expression for these probabilities[^4]. If you don't think probability is fun, the rest of the post is going to be a dark rabbit hole that won't be particularly pleasant to descend.

First, because math, we can think about this problem instead as a problem of drawing balls from an urn. This urn has 365 balls in it, and each ball has a date written on it. We draw a ball, cross that ball's date off our calendar, and put the ball back in the urn. What we want to understand is the distribution of the number of balls we need to draw before we cross off all 365 dates. However, we can state this more generally

> Drawing balls from an urn with $$n$$ balls with replacment, what is the distribution of the number of balls we draw before observing $$k$$ unique balls?

where, for our modified birthday problem, $$ n = k = 365 $$. First, to simplify things, lets think about just calculating the average, rather than the full distribution. To do this, we can break this down into a series of simpler problems - how many draws does it take us to draw the first unique ball, how many draws does it take us to draw the second unique ball, all the way up to how many draws does it take us to draw the $$k$$th unique ball given that we've drawn $$k - 1$$ unique balls.

Lets consider the case where $$ n = 2 $$. The first ball we draw immediately - no matter which ball we draw, we haven't drawn it yet. Lets call this random variable $$X_{2,1}$$ - the number of draws we needed to observe the first (1) unique ball from an urn with 2 balls. $$X_{2,1}$$ in fact isn't random - it's always 1. We can still write $$\mathbb{E}[X_{2,1}] = 1$$. Lets think about the second unique ball, which takes us $$X_{2,2}$$ draws to find. $$\frac{1}{2}$$ of the time, after we draw the first unique ball, we immediately draw the second ball - we either draw the second ball, or miss and draw the first ball. We can think about this as our probability of success is $$\frac{1}{2}$$, and our probability of failure is $$\frac{1}{2}$$. We'll continue to draw balls until we have a success, and then we'll stop. This type of random variable follows what's called a geometric distribution. Lets use $$f_{2,2}(x)$$ to denote the probability that $$X_{2,2} = x$$. Then, $$ f_{2,2}(x) = \left(\frac{1}{2}\right)^{x - 1} \left(\frac{1}{2}\right) $$. This is just the probability that we had $$ x - 1 $$ failures before $$1$$ success.

What's the average number of draws, after finding the first unique ball, we need until we get the second unique ball? Intuitively, this is just the inverse of our probability of finding the ball - 2. So, $$ \mathbb{E}[X_{2,2}] = 2 $$. How about both balls? Lets define $$ X_{2,1:2} = X_{2,1} + X_{2,2} $$, the time it takes us to draw the first through the second ball. For us to find both balls, on average it takes $$ \mathbb{E}[X_{2,1:2}] = \mathbb{E}[X_{2,1}] + \mathbb{E}[X_{2,2}] = 3 $$ draws. How does this relate to our modified birthday problem? For the modified birthday problem, we want to know $$ \mathbb{E}[X_{365,1:365}] $$. We can rewrite this as $$ \sum_{i=1}^{365} \mathbb{E}[X_{365,i}] $$. To successfully draw the $$i$$th ball, we need to avoid the other $$ i - 1 $$ balls we've already drawn, so the probability of a success is $$ \frac{365 - (i - 1)}{365} $$. We saw before that we can find the expected number of draws before a success by inverting this, this tells us $$ \mathbb{E}[X_{365,1:365}] = \sum_{i=1}^{365} \frac{365}{365 - (i - 1)} $$. We can calculate this in R.

{% highlight r %}
X.mean <- function(n, k) {
  # adds, from 1 to k, n/(n - (i-1))
  sum(n/(n - (1:k - 1)))
}
X.mean(n = 365, k = 365)
# 2364.646
{% endhighlight %}

So on average, we'd need 2365 friends. Note that this is different from the median number of friends we estimated - sometimes, we would need a lot of friends in order to fill our calendar (if we could repeat this idealized experiment over and over again), which pulls the average up. How does this compare to the average from our simulations?

{% highlight r %}
mean(sims)
# 2367.594
{% endhighlight %}

These seem pretty close, suggesting we probably calculated the mean correctly. Can we be a little more formal about pretty close? One thing we can do is to resample from our simulations with replacement, and take the mean of each resampling. This allows us to approximate the distribution of the mean of the simulations if we repeated those simulations over and over again. This is known as bootstrapping. If the true mean we calculated is far away from this distribution, then it's unlikely we would have observed those 1000 simulations we generated, and this suggests that either we calculated the true mean wrong, or we got unlucky with the simulations.

{% highlight r %}
set.seed(42)
# resample our sims 10000 times, calculate the mean each time
sims.bs <- replicate(10000, mean(sample(sims, replace = T)))
sims.bs <- data.frame(x = sims.bs)
g <- ggplot(sims.bs, aes(x = x, y = ..density..)) +
  # histogram
  geom_histogram(binwidth = 10, fill = "black", col = "black") +
  # draw a line at our calculated mean
  geom_vline(xintercept = X.mean(n = 365, k = 365), col = "red") +
  mytheme + labs(x = "number of friends", y = "density")
myplot(g, filename = "bpr_meanbs.png")
{% endhighlight %}

![Bootstrapped distribution of simulated mean]({{ site.baseurl }}/assets/bpr_meanbs.png)

Looks like there's no reason to think we screwed up our math yet, since our calculated mean isn't in the tails of the simulated distribution. Lets go a step further - instead of settling with calculating the mean of $$X_{n,1:k}$$, the number of draws we needed to find $$k$$ unique balls from an urn with $$n$$ balls, lets try to calculate its full distribution. We've now seen that we can write $$X_{n,1:k}$$ as the sum $$\sum_{i=1}^{k} X_{n,i}$$, where $$X_{n,i}$$ is a geometric random variable. In general, I'm not sure if it's possible to write down the distribution of the sum of geometric random variables in a clean way.[^5] This is a special case though, where the probabilities of a success follow a very nice pattern (1, $$\frac{n-1}{n}$$, $$\frac{n-2}{n}$$, ...). So maybe we can do it!

Lets get started! We know $$f_{n,1}(x)$$, the distribution of $$X_{n,1}$$, is a degenerate case - we always immediately find the first ball. We know $$f_{n,i}(x)$$, the distribution of $$X_{n,i}$$, is just the geometric distribution - $$ f_{n,i}(x) = \left(\frac{n - (i-1)}{n}\right) \left(\frac{i-1}{n}\right)^{x-1} $$. To get the distribution of $$X_{n,1:2}$$, we add one to the random variable $$X_{n,2}$$ - in other words, we shift the distribution right by one. This gives us the distribution $$ f_{n,1:2}(x) = \frac{n - 1}{n} \left( \frac{1}{n} \right)^{x-2} $$, with the restriction that $$ x > 2 $$, since it's impossible to find two unique balls in fewer than two draws.

Lets try to get $$ f_{n,1:3}(x) $$. We know $$ X_{n,1:3} = X_{n,1:2} + X_{n,3} $$. To calculate the distribution of this sum, we need to use convolution. This means that we'll use the fact that, for example, the probability $$X_{n,1:3}$$ is equal to 4 is the probability that $$X_{n,1:2} = 3$$ times the probability that $$X_{n,3} = 1$$ plus the probability that $$X_{n,1:2} = 2$$ times the probability that $$X_{n,3} = 2$$. Equivalently, $$X_{n,1:3} = 4$$ if $$X_{n,1:2} = 2$$ and $$X_{n,3} = 2$$ or $$X_{n,1:2} = 3$$ and $$X_{n,3} = 1$$. More generally, $$f_{n,1:3}(x) = \sum_{i=1}^{x-2} f_{n,1:2}(x - i) f_{n,3}(i)$$. Solving this expression, we can find that $$ f_{n,1:3}(x) = \frac{(n-1)(n-2)}{n^{2}} \left[ 2 \left( \frac{2}{n} \right)^{x-3} - \left(\frac{1}{n}\right)^{x-3} \right] $$. We can repeat the same procedure for $$ f_{n,1:4}(x) $$, and find that $$ f_{n,1:4}(x) = \frac{(n-1)(n-2)(n-3)}{n^{3}} \left( \frac{9}{2} \left( \frac{3}{n} \right)^{x - 4} - 4 \left( \frac{2}{n} \right)^{x - 4} + \frac{1}{2} \left( \frac{1}{n} \right)^{x-4} \right) $$.

Now, a few patterns to note here. First, the first term in each expression, with all the $$n$$s that are don't have $$x$$s in their exponents, keeps getting multiplied by $$ \frac{n - (k - 1)}{n} $$ each time $$k$$ increases. This first term actually has a nice interpretation - it's the probability that every single draw is a success, or the probability that it takes $$k$$ draws to find the $$k$$ unique balls. Next, we have a series of terms that look like some constant multiplied by $$ \left( \frac{i}{n} \right)^{x - k} $$, where $$i$$ goes from 1 and $$k - 1$$. In fact, it's possible to show, but beyond the scope of this post, by induction that both of these statements will hold in general. Formally, we can write this as $$ f_{n,1:k} = \alpha_{n,k} \sum_{i=1}^{k-1} \beta_{k,i} \left( \frac{i}{n} \right)^{x-k} $$, for some values of $$ \beta_{k,i} $$, where $$ \alpha_{n,k} = \prod_{i=1}^{k-1} \frac{n-i}{n} $$.

Now, this just tells us what $$ f_{n,1:k} $$ looks like. We can go a step further and explicitly calculate the inductive step, to calculate $$ \beta_{k,i} $$ given $$ \beta_{k-1,i} $$, which will allow us to recursively calculate $$ f_{n,1:k} $$. Again, it's beyond the scope of this post, but we can find $$ \beta_{k,k-1} = \sum_{i=1}^{k-2} \frac{k - 1}{k - 1 - i} \beta_{k-1,i} $$ and, for $$ i < k-1 $$, $$\beta_{k,i} = - \frac{i}{k - 1 - i} \beta_{k-1,i} $$. We just need to combine this with the initial step, where we calculated $$ \beta_{2,1} = 1 $$. Lets calculate the first few terms in R.

{% highlight r %}
betaki <- function(k, i) {
  # base case
  if(k == 2 & i == 1) {
    return(1)
  # case when i = k - 1
  } else if (i == k - 1) {
    terms <- sapply(1:(k-2), function(j) {
      ((k-1)/(k-1-j)) * betaki(k-1, j)
      })
    return(sum(terms))
  # case when i < k - 1
  } else {
    return(-(i/(k - 1 - i)) * betaki(k-1, i))
  }
}
# calculate the beta terms for different k
sapply(1:2, function(i) { betaki(3, i) })
# -1 2
sapply(1:3, function(i) { betaki(4, i) })
# 0.5 -4.0  4.5
sapply(1:4, function(i) { betaki(5, i) })
# -0.1666667   4.0000000 -13.5000000  10.6666667
sapply(1:5, function(i) { betaki(6, i) })
# 0.04166667  -2.66666667  20.25000000 -42.66666667  26.04166667
{% endhighlight %}

This definition isn't ideal though. With some difficulty, we can reprogram the betaki function so it will run efficiently when $$k$$ gets up to 365, but it's just nicer to have a definition of these coefficients for any $$k$$ without having the calculate the coefficients for all previous $$k$$. To do this, looking at our recursive definitions, we can see the problem is that we don't have an expression for $$\beta_{k,k-1}$$ - once we calculate this, we're done. To do this, we'll use the fact that we've calculated the first 5 terms of this sequence - $$ \{ 1, 2, \frac{9}{2}, \frac{32}{3}, \frac{625}{24}, \ldots \} $$.

We can take the numerators of these terms, $$ \{ 1, 2, 9, 32, 625 \} $$, and search for them at [https://oeis.org/](https://oeis.org/) - generally, any time you have a sequence of integers, you'll find them there. Sure enough, we can find that $$ \beta_{k, k-1} = \frac{(k-1)^{k-2}}{(k-1)!} $$ for the first 5 terms. Not sure if there's any easy way to prove this, but this seems like a pretty big coincidence, so lets assume this will hold for all $$k$$. Using this in our recursive definitions, we can write down our final expression for the distribution.

$$ \alpha_{n,k} = \prod_{i=1}^{k-1} \frac{n - i}{n} $$

$$ \beta_{k,i} = (-1)^{k - i - 1} \frac{i^{k-2}}{(i-1)!(k - i - 1)!} $$

$$ f_{n,1:k}(x) = \alpha_{n,k} \sum_{i=1}^{k-1} \beta_{k,i} \left( \frac{i}{n} \right)^{x-k} $$

So, lets code this up. To test it, lets try $$n = 365$$, $$k = 365$$, and $$x = 365$$. In this case, we know the output should be $$ \alpha_{n,k} $$ - the probability that we pick a new unique date each time.

{% highlight r %}
f <- function(x, n, k) {
  # sets up our sums and products
  i <- 1:(k-1)
  # multiplies all the (n-i)/n terms together
  alphank <- prod((n - i)/n)
  # calculates each betaki term
  betaki <- ((-1)^(k - i - 1)) * (i^(k-2)) /
    (factorial(i - 1) * factorial(k - 1 - i))
  # applies our formula for f
  return(alphank * sum(betaki * (i/n)^(x - k)))
}
prod((365 - 1:364)/365)
# 1.454955e-157
f(365, 365, 365)
# NaN
# There were 50 or more warnings
{% endhighlight %}

Turns out trying to calculate 365! is beyond the standard memory R can use to hold a number, since 365! is really big. We can try to use the log of the factorial instead, which is smaller, and make use of the trick that $$ e^{\log x} = x $$.

{% highlight r %}
f2 <- function(x, n, k) {
  # sets up our sums and products
  i <- 1:(k-1)
  # multiplies all the (n-i)/n terms together
  alphank <- prod((n - i)/n)
  # calculates each betaki term
  betaki <- ((-1)^(k - i - 1)) *
    exp((k-2) * log(i) - lfactorial(i - 1) -
          lfactorial(k - 1 - i))
  # applies our formula for f
  return(alphank * sum(betaki * (i/n)^(x - k)))
}
prod((365 - 1:364)/365)
# 1.454955e-157
f(365, 365, 365)
# -1.461896e+30
{% endhighlight %}

It's not good when a probability gets returned as a negative number, especially a really big negative number. Looking at our formula, the $$\beta_{k,i}$$ terms are large numbers that are alternating in sign. When we try to add these together, small (in terms of percentage of the original number) approximation errors in how these terms are represented can end up being large. To get around this, we can turn to libraries that use arbitrary precision arithmetic in R. These libraries will represent all of the numbers we're using as rational numbers, and they'll expand the amount of memory used to represent them as the integers in the rational numbers get very large. This adds processing time, but it makes the computation feasible.

{% highlight r %}
library(gmp)
f3 <- function(x, n, k) {
  # sets up our sums and products
  i <- 1:(k-1)
  # multiplies all the (n-i)/n terms together
  alphank <- prod(as.bigq(n - i, n))
  # calculates each betaki term
  betaki <- ((-1)^(k - i - 1)) * (as.bigz(i)^(k-2)) /
    (factorialZ(i - 1) * factorialZ(k - 1 - i))
  # applies our formula for f
  out <- alphank * sum(betaki * as.bigq(i, n)^(x - k))
  # converts the answer back to a normal number in R
  return(as.numeric(out))
}
prod((365 - 1:364)/365)
# 1.454955e-157
f3(365, 365, 365)
# 1.454955e-157
{% endhighlight %}

Looks like we did things right! To verify, lets compare how this distribution looks with our simulations. Actually evaluating this function for large $$x$$, $$n$$ and $$k$$ can take a long time, which is exactly what we want to do for the birthday problem. To speed up the comparison, we'll instead only calculate it every 50 integers, and use that to approximate the distribution.

{% highlight r %}
# generate the xs at which we'll evaluate f
x <- seq(from = floor(min(sims)/50)*50 - 25,
         to = ceiling(max(sims)/50)*50 + 25,
         by = 50)
# evaluate f at those xs
f3.g <- sapply(x, function(xi) { f3(xi, 365, 365) })
# store this in a data.frame
f3.g <- data.frame(x = x, y = f3.g)
# store the simulations in a data.frame
sims.g2 <- data.frame(x = sims)

# plot the histogram from the sims and the density we calculated
g <- ggplot(data = sims.g2) +
  geom_histogram(data = sims.g2, aes(x = x, y = ..density..),
                 binwidth = 50,
                 col = "black", fill = "black") +
  geom_line(data = f3.g, aes(x = x, y = y), col = "red") +
  mytheme + labs(x = "number of friends", y = "density")
myplot(g, filename = "bpr_f.png")
{% endhighlight %}

![Simulated and calculated densities for f]({{ site.baseurl }}/assets/bpr_f.png)

Looks like we got the math right! We're now almost equipped to answer our original question.

> How many people do I need to have in a room in order to have a greater than 50% chance that every single birthday is represented in the room?

What we need for this is the probability that every single birthday is represented in a room with $$x$$ people, or that we needed less than or equal to $$x$$ people in order to observe each birthay. This is just the cumulative distribution function, which we'll call $$F_{n,1:k}$$, which we can get by adding together the $$f$$s we calculated for every number less than or equal to $$x$$. This is a time intensive operation, so instead we can do a little math to calculate $$F_{n,1:k}$$.

$$ F_{n,1:k}(x) = \alpha_{n,k} \sum_{i=1}^{k-1} \beta_{k,i} \frac{1 - \left( \frac{i}{n} \right)^{x-k+1}}{1 - \left( \frac{i}{n} \right)} $$

So, our problem reduces to finding the smallest $$x$$ that satisfies $$ F_{n,1:k}(x) \geq 0.5 $$. For the original birthday problem, we solved this by trying every possible value of $$x$$, and finding the smallest one that satisfied this inequality. This will take too long for our new birthday problem, so we need to be more careful about which values of $$x$$ we'll try. For this we'll use Euler's method. Imagine we want to solve $$ h(x) = 0 $$ for some function $$ h $$. Let's take two guesses, $$ x_{0} $$ and $$ x_{1} $$. We can then evaluate $$ h $$ at each of those guesses, and then approximate $$ h $$ using a linear function.

$$ h(x) \approx \frac{h(x_{1}) - h(x_{0})}{x_{1} - x_{0}} x + h(x_{0}) $$

We can then solve $$ h(x) = 0 $$ using our approximation. The solution to this gives us a new guess, $$ x_{2} $$.

$$ x_{2} = x_{0} - y_{0} \frac{x_{1} - x_{0}}{y_{1} - y_{0}} $$

If we repeat this procedure using our two new guesses, $$ x_{1} $$ and $$ x_{0} $$, then for nice functions we'll eventually converge to a solution. Lets implement a simple version of this in R, accounting for the fact that we want to restrict our guesses to be integers, and apply this to our cumulative distribution function.

{% highlight r %}
F3 <- function(x, n, k) {
  # sets up our sums and products 
  i <- 1:(k-1)
  # multiplies all the (n-i)/n terms together
  alphank <- prod(as.bigq(n - i, n))
  # calculates each betaki term
  betaki <- ((-1)^(k - i - 1)) * (as.bigz(i)^(k-2)) /
    (factorialZ(i - 1) * factorialZ(k - 1 - i))
  # applies our formula for F
  out <- alphank *
    sum(betaki * (1 - as.bigq(i, n)^(x - k + 1)) / (1 - as.bigq(i, n)))
  # converts the answer back to a normal number in R
  return(as.numeric(out))
}

mysolve <- function(myf, x0, x1, y0 = NA) {
  # calculates the difference between the guesses
  inc <- x1 - x0
  # if the guesses are far apart
  if(inc > 1) {
    # calculates y0 if we didn't already
    if(is.na(y0)) {
      y0 <- myf(x0)
    }
    # calculates y1
    y1 <- myf(x1)
    # calculates x2
    x2 <- round(x0 - y0 * (x1 - x0)/(y1 - y0))
    # repeats with x1, x2 and y1
    return(mysolve(myf, x1, x2, y1))
  # if the guesses are close
  } else {
    # return our most recent guess
    return(x1)
  }
}
# define new function we want to solve for 0
F3.bday <- function(x) { F3(x, 365, 365) - 0.5 }
# use initial guesses of 2000 and 2200
mysolve(F3.bday, 2000, 2200)
# 2287
F3(2287, 365, 365)
# 0.5003708
F3(2286, 365, 365)
# 0.4994142
{% endhighlight %}

So, we can answer our original question - we need 2287 friends to have a greater than 50% chance of having a friend with every single birthday! But I have 1049 friends, so what's my probability of having a friend with every single birthday?

{% highlight r %}
F3(1049, 365, 365)
# 7.503474e-11
{% endhighlight %}

That's about the probability of selecting a particular person at random from the world's population, so not too good. So let's try to calculate something a little more interesting.

> If I have $$x$$ people in a room, what's the median number of birthdays that will be represented in the room?

In other words, how many birthdays can I expect to have represented amongst my $$x$$ Facebook friends? First, lets count the number of birthdays my Facebook friends have (dropping February 29), and the number of my Facebook friends who display their birthday. If you search "export facebook birthdays", you'll be able to find a help link that will allow you to export them as a calendar. I downloaded this and called this file "fbbday.ics". 

{% highlight r %}
# read in the file
bdays <- readLines("fbbday.ics")
# just get the dates
bdays <- bdays[grepl("DTSTART", bdays)]
# strip to the month and day
bdays <- substr(bdays, 13, nchar(bdays))
# drop February 29
bdays <- subset(bdays, bdays != "0229")
length(bdays)
# 894 friends
length(unique(bdays))
# 331 birthdays
{% endhighlight %}

So I've got 331 of 365 represented. Is that around what we should expect? Lets define a new random variable, $$Y_{n,x}$$, the number of unique balls I've drawn from an urn with $$ n $$ balls after taking $$ x $$ draws. It's probability distribution function is $$ g_{n,x}(k) $$, the probability that I have drawn $$ k $$ unique balls from an urn with $$ n $$ balls given that I've taken $$ x $$ draws. This, however, is just the probability that I needed less than or equal to $$ x $$ draws to reach $$ k $$ unique balls, minus the probability that I needed less than or equal to $$ x $$ draws to reach $$ k + 1 $$ unique balls.

$$ g_{n,x}(k) = F_{n,k}(x) - F_{n,k+1}(x) $$

Similarly, suppose I want to know the cumulative distribution function for $$ Y_{n,x} $$, the probability that I reached less than or equal to $$ k $$ unique balls after $$ x $$ draws. This is just 1 minus the probability that I reached $$ k + 1 $$ unique balls in less than or equal to $$ x $$ draws.

$$ G_{n,x}(k) = 1 - F_{n,k+1}(x) $$

First, lets run some simulations, and calculate the mean and median for reference.

{% highlight r %}
g.sim <- function(x, m = 100000) {
  # run m simulations
  sims <- replicate(m, {
    # sample x birthdays with replacement
    sim <- sample(1:365, size = x, replace = T)
    # number of unique birthdays we sampled
    length(unique(sim))
  })
  return(sims)
}
set.seed(42)
# run the simulations with 894 draws
sims2 <- g.sim(894)
mean(sims2)
# 333.5859
median(sims2)
# 334
{% endhighlight %}

The actual number of birthdays I have on Facebook, 331, seems pretty close to the mean and median of the distribution. How close is close? Let's actually calculate the probability that the number of birthdays I would have is less than or equal to 331.

{% highlight r %}
g <- function(k, n, x) {
  F3(x, n, k) - F3(x, n, k + 1)
}
G  <- function(k, n, x) {
  1 - F3(x, n, k + 1)
}
G(331, 365, 894)
# 0.3236788
{% endhighlight %}

Pretty high. Maybe we just got lucky and our math is wrong? Lets compare the density $$ g_{n,x}(k) $$ we just calculated to the simulated density.

{% highlight r %}
# make ks we'll use for plotting
k <- seq(from = min(sims2),
         to = max(sims2),
         by = 1)
# estimate density at each k
g.g <- sapply(k, function(ki) { g(ki, 365, 894) })
# convert to data.frame
g.g <- data.frame(x = k, y = g.g)
# convert simulations to data.frame
sims2.g <- data.frame(x = sims2)

# plot with histogram, density, and line at 331
g <- ggplot(data = sims2.g) +
  geom_histogram(data = sims2.g, aes(x = x, y = ..density..),
                 binwidth = 1,
                 col = "black", fill = "black") +
  geom_line(data = g.g, aes(x = x, y = y), col = "red") +
  geom_vline(xintercept = 331, col = "blue") +
  mytheme + labs(x = "number of birthdays", y = "density")
myplot(g, filename = "bpr_g.png")
{% endhighlight %}

![Simulated and calculated densities for g]({{ site.baseurl }}/assets/bpr_g.png)

It doesn't look like the math was wrong - our simulations and our calculated density line up almost perfectly.[^6] Everything seems OK!

I was a little surprised by how close we ended up to the actual distribution. In practice, there is variation in how likely some birthdays are relative to others. Not only that, who our friends are can affect where birthdays end up being clustered. My friends who were college athletes are more likely to have January and February birthdays, and my Rwandan friends are more likely to have January 1 as a birthday. We can actually test if my friends' birthdays are more clustered than would be expected under a uniform distribution. One easy way to do this is to calculate the number of my friends who have each birthday, and then sum the squares of those numbers. We can then simulate this by generating uniformly distributed random variables across all 365 birthdays, calculate the same statistic, and compare the calculated statistic to the simulated distribution.

{% highlight r %}
mystat <- function(x) {
  # how many friends have each birthday
  out <- sapply(unique(x), function(xi) { sum(x == xi) })
  # sum the squares of these numbers
  sum(out^2)
}
sims3 <- replicate(10000, {
  # generate 894 random birthdays
  sim <- floor(runif(894) * 365)
  # calculate the statistic
  mystat(sim)
})
# how frequently is the statistics we'd get randomly
#   greater than the statistic from my friends'
#   birthdays
1 - sum(mystat(bdays) > sims3) / 10000
# 0.1211

# put the simulations in a data.frame
sims3.g <- data.frame(x = sims3)
g <- ggplot(data = sims3.g) +
  geom_histogram(data = sims3.g, aes(x = x, y = ..density..),
                 binwidth = 10,
                 col = "black", fill = "black") +
  geom_vline(xintercept = mystat(bdays), col = "blue") +
  mytheme + labs(x = "statistic", y = "density")
myplot(g, filename = "bpr_stat.png")
{% endhighlight %}

![Simulated density for clustered statistic]({{ site.baseurl }}/assets/bpr_stat.png)

So it looks like my friends' birthdays might be a bit more clustered than we'd typically expect for a uniformly distributed random variable, but not too much so.[^7] It actually looks like our approximation, assuming birthdays are random, does a pretty good job!

[^1]: This follows from the fact that the $$n$$th person needs to avoid the birthdays of the other $$n-1$$ people in order to keep it so no two people in the room have the same birthday. Also, I'm going to pretend that there's only 365 calendar days and that birthdays are uniformly distributed across those days, which is close enough to the truth to not affect the solutions to these problems much.
[^2]: My prefered (under)approximation relies on the fact that, for small $$x$$ and $$y$$, $$(1 - x)(1 - y) \approx 1 - x - y$$. We then need to solve $$ 1 - \sum_{i = 1}^{N} \frac{i - 1}{365} \leq 0.5 $$, which simplifies to $$ \sum_{i=0}^{N - 1} i \geq \frac{365}{2} $$. We can further use a trick (that a young Gauss apocryphally discovered when asked to sum all the numbers from 1 to 100 as punishment by teacher) to rewrite this as $$ \frac{N(N-1)}{2} \geq \frac{365}{2} $$, which yields 20 as the approximation.
[^3]: It's not a paradox.
[^4]: "True" is relative - again, this all assumes we have a 365 day calendar with each birthday equally probable.
[^5]: This is a special case of finding the distribution of the sum of negative binomial random variables. It looked like there's some paper that derives an expression for exactly this case, but it's behind a paywall so I'm going to pretend it doesn't exist.
[^6]: The fit here with the simulations is much better than last time, since we ran a much larger number of simulations.
[^7]: I doubt that 894 observations is enough to detect the slight degree of clustering that occurs in practice with birthdays though.