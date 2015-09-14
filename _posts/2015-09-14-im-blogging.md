---
layout: post
title:  "I'm blogging?"
date:   2015-09-14 14:01:00
categories: meta
---
Sometimes Brick asks questions when he should be making a statement. Like "I love lamp?".

{% highlight r %}
brick_statements <- c("I love lamp?", "I'm blogging?")
brick <- function(statements) {
	statement <- sample(statements, 1)	
	print(statement)
}
# will randomly print either "I love lamp?" or "I'm blogging?"
brick(brick_statements)
{% endhighlight %}

When I was testing this, I almost immediately got a run of 7 "I'm blogging"s[^1]. How likely is this? I know it's not easy to calculate, and if the reason I don't want to calculate it is I don't trust R's random number generator, then I might find it to difficult to estimate by simulation the probability of observing such a long run so quickly.

I try to do math sometimes. The first time I learned to program was when I made my TI-83 ask me what $$a$$, $$b$$, and $$c$$ were so it could calculate for me that $$ a x^{2} + b x + c = 0 \Leftrightarrow x = - \frac{b}{2a} \pm \frac{\sqrt{b^{2} - 4ac}}{2a} $$. I don't do this any more because my arithmetic skills have gotten a lot better since middle school even if I'm a lot less clever.

I'm writing all of this in markdown. Seems magical. [Jekyll][jekyll] is nifty.

If you click the CV link, you'll notice something funny about my CV. ![CV]({{ site.baseurl }}/CV.jpg) So it looks like my maturity hasn't improved much either. I could definitely kick my middle school self's ass though, I'll be able to hold that over him for a while.

[^1]: I really don't like using an apostrophe when I'm constructing a plural of something like "I'm blogging". What if "I'm blogging" is in possession of something? Maybe multiple "I'm blogging"s, as in ""I'm blogging"'s "I'm blogging"s". Am I doing that right?

[jekyll]: http://jekyllrb.com/