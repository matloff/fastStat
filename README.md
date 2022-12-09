
# fastStat: All of REAL Statistics

## *"Quick intro to statistics for those who know probability"*

### Norm Matloff, Prof. of Computer Science, UC Davis; [my bio](http://heather.cs.ucdavis.edu/matloff.html)

(See notice at the end of this document regarding copyright.)

## WHO IS THIS FOR?

Students in computer science, engineering, mathematics and the like
typically take a course in calculus-based probability -- unconditional
and conditional probability, cdfs and density functions, expected value
and so on.  But later they have a need to use statistics, and find it's
a broader and more nuanced field than they had realized.

**This document will enable such people to quickly acquire the needed
concepts, with a good intuitive understanding.**  It is modeled after
my popular [fasteR tutorial](https://github.com/matloff/fasteR) for
quickly becoming proficient in R. 

By the way, the title, "All of REAL Statistics," is a play on two 
excellent books, *All of Statistics* and *All of Nonparametric Statistics*,
by one of my favorite statisticians, Larry Wasserman.  Both books are
quite thin, making their "All of" titles ironic.  Well, my short
tutorial here is even more "all of" in that sense.

## <a name="intuition">Lesson INTRO:  What Is Really Going on?</a> 

Professional statisticians, especially Statistics professors, may find
the presentation here to be a familiar story, but with an odd plot,
a different cast of characters, and a different ending. :-) 

Indeed, **many of readers of this
tutorial will be surprised to see that it does not contain many
equations.**  But sadly, many people know the mechanics of statistics
very well, without truly understanding an intuitive levels what those
equations are really doing.

For instance, consider estimator bias. Students in a math stat course
learn the mathematical definition of bias, after which they learn that
the sample mean is unbiased and that the sample variance can be
adjusted to be unbiased.  But that is the last they hear of the issue.
Actually, most estimators are in fact biased, and lack "fixes" like that
of the sample variance.  Does it matter?  None of that is discussed in
textbooks and courses.

We will indeed do some math derivations here, but not at the outset, and
not highlighted.  This tutorial aims to explain the practical ISSUES. 

## <a name="sampling">Lesson SAMPLING:  The Notion of a Sample</a> 

We've all heard the term *margin of error* in an opinion poll.  It will
be discussed in detail in a later lesson, but what question is it
addressing?

Say the poll consists of querying 1200 people.  These were drawn
randomly from some list, say a list of phone numbers.  The point is that
if we were to do this again, we would get 1200 other people, and the
percentage saying Yes to our question would change.  Thus we want to
have some idea as to how much our Yes percentage varies from one sample
of 1200 people (note: this is NOT referred to as "1200 samples") to
another.

Let's set some notation.  Say we are interested in some quantity X, say
human height.  We take a sample of n people from a given target
population, and denote the value of X in the i<sup>th</sup> person in
our sample by X<sub>i</sub>.  If we sample with replacement (or if n is
small relative to the total population size), the X<sub>i</sub> are
independent random variables.  Also, each X<sub>i</sub> has distribution
equal to that of the sampled population.  If, say 22.8% of people in
this population are taller than 70 inches, then P(X<sub>i</sub> > 70) =
0.228.

## <a name="normal">Lesson NORMALETC:  The Role of Normal (Gaussian) and Other Parametric Distribution Families</a> 

In the last lesson, we talked about the distribution of X in the
population.  Although the population is finite (more on this below) and
thus X is a discrete random variable, one often models X as continuous,
with its distribution being in the normal family.

Why do this?

* Histograms of X in many applications do look rather bell-shaped.  This
  may in turn be due to the Central Limit Theorem (CLT).  The CLT says
that sums are approximately normal, and in the human height case, one
can think of the body as consisting of chunks whose heights sum to the
height of the person.  (The CLT assumes i.i.d. summands, and the chunks
here would be neither independent nor indentically distributed, but
there are non-i.i.d. versions of the CLT.)

* The early developers of statistics had no computers, and it turns out
  that the normal distribution family is quite mathematically tractable,
thus amenable to closed-form "exact" solutions.

* It is often the case in math that discrete quantities are approximated
  by continuous ones.

* A normal distribution is determined by two parameters, the mean and
  variance of the distribution.  Without that assumption, we have many
  parameters, essentially infinitely many.  Let F<sub>X</sub> be the cdf
  of X, i.e. F<sub>X</sub>(t) = P(X &le; t).  Well, there are infinitely
  many possible values for t.

Another popular model is the exponential distribution family.  You
probably learned in your probability course that it is "memoryless,"
which makes it a suitable model in some applications.

Note that, as models, these are necessarily inexact.  No distribution in
practice is exactly normal, for instance.  No one is 900 feet tall, and
no one has a negative height.  For that matter, a normal distribution is
continuous, whereas X is discrete, for two reasons:

* We are sampling from a finite population.

* Our measuring instruments have only finite precision.  If, say, X is
  bounded between 0 and 10, and is measured to 2 decimal places, X can
take on 1000 values, and thus is discrete.

But what are we estimating, in light of the fact that our model is only
approximate?  Say for instance we model X as having a gamma
distribution.  Then in some sense, depending on how we estimate, we are
estimating the gamma distribution that is closest to our true population
distribution.

## <a name="normal">Lesson CONCEPTPOPS:  Conceptual Populations</a> 

In the opinion poll example, it is clear as to which population is
sampled.  In many applications, the issue is more conceptual.  If for
instance we run a clinical trial with 100 diabetic patients, we might
think of them as having been sampled from the population of all
diabetics, even though we did not actually select the patients in our
sample, whether randomly or otherwise.

This issue can become quite a challenge in, say, economic analysis.
If we have 10 years of annual data, i.e. n = 10, what population
is that a "sample" from?

## <a name="stderr">Lesson STDERRS:  Standard Errors</a> 

Earlier we mentioned the "margin of error" in reporting the results of
opinion polls.  To make that notion concrete, let's first discuss a
related idea, *standard errors*.

We use our data X<sub>1</sub>,...,X<sub>n</sub> to estimate some
quantity of interest, say the proportion q of people in the population
who would answer Yes to our poll if we had a chance to ask them all.
Our estimate, Q, would be the proportion of people in our sample who say
yes.  (Extremely important note:  Make sure to always carefully
distinguish between a population quantity, q in this case, and its
sample estimate, Q here.)

We want to have some measure of how much Q varies from one sample to
another.  Of course, Var(Q) is such a measure.

Say for now that the average of Q, averaged over all possible
samples, is q.  For some samples, Q > q, for others Q <- q, but on
average we get q.  This relates to the issue of *bias*, which we will
turn to later, but for now, say we have this situation, i.e. EQ = q.

The key point:  If Var(Q) is small, then Q doesn't vary much from one
sample to another, and if EQ = q, then for "most" sample, Q should be
near q.  That exactly what we hope for!  We only have one sample, of
course, but if we know that Q is usually near q, we feel
reasonably confident that the Q from our particular sample is near q.

Of course, the square root of any variance is called the *standard
deviation*.  In the case of an estimator, Q here, we use the term
*standard error*.  In some cases, it will be only the approximate
standard deviation, as will be seen below.

## <a name="bias">Lesson BIAS:  Bias, and Impact on Standard Errors</a> 

In our last lesson, we assumed that EQ = q.  We say that Q is an 
*unbiased* estimator of q.  In English:  the average value of Q over all
possible samples is q. 

In the above example, in which Q is the sample proportion of Yes's an
q is the correspondng population proportion, it does turn out that Q is
unbiased.  In fact, any sample mean is an unbiased estimator for the
population mean.  (Here X is 1 or 0, so the average of the X<sub>i</sub>
is the proportion of Yes's.)  Let's skip the derivation for now (we'll
have a Derivations lesson later), so we can get to the larger issues.

Unbiasedness at first seems to be a very desirable property.  It does
hold for some classical statistical methods, but does NOT hold for many
others, one of which is the sample variance, as follows.

Say we wish to estimate the population variance &sigma;<sup>2</sup>.
(We are NOT necessarily assuming a normal distribution.)  In the
population, that is the average squared difference between the data and
their mean.  The sample analog is

S<sup>2</sup> = (1/n) &Sigma;<sub>i</sub><sup>n</sup>
(X<sub>i</sub> - &#x100;)<sup>2</sup>

where &#x100; is the sample mean, (1/n)  &Sigma;<sub>i</sub><sup>n</sup>
X<sub>i</sub>.

It can be shown that S<sup>2</sup> is biased:

E(S<sup>2</sup>) = [(n-1) / n] &sigma;<sup>2</sup>

The average value of S<sup>2</sup> over all samples is a little too
low.  The amount of bias is 

E(S<sup>2</sup>) -  &sigma;<sup>2</sup> = -1/n  &sigma;<sup>2</sup> 

This bothered the early developers of statistics, who defined the
sample variance as

s<sup>2</sup> = (1/(n-1)) &Sigma;<sub>i</sub><sup>n</sup>
(X<sub>i</sub> - &#x100;)<sup>2</sup>

(In the field of probability and statistics, it is customary to use
capital letters for random variables.  This is an exception.)

The standard error of &#x100; is

s.e.(&#x100;) = S/n<sup>0.5</sup>

(use s instead of S if you wish).

But most estimators are not only biased, but also lack simple
adjustments like that for S above.  So, one must accept bias in general,
and consider its implications.

In that light, let's return to the discussion of standard error in the
last lesson.  We stated that an estimator with small standard error
would likely be near the population value it is estimating, but note now
that that argument depended on the estimator being unbiased.

But it is much more optimistic, actually.  Say an estimator has bias
of size O(1/n), as above.  Typically the size of the standard error is
O(1/n<sup>0.5</sup>).  In other words, the bias is small relative to the
standard error, so the argument in the last lesson still holds.

## <a name="CIs">Lesson INDICATORS:  Indicator Variables</a> 

Often X has only the values only 1 and 0, indicating the presence or
absence of some trait.  That is the case in the opinion poll, for
example, where the respondent replies Yes or not Yes.  Such a variable
is called an *indicator variable*, as it indicates whether the trait is
present or not.

In this case, &#x100; reduces to the proportion of 1s, as with Q, the
proportion of Yes responses to the opinion poll.  After some algebraic
simplification, it turns out that 

S<sup>2</sup> = &#x100; (1-&#x100;) / n

(or use n-1 instead of n for s).

## <a name="CIs">Lesson CI:  Confidence Intervals</a> 

In our opinion poll example, Q is called a *point estimate* of q.  We
would also like to have an interval estimate, which gives a range of
values.  If say in in an election, the results of an opinion poll are
reported as, Candidate X has support of 62.1% of the voters, with a
margin of 3.9%," it is saying,

> A 95% confidence interval (CI) for X's support is (58.2%,66.0%).

A key point is the meaning of "95% confident."  Imagine forming this
interval on each possible sample from the given population.  Then 95% of
the intervals would cover the true value, q.

Of course, we do not collect all possible samples; we just have 1.  We
say we are 95% that q is in our particular interval in the above sense.
(A note on the phrasing "q is in our interval":  Some may take this to
mean that q is random, which it is not; q is unknown but fixed.)

It's exactly like gambling.  We don't know whether our particular roll
of the dice will yield a winner, but if most rolls of the dice do so,
then we may be willing to go ahead.

## <a name="asymp">Lesson CIAPPROX:  Confidence Intervals from Asymptotics</a> 

The early developers of statistics defined a distribution family known
as *Student's t*.  This supposedly can be used to form "exact," i.e. the
probability of the CI covering the target population value is exactly
0.95, if the population distribution of X is normal.  Student-t is
widely taught, and thus widely used.  But that is just an illusion.  As
pointed out earlier, no distribution in practice is exactly normal.

What saves the day, though, is the Central Limit Theorem.  &#x100; is a
sum, which the CLT tells us is asymptocially normally distributed
as n goes to infinity/

In other words, if we were to compute &#x100; on each of the possible
samples of size n from the population, and then plot the results in a
histogram, the graph would be approximately bell-shaped, and the
probabilities for any range of values would be approximately those of a
normal distribution.

Ah, so we're in business:  For any random variable W, the quantity

(W - EW) / (Var(W)<sup>0.5</sup>

has mean 0 and variance 1.  (This has nothing to do with whether W is
normal or not.)  So,

Z = (&#x100; - &mu;) / s.e.(&#x100;)

has mean 0 and variance 1, where &mu; is the population mean of X.
(Recall that &#x100; is unbiased for &mu;.)  And since Z actually DOES
have an approximately normal distribution, its distribution is thus
approximately N(0,1).  

Now since the N(0,1) distribution has 95% of its area between -1.96 and
1.96, we have

0.95 &approx; P[-1.96 < (&#x100; - &mu;) / s.e.(&#x100;) < 1.96]

which after algebra becomes

0.95 &approx; P[
&#x100; - 1.96 s.e.(&#x100;) < &mu; <
&#x100; + 1.96 s.e.(&#x100;))

There's our CI for &mu;!

(&#x100; - 1.96 s.e.(&#x100;), &#x100; + 1.96 s.e.(&#x100;))

We are (approximately) 95% confident that the true population mean is in
that interval.  (Remember. it is the interval that is random, not &mu;)

And things don't stop there.  Actually, many types of estimators have
some kind of sum within them, and thus have an approximately normal
distribution.  Thus approximate CIs can be found for lots of different
estimators, notably Maximum Likelihood Estimators and least-squares
parametric regression estimators, as we will see in a later lesson.

In other words, we have the following principle (the name is mind, the
principle general):

**The Fundamental Tool of Statistical Inference:**

> If R is a "smooth" estimator of a population quantity r, based on an
> i.i.d. sample, then an approximate 95% confidence interval for r is
> 
> (R - 1.96 s.e.(R), R + 1.96 s.e.(R)

The term *smooth* roughly means that R is a differentiable function of
X<sub>1</sub>, ..., X<sub>n</sub>.

By the way, for large n, the Student-t distribution is almost identical
to N(0,1), so "No harm, no foul" -- Student-t will be approximately
correct.  But it won't be exactly correct, in spite of the claim.

## <a name="asymp">Lesson SOMEMATH:  Some Derivations</a> 

As noted earlier, the goal of this tutorial is to develop within the
reader an understanding of the intuition underlying statistical concepts
of methods.  It is not a tutorial on math stat.  Nevertheless, it's
important to show a few derivations.

**&#x100; is an unbiased estimator of &mu;:**

By the linerity of E(), 

E&#x100; = (1/n) &Sigma;<sub>i</sub><sup>n</sup> EX<sub>i</sub>
= (1/n) n &mu; = &mu; 

**Var(&#x100;) = (1/n) &sigma;<sup>2</sup>, where
&sigma;<sup>2</sup> is the population variance of X**

By the fact that the variance of a sum of independent random variables
is the sum of their variances,

Var(&#x100;) = (1/n<sup>2</sup>) &Sigma;<sub>i</sub><sup>n</sup>
Var(X<sub>i</sub>) = (1/n<sup>2</sup>) n &sigma;<sup>2</sup> = (1/n)
&sigma;<sup>2</sup>

**S<sup>2</sup> is a biased estimator of &sigma;<sup>2</sup></sup>**

For any random variable W (with finite variance), 

Var(W) = E(W<sup>2</sup>) - (EW)<sup>2</sup>  

We will use this fact multiple times.

And the sample analog (just algebraic manipulation) is

S<sup>2</sup> = (1/n) &Sigma;<sub>i</sub><sup>n</sup>
X<sub>i</sub><sup>2</sup> - &#x100;<sup>2</sup>

Applying E() to both sides of this last equation, we have

E(S<sup>2</sup>) =
(1/n) n E(X<sub>i</sub><sup>2</sup>) -
(Var(&#x100;) + &mu;<sup>2</sup>) =
(&sigma;<sup>2</sup> + &mu;<sup>2</sup>) - [(1/n) &sigma;<sup>2</sup> + &mu;<sup>2</sup>] =
[(n-1)/n] &sigma;<sup>2</sup>

## <a name="geyser">Lesson GEYSER:  Old Faithful Geyser Example</a> 

To start to make the concepts tangible, let's look at **faithful**, a
built-in dataset in R, recording eruptions of the Old Faithful geyser in
the US national park, Yellowstone.

The object consists of an R data frame with 2 columns, **eruptions** and
**waiting**, showing the eruption durations and times between eruptions.
The conceptual "population" here is the set of all Old Faithful
eruptions, past, present and future.

Let's start simple, with a CI for the population mean duration:

``` r
erps <- faithful$eruptions
sampleMean <- mean(erps)
s2 <- var(erps)  # this is s^2 not S^2
stdErr <- sqrt(s2/length(erps))
c(sampleMean - 1.96*stdErr, sampleMean + 1.96*stdErr)
# (3.35,3.62)
```

## <a name="distrs">Lesson ESTDISTRS:  Estimating Entire Distributions</a> 

Recall the *cumulative distribution function*` (cdf) of a random variable 

F<sub>X</sub>(t) = P(X <= t)

This is indeed a function; for different values of t, we get different
values of the cdf.

And as always, there are population and sample estimates.  If X is human
height, then F<sub>X</sub>(66.5) is the population proportion of people
with height at most 66.5 inches; the sample estimate of that quantity is
the corresponding population proportion.

Let's plot the ecdf for the geyser data.

``` r
plot(ecdf(erps))
```

![alt text](FaithfulCDF.png)

The sample estimate is called the *empirical cdf* (hence the name of the
function).  Note:  Since it consists of proportions, it is
unbiased.

What about estimating the probability density function (pdf)?  As noted,
pdfs do not really exist in practice, so "the" pdf here is the one that
best approximates the true population distribution, but from here on,
let's just speak of estimating "the" pdf.

Actually, the familiar histogram is a pdf estimator, provided we specify
that the total area under it is 1.0.

``` r
hist(erps,freq=FALSE)
```

![alt text](HistErps.png)

This is interesting, as it is bimodal.  There have been many geophysical
theories on this, e.g. postulating that there are 2 kinds of eruptions.

Histograms are considered rather crude estimators of a density, as they
are so choppy.  A more advanced approach is that of *kernel* density
estimators.  

In estimating the density at a given point t, kernel estimators work by
placing more weight on the data points that are closer to t, with the
weights being smooth functions of the distance to t.  A smooth curve
results.

But what does "close" mean?  Just like histograms have a *tuning
parameter* (called a *hyperparameter* in Machine Learning circles) in
the form of the bin width, kernel estimators have something called the
*bandwidth*.  Let's not go into the formula here, but the point is that
smaller bandwidths yield a more peaked graph, while larger values
produce flatter curves.

Here are the graphs for bandwidths of 0.25 and 0.75:

``` r
plot(density(erps,bw=0.25))
plot(density(erps,bw=0.75))

```

![alt text](DensErps025.png)

![alt text](DensErps075.png)

The first graph seems to clearly show 2 bells, but the second one is
rather ambiguous.  With even larger values for the bandwidth (try it
yourself), we see a single bell, no hint of 2.

How should we choose the bandwidth, or for that matter, the bin width?
If we make the bandwidth too large, some important "bumps" may not be
visible.  On the other hand, if we make it too small, this will just
expose sampling variability, thus displaying "false" bumps.

We'll address this (but unfortunately not answer it) next.

## <a name="trade">Lesson TRADE:  The Bias-Variance Tradeoff</a> 

In the above histogram of the **erps** data, the graph seems,
for example, to be increasing from 2.5 o 4.5.  Consider in particular
the bin from 3.5 to 4.

This suggests that the true population density curve f(t) is also rising
from 2.5 to 4.5, and in particular, from 3.5 to 4.  Yet the histogram
height is a constant from 3.5 to 4.  This likely would mean that
the true f(t) curve is higher than the histogram for t near 4, and lower
than the histogram for t near 3.5.  In other words:

> The histogram, as an estimate of f, is likely biased upward (i.e. >
> 0) near 3.5 and downward (< 0) near 4.

And what if the bin width were much narrower? Then the bias described
above would still exist, but would be smaller.

On the other hand, the narrower the bin, the fewer the number of data
points, and thus the more the bin's height will vary from sample to
sample.

In other words:

> There is a tradeoff here: Smaller bins produce smaller bias but larger
> variance (and the opposite for larger bins).

So, even though bias was seen not to be an issue in the context of CIs,
it *is* an issue here.

There is no good, universally agreed-to way to choose the bin size.
Various math stat people have done some theoretical work that has led to
suggestions, which you can try, such as setting **breaks='FD'** in your
call to **hist()**.

Where the bias-variance really becomes an isssue is in
prediction/machine learning contexts, to be covered later.

## <a name="predict">Lesson PREDICT:  Predictive Models</a> 

From the 19th century linear models to today's fancy machine learning 
(ML) algorithms, a major application of statistics has been prediction.  In
this lesson, we set the stage for discussing prediction.

Say we are predicting a scalar Y from (a possibly vector-valued) X.  Let
p(X) denote our prediction.  Classically, we wish to minimize the Mean
Squared Prediction Error,

E[(Y - p(X))<sup>2</sup>]

Note that E() will be the average over all possible (X,Y) pairs in the
population.  Other loss functions besides squared-error are possible,
but this one is mathematically tractable.

The minimizer is easily shown to be 

p(X) = E(Y | X)

Note that in the case of Y being an indicator variable, this reduces
to 

p(X) = P(Y = 1 | X)

Borrowing from the fact that a typical symbol for the mean is &mu;,
let's define the function

&mu;(t) = E(Y | X = t)

which, as noted, in the case of dichotomous Y becomes

&mu;(t) = P(Y =  | X = t)

This is called the *regression function of Y on X*.  Note that this is a
general term, NOT restricted just to the linear model.

Our goal, then, is to use our sample data

(X<sub>1</sub>,Y<sub>1</sub>),...  (X<sub>n</sub>,Y<sub>n</sub>)

to estimate &mu;(t).  Denote the sample estimate by m(t).  Keep in mind,
we are estimating an entire function here, one value for each t.

We will often take as a convenient example predicting weight Y from
height X, or a vector X = (height,age).

The Bias-Variance Tradeoff becomes key in such models.  The more
predictors we use (*features* in ML parlance),* the smaller the bias in
m(t) but the larger the variance.  If we keep adding features, at some
point the variance becomes dominant, and we overfi.

## <a name="lin">Lesson LIN:  Linear Predictive Model</a> 

One can show that (X,Y) has a multivariate normal distribution, then

* &mu;(t) is linear in t

* the conditional distribution of Y given X = t is normal

* Var(Y | X = t) is constant in t

So, classically one assumes that

&mu;(t) = 
&beta;<sub>0</sub> +
&beta;<sub>1</sub> +
...
&beta;<sub>p</sub> 

for a p-predictor model, where t = (t<sub></sub>,...,t<sub>p</sub>).

In vector form, our assumption is

&mu;(t) = &beta;'(1,t)

where &beta; and (1,t) are taken to be column vectors and ' means matrix
transpose.

Again, the &beta;<sub>i</sub> are population values

The sample estimates vector b is computed by minimizing

&Sigma;<sub>1</sub><sup>n</sup>
[Y<sub>i</sub> - b'X<sub>i</sub>]<sup>2</sup>

A closed-form solution exists, and is implemented in R as **lm()**.

**But what about that multivariate normal assumption?**  ML
approaches work better in many cases, because they don't have to make
such assumptions.

The answer is that the linear model is hugely popular, and often for
good reason:  In practice, &mu;(t) is often in fact approximately
linear.  And:

> A parametric model may perform better than a nonparametric one if
> the former is approximately correct.  Better to estimate a few values,
> e.g. &beta;, than infinitely many (one for each value of t).

Moreover, one can fit more complex models that are still linear, e.g.
a full quadratic model of the regression of weight on height and age,

mean wt =  
&beta;<sub>0</sub> +  
&beta;<sub>1</sub> ht +
&beta;<sub>2</sub> age +
&beta;<sub>3</sub> ht<sup>2</sup> +
&beta;<sub>4</sub> age<sup>2</sup> +
&beta;<sub>5</sub> ht age

This is still a linear model, as it is linear in &beta;.

## LICENSING

The document is covered by a 
[Creative Commons](http://creativecommons.org/licenses/by-nd/3.0/us/) license,
Creative Commons Attribution-No Derivative Works 3.0 United States 
![alt text](http://i.creativecommons.org/l/by-nd/3.0/us/88x31.png).  I have
written the document to be *used*, so readers, teachers and so on are
very welcome and encouraged to copy it verbatim.  Copyright is retained
by N. Matloff in all non-U.S. jurisdictions, but permission to use these
materials in teaching is still granted, provided the authorship and
licensing information here is displayed.  I would appreciate being
notified if you use this book for teaching, just so that I know the
materials are being put to use, but this is not required.  information
displayed.  No warranties are given or implied for this material.

