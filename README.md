
# fastStat: All of REAL Statistics

## *"Quick intro to (real) statistics for those who know probability"*

### Norm Matloff, Prof. of Computer Science, UC Davis; [my bio](http://heather.cs.ucdavis.edu/matloff.html)

(See notice at the end of this document regarding copyright.)

# WHO IS THIS FOR?

Students in computer science, engineering, mathematics and the like
typically take a course in calculus-based probability -- unconditional
and conditional probability, cdfs and density functions, expected value
and so on.  But later they have a need to use statistics, and find it's
a broader and more nuanced field than they had realized.

**This document will enable such people to quickly acquire the needed
concepts, with a good intuitive understanding.**  It is modeled after
my popular [fasteR tutorial](https://github.com/matloff/fasteR) for
quickly becoming proficient in R. 

By the way, the title, "All of REAL Statistics," is a play on the titles
of two excellent books, *All of Statistics* and *All of Nonparametric
Statistics*, by one of my favorite statisticians, Larry Wasserman.  Both
books are quite thin, making their "All of" titles ironic.  Well, my
short tutorial here is even more "all of" in that sense.

# What Is Really Going on in Statistics?

Professional statisticians, especially Statistics professors, may find
the presentation here to be a familiar story, but with an odd plot,
a different cast of characters, and a different ending. :-) 

Indeed, **many of readers of this
tutorial will be surprised to see that it does not contain many
equations.**  But sadly, many people know the mechanics of statistics
very well, without truly understanding an intuitive levels what those
equations are really doing, and this is our focus.

For instance, consider estimator bias. Students in a math stat course
learn the mathematical definition of bias, after which they learn that
the sample mean is unbiased and that the sample variance can be
adjusted to be unbiased.  But that is the last they hear of the issue.
Actually, most estimators are in fact biased, and lack "fixes" like that
of the sample variance.  Does it matter?  None of that is discussed in
textbooks and courses.

We will indeed do some math derivations here, but not at the outset, and
not highlighted.  This tutorial aims to explain the practical ISSUES. 
Many of these are rather generally known, but not written down in books.
Some are actually not widely known.  And some are entirely new ways of
looking at familiar statistical concepts and properties.

# Table of contents

- [Lesson SAMPLING:  The Notion of a Sample](#lesson-sampling--the-notion-of-a-sample)
- [Lesson NORMALETC:  The Role of Normal (Gaussian) and Other Parametric Distribution Families](#lesson-normaletc--the-role-of-normal-gaussian-and-other-parametric-distribution-families)
- [Lesson CONCEPTPOPS:  Conceptual Populations](#lesson-conceptpops--conceptual-populations)
- [Lesson STDERRS:  Standard Errors](#lesson-stderrs--standard-errors)
- [Lesson BIAS:  Bias, and Impact on Standard Errors](#lesson-bias--bias-and-impact-on-standard-errors)
- [Lesson INDICATORS:  Indicator Variables](#lesson-indicators--indicator-variables)
- [Lesson CI:  Confidence Intervals](#lesson-ci--confidence-intervals)
- [Lesson CIAPPROX:  Confidence Intervals from Asymptotics](#lesson-ciapprox--confidence-intervals-from-asymptotics)
- [Lesson CONVERGE:  More on Asymptotics](#lesson-converge--more-on-asymptotics)
- [Lesson SOMEMATH:  some derivations](#lesson-somemath--some-derivations)
- [Lesson SIG: significance testing](#lesson-sig-significance-testing)
- [Lesson GEYSER:  old faithful geyser example](#lesson-geyser--old-faithful-geyser-example)
- [Lesson MLEMM:  general methods of estimation](#lesson-mlemm--general-methods-of-estimation)
- [Lesson ESTDISTRS:  estimating entire distributions](#lesson-estdistrs--estimating-entire-distributions)
- [Lesson TRADE:  the bias-variance tradeoff](#lesson-trade--the-bias-variance-tradeoff)
- [Lesson MULTI:  multivariate distributions](#lesson-multi--multivariate-distributions)
- [Lesson PREDICT:  predictive modeling -- preliminaries](#lesson-predict--predictive-modeling----preliminaries)
- [Lesson MLB:  the mlb dataset](#lesson-mlb--the-mlb-dataset)
- [Lesson LIN:  predictive modeling -- linear](#lesson-lin--predictive-modeling----linear)
- [Lesson LOGIT:  predictive modeling -- logistic](#lesson-logit--predictive-modeling----logistic)
- [Lesson NBHR:  predictive modeling -- a feature neighborhood view of overfitting in ml](#lesson-polyml--predictive-modeling----a-feature-neighborhood-view-of-overfitting-in-ml)
- [Lesson POLYML:  predictive modeling -- a polynomial view of overfitting in ml](#lesson-polyml--predictive-modeling----a-polynomial-view-of-overfitting-in-ml)
- [Lesson OVER:  predictive modeling -- avoiding overfitting](#lesson-over--predictive-modeling----avoiding-overfitting)
- [Lesson NOWORRY:  predictive modeling -- ignoring overfitting](#lesson-over--predictive-modeling----ignoring-overfitting)


## Lesson SAMPLING:  the Notion of a Sample

we've all heard the term *margin of error* in an opinion poll.  it will
be discussed in detail in a later lesson, but what question is it
addressing?

say the poll consists of querying 1200 people.  these were drawn
randomly from some list, say a list of phone numbers.  the point is that
if we were to do this again, we would get 1200 other people, and the
percentage saying yes to our question would change.  thus we want to
have some idea as to how much our yes percentage varies from one sample
of 1200 people to another.  

(note: sampling 1200 people is not referred to as "1200 samples."  the
set of 1200 people is referred to as one sample of size 1200.) 

let's set some notation.  say we are interested in some quantity x, say
human height.  we take a sample of n people from a given target
population, and denote the value of x in the i<sup>th</sup> person in
our sample by x<sub>i</sub>.  if we sample with replacement (or if n is
small relative to the total population size), the x<sub>i</sub> are
independent random variables.  also, each x<sub>i</sub> has distribution
equal to that of the sampled population.  if, say 22.8% of people in
this population are taller than 70 inches, then p(x<sub>i</sub> > 70) =
0.228.

## Lesson NORMALETC:  the Role of Normal (Gaussian) and Other Parametric Distribution Families

in the last lesson, we talked about the distribution of x in the
population.  although the population is finite (more on this below) and
thus x is a discrete random variable, one often models x as continuous,
with its distribution being in the normal family.

why do this?

* histograms of x in many applications do look rather bell-shaped.  this
  may in turn be due to the central limit theorem (clt).  the clt says
that sums are approximately normal, and in the human height case, one
can think of the body as consisting of chunks whose heights sum to the
height of the person.  (the clt assumes i.i.d. summands, and the chunks
here would be neither independent nor indentically distributed, but
there are non-i.i.d. versions of the clt.)

* the early developers of statistics had no computers, and it turns out
  that the normal distribution family is quite mathematically tractable,
thus amenable to closed-form "exact" solutions.

* it is often the case in math that discrete quantities are approximated
  by continuous ones (also vice versa).

* a normal distribution is determined by two parameters, the mean and
  variance of the distribution.  without that assumption, we have many
  parameters, essentially infinitely many.  let f<sub>x</sub> be the cdf
  of x, i.e. f<sub>x</sub>(t) = p(x &le; t).  well, there are infinitely
  many possible values for t, thus infinitely many values of f<sub>x</sub>(t).
  but if we assume x is normal, those infinitely many values are all
  expressible in terms of just two numbers.

another popular model is the exponential distribution family.  you
probably learned in your probability course that it is "memoryless,"
which makes it a suitable model in some applications.

note that, as models, these are necessarily inexact.  no distribution in
practice is exactly normal, for instance.  no one is 900 feet tall, and
no one has a negative height.  for that matter, a normal distribution is
continuous, whereas x is discrete, for two reasons:

* we are sampling from a finite population.

* our measuring instruments have only finite precision.  if, say, x is
  bounded between 0 and 10, and is measured to 2 decimal places, x can
take on 1000 values, and thus is discrete.

but what are we estimating, in light of the fact that our model is only
approximate?  say for instance we model x as having a gamma
distribution.  then in some sense, depending on how we estimate, we are
estimating the gamma distribution that is closest to our true population
distribution.

## Lesson CONCEPTPOPS:  Conceptual Populations

in the opinion poll example, it is clear as to which population is
sampled.  in many applications, the issue is more conceptual.  if for
instance we run a clinical trial with 100 diabetic patients, we might
think of them as having been sampled from the population of all
diabetics, even though we did not actually select the patients in our
sample, whether randomly or otherwise.

this issue can become quite a challenge in, say, economic analysis.
if we have 10 years of annual data, i.e. n = 10, what population
is that a "sample" from?

## Lesson STDERRS:  Standard Errors

earlier we mentioned the "margin of error" in reporting the results of
opinion polls.  to make that notion concrete, let's first discuss a
related idea, *standard errors*.

we use our data x<sub>1</sub>,...,x<sub>n</sub> to estimate some
quantity of interest, say the proportion q of people in the population
who would answer yes to our poll if we had a chance to ask them all.
our estimate, q, would be the proportion of people in our sample who say
yes.  (extremely important note:  make sure to always carefully
distinguish between a population quantity, q in this case, and its
sample estimate, q here.)

we want to have some measure of how much q varies from one sample to
another.  of course, var(q) is such a measure.

say for now that the average of q, averaged over all possible
samples, is q.  for some samples, q > q, for others q <- q, but on
average we get q.  this relates to the issue of *bias*, which we will
turn to later, but for now, say we have this situation, i.e. eq = q.

**the key point:**  if var(q) is small, then q doesn't vary much from one
sample to another, and if eq = q, then for "most" sample, q should be
near q.  that exactly what we hope for!  we only have one sample, of
course, but if we know that q is usually near q, we feel
reasonably confident that the q from our particular sample is near q.

of course, the square root of any variance is called the *standard
deviation*.  in the case of an estimator, q here, we use the term
*standard error*.  in some cases, it will be only the approximate
standard deviation, in a sense to be seen below.

## Lesson BIAS:  Bias, and Impact on Standard Errors

in our last lesson, we assumed that eq = q.  we say that q is an 
*unbiased* estimator of q.  in english:  the average value of q over all
possible samples is q. 

in the above example, in which q is the sample proportion of yes's and
q is the correspondng population proportion, it does turn out that q is
unbiased.  in fact, any sample mean is an unbiased estimator for the
population mean.  (here x is 1 or 0, so the average of the x<sub>i</sub>
is the proportion of yes's.)  let's skip the derivation for now (we'll
have a derivations lesson later), so we can get to the larger issues.

unbiasedness at first seems to be a very desirable property.  it does
hold for some classical statistical methods, but does not hold for many
others, one of which is the sample variance, as follows.

say we wish to estimate the population variance &sigma;<sup>2</sup>.
(we are not necessarily assuming a normal distribution.)  in the
population, that is the average squared difference between the data and
their mean.  the sample analog is

s<sup>2</sup> = (1/n) &sigma;<sub>i</sub><sup>n</sup>
(x<sub>i</sub> - &#x100;)<sup>2</sup>

where &#x100; is the sample mean, (1/n)  &sigma;<sub>i</sub><sup>n</sup>
x<sub>i</sub>.

it can be shown that s<sup>2</sup> is biased:

e(s<sup>2</sup>) = [(n-1) / n] &sigma;<sup>2</sup>

the average value of s<sup>2</sup> over all samples is a little too
low.  the amount of bias is 

e(s<sup>2</sup>) -  &sigma;<sup>2</sup> = -1/n  &sigma;<sup>2</sup> 

this bothered the early developers of statistics, who then defined the
sample variance as

s<sup>2</sup> = (1/(n-1)) &sigma;<sub>i</sub><sup>n</sup>
(x<sub>i</sub> - &#x100;)<sup>2</sup>

(in the field of probability and statistics, it is customary to use
capital letters for random variables.  this is an exception.)

the standard error of &#x100; is

s.e.(&#x100;) = s/n<sup>0.5</sup>

(use s instead of s if you wish).

but most estimators are not only biased, but also lack simple
adjustments like that for s above.  so, one must accept bias in general,
and consider its implications.

in that light, let's return to the discussion of standard error in the
last lesson.  we stated that an estimator with small standard error
would likely be near the population value it is estimating, but note now
that that argument depended on the estimator being unbiased.

but it is much more optimistic, actually.  say an estimator has bias
of size o(1/n), as above.  typically the size of the standard error is
o(1/n<sup>0.5</sup>).  in other words, the bias is small relative to the
standard error, so the argument in the last lesson still holds.

## Lesson INDICATORS:  Indicator Variables

often x has only the values only 1 and 0, indicating the presence or
absence of some trait.  that is the case in the opinion poll, for
example, where the respondent replies yes (1) or not-yes (0).  such a
variable is called an *indicator variable*, as it indicates whether the
trait is present or not.

in this case, &#x100; reduces to the proportion of 1s, as with q, the
proportion of yes responses to the opinion poll.  after some algebraic
simplification, it turns out that 

s<sup>2</sup> = &#x100; (1-&#x100;) / n

(or use n-1 instead of n for s).

## Lesson CI:  Confidence Intervals

in our opinion poll example, q is called a *point estimate* of q.  we
would also like to have an interval estimate, which gives a range of
values.  if say in in an election, the results of an opinion poll are
reported as, "candidate x has support of 62.1% of the voters, with a
margin of 3.9%," it is saying,

> a 95% confidence interval (ci) for x's support is (58.2%,66.0%).

a key point is the meaning of "95% confident."  imagine forming this
interval on each possible sample from the given population.  then 95% of
the intervals would cover the true value, q.

of course, we do not collect all possible samples; we just have 1.  we
say we are 95% that q is in our particular interval in the above sense.
(a note on the phrasing "q is in our interval":  some may take this to
mean that q is random, which it is not; q is unknown but fixed.  the ci
is what varies from one sample to another.)

it's exactly like gambling.  we don't know whether our particular roll
of the dice will yield a winner, but if most rolls of the dice do so,
then we may be willing to go ahead.

## Lesson CIAPPROX:  Confidence Intervals from Asymptotics

the early developers of statistics defined a distribution family known
as *student's t*.  this supposedly can be used to form "exact" cis, i.e. the
probability of the ci covering the target population value is exactly
0.95, if the population distribution of x is normal.  student-t is
widely taught, and thus widely used.  **but that is just an illusion.**  as
pointed out earlier, no distribution in practice is exactly normal.

what saves the day, though, is the central limit theorem.  &#x100; is a
sum, which the clt tells us is asymptotically normally distributed
as n goes to infinity.

in other words, if we were to compute &#x100; on each of the possible
samples of size n from the population, and then plot the results in a
histogram, the graph would be approximately bell-shaped, and the
probabilities for any range of values would be approximately those of a
normal distribution.

ah, so we're in business:  for any random variable w, the quantity

(w - ew) / (var(w)<sup>0.5</sup>

has mean 0 and variance 1.  (this has nothing to do with whether w is
normal or not.)  so,

z = (&#x100; - &mu;) / s.e.(&#x100;)

has mean 0 and variance 1, where &mu; is the population mean of x.
(recall that &#x100; is unbiased for &mu;.)  and since z actually does
have an approximately normal distribution, its distribution is thus
approximately n(0,1), i.e. normal with mean 0 and variance 1..  

now since the n(0,1) distribution has 95% of its area between -1.96 and
1.96, we have

0.95 &approx; p[-1.96 < (&#x100; - &mu;) / s.e.(&#x100;) < 1.96]

which after algebra becomes

0.95 &approx; p[
&#x100; - 1.96 s.e.(&#x100;) < &mu; <
&#x100; + 1.96 s.e.(&#x100;))

there's our ci for &mu;!

(&#x100; - 1.96 s.e.(&#x100;), &#x100; + 1.96 s.e.(&#x100;))

we are (approximately) 95% confident that the true population mean is in
that interval.  (remember. it is the interval that is random, not &mu;)

and things don't stop there.  actually, many types of estimators have
some kinds of sums within them, and thus have an approximately normal
distribution, provided the estimator is a smooth function of those sums.
(a taylor series approximation results in a linear function of normal
random variable, thus again normal.)

thus approximate cis can be found for lots of different
estimators, notably maximum likelihood estimators and least-squares
parametric regression estimators, as we will see in later lessons.

in other words, we have the following principle (the name is mine, the
principle general):

**the fundamental tool of statistical inference:**

> if r is a "smooth" estimator of a population quantity r consisting of
> sums, based on an i.i.d. sample, then an approximate 95% confidence
> interval for r is
> 
> (r - 1.96 s.e.(r), r + 1.96 s.e.(r)

the term *smooth* roughly means that r is a differentiable function of
x<sub>1</sub>, ..., x<sub>n</sub>.

by the way, for large n, the student-t distribution is almost identical
to n(0,1), so "no harm, no foul" -- student-t will be approximately
correct.  but it won't be exactly correct, in spite of the claim.

## Lesson CONVERGE:  More on Asymptotics

the concept of standard error needs to be clarified.

what does the clt say, exactly?  say s<sub>n</sub> is the sum of iid
random variables, each with mean &mu; and variance &sigma;<sup>2</sup>.
then 

z<sub>n</sub> = s<sub>n</sub> / [&sigma; / sqrt(n)] 

*converges in distribution* to n(0,1), **meaning that** the cdf of z<sub>n</sub>
converges to the n(0,1) cdf.

in other words, the asymptotics apply to probabilities--but not to
expected values etc.  two random variables can have almost the same cdf
but have very different means, variances and so on.  e.g. take any
random variable and shift a small amount of its probability mass from x
= c to some huge number c =d; almost the same cdf, very different mean.

so, the asymptotic statement about regarding r and r, enables us to
compute valid cis based on the n(0,1) cdf.  but the standard error used
in that interval, s.e.(r), is not necessarily the standard deviation of
r. 

## Lesson SOMEMATH:  Some Derivations

as noted earlier, the goal of this tutorial is to develop within the
reader an understanding of the intuition underlying statistical concepts
or methods.  it is not a tutorial on math stat.  nevertheless, it's
important to show a few derivations.

**&#x100; is an unbiased estimator of &mu;:**

by the linerity of e(), 

e&#x100; = (1/n) &sigma;<sub>i</sub><sup>n</sup> ex<sub>i</sub>
= (1/n) n &mu; = &mu; 

**var(&#x100;) = (1/n) &sigma;<sup>2</sup>, where
&sigma;<sup>2</sup> is the population variance of x**

by the fact that the variance of a sum of independent random variables
is the sum of their variances,

var(&#x100;) = (1/n<sup>2</sup>) &sigma;<sub>i</sub><sup>n</sup>
var(x<sub>i</sub>) = (1/n<sup>2</sup>) n &sigma;<sup>2</sup> = (1/n)
&sigma;<sup>2</sup>

**s<sup>2</sup> is a biased estimator of &sigma;<sup>2</sup></sup>**

for any random variable w (with finite variance), 

var(w) = e(w<sup>2</sup>) - (ew)<sup>2</sup>  

we will use this fact multiple times.

and the sample analog (just algebraic manipulation) is

s<sup>2</sup> = (1/n) &sigma;<sub>i</sub><sup>n</sup>
x<sub>i</sub><sup>2</sup> - &#x100;<sup>2</sup>

applying e() to both sides of this last equation, we have

e(s<sup>2</sup>) =
(1/n) n e(x<sub>i</sub><sup>2</sup>) -
(var(&#x100;) + &mu;<sup>2</sup>) =
(&sigma;<sup>2</sup> + &mu;<sup>2</sup>) - [(1/n) &sigma;<sup>2</sup> + &mu;<sup>2</sup>] =
[(n-1)/n] &sigma;<sup>2</sup>

## Lesson SIG: Significance Testing 

the notion of *signficance testing* (st) (also known as
*hypothesis testing*, and *p-values*) is
one of the real oddities in statistics.  

* on the one hand, statisticians are well aware of the fact that st can
  be highly miseadling.

* but on the other hand, they teach st with ilttle or
no warning about its dangers.  as a result, its use is widespread.

a few years ago, the american statistical association released its
first-ever position paper on any topic, stating what everyone had known
for decades: st is just not a good tool.

the stopped just short of recommending fully against st,
but it is my position that st should simply not be used.  instead, analysis
should be based on cis, as explained later in this lesson..  

but first, what is st?  to keep things simple, let's say we have some
coin, and want to test the hypothesis h<sub>0</sub>: r = 0.5, where r is
the probability of heads.  we will toss the coin n times, and set r to
the proportion of heads in our sample.

we take an "innocent until proven guilty" approach, clinging to our
belief in h<sub>0</sub> until/unless we see very strong evidence to the
contrary.  well, what constituters "strong"?

we look at the ratio z = (r-0.5) / s.e.(r).  r is approximately normal,
as noted earlier, and under h<sub>0</sub> r has mean 0.  so under
h<sub>0</sub>, w ~ n(0,1).  (tilde is standard notation for "distributed
as.")

so, p(|z| > 1.96) &approx; 0.05 and 5% is the traditional standard for
"strong evidence."  so we reject h<sub>0</sub> if and only if |z| >
1.96.

and we can get greedy.  what if, say, we find that r = 2.2?  note:

``` r
> pnorm(-2.2)
[1] 0.01390345
```

under h<sub>0</sub>, p(|z| > 2.2) &approx; 0.028.  ah, we would have
rejected h<sub>0</sub> even under the more stringent "strong evidence"
criterion of 0.028.  so we report 0.028, known as the *p-value*.  the
smaller the p-value, the more *signficant* we declare our finding.

well, what's wrong with that?

* we know *a priori* that h<sub>0</sub> is false.  no coin is exactly
  balanced.  so it's rather silly to ask the question regarding
  h<sub>0</sub>.

* we might be interested in knowing whether the coin is *approximately*
  balanced.  fine, but the st is not addressing that question.  even if
r is just a little different from 0, then as the number of tosses n goes
to infinity, the denominator in z goes to 0, and thus r goes to
&pm;&infin;--and the p-value goes to 0.

* in such a scenario, we declare that the coin is "signficantly" 
unbalanced, an egregiously misleading statement.

* we have the opposite problem with small n.  we will declare "no
  signficant difference" when we actually should say, "we have too
little data to make any claims."

one reason that st is so appealing is that it allows the user to be
lazy.  say we have two drugs, an old one a and a new one b, for treating
high blood pressure.  the user here may be a government agency, deciding
whether to approve the new drug.

let &mu;<sub>a</sub> and &mu;<sub>b</sub> denote the population mean
effects of the two drugs.  then the null hypothesis is

h<sub>0</sub>: &mu;<sub>a</sub> = &mu;<sub>b</sub>

again, we know *a priori* that h<sub>0</sub> must be false--the two
means can't be equal, to infinitely many decimal places--so the test is
meaningless in the first place, and we will have the problems described
above.  but, forget all that--the test gives us an answer, and the user
is happy.

a more careful analysis would be based on forming a ci for the
difference &mu;<sub>a</sub> - &mu;<sub>b</sub>.  that gives us a range
of values, against which we can weigh things like cost differences
between the two drugs, possible side effects and so on.  in addition,
the width of the interval gives the user an idea as to whether there is
enough data for accurate estimation of the means.

yes, the user does have to make a decision, but it is an informed
decision.  in other words:

> the user is taking responsibility, making an informed decision,
> instead of allowing a poor statistical procedure to make the decision
> for him/her.

note by the way that what we are not doing is
"check to see whether the ci contains 0.")

see 
[this document](https://github.com/matloff/regtools/blob/master/inst/nopvals.md)
for further details.  


## Lesson GEYSER:  Old Faithful Geyser Example

to start to make the concepts tangible, let's look at **faithful**, a
built-in dataset in r, recording eruptions of the old faithful geyser in
the us national park, yellowstone.

the object consists of an r data frame with 2 columns, **eruptions** and
**waiting**, showing the eruption durations and times between eruptions.
the conceptual "population" here is the set of all old faithful
eruptions, past, present and future.

let's start simple, with a ci for the population mean duration:

``` r
erps <- faithful$eruptions
samplemean <- mean(erps)
s2 <- var(erps)  # this is s^2 not s^2
stderr <- sqrt(s2/length(erps))
c(samplemean - 1.96*stderr, samplemean + 1.96*stderr)
# (3.35,3.62)
```

# Lesson MLEMM:  General Methods of Estimation

so far, we've discussed only ad hoc estimators, set up for a specific
purpose.  it would be nice to have general ways of forming estimators.
the two most common such techniques are *maximum likelihood estimation*
(mle) and the *method of moments* (mm).  let's illustrate them with a
simple example, mm first.

**the method of moments**

> say we are estimating some parameter &theta; with the estimator to be
> found and named t.  then, in the formal expression for ex, replace
> &theta; by t; set the result to the sample mean &#x100; and solve for t.

say we are modeling our data x<sub>i</sub> as coming from a population
with an exponential distribution, i.e.

f<sub>x</sub>(t) = &lambda;e<sup>-&lambda;t</sup>, t > 0

it is well known (and easy to derive) that ex = 1 / &lambda;.  denote
our estimator of &lambda; by l.  then the above recipe gives us

1 / l = &#x100;

and thus 

l = 1 / &#x100;

it may be that our parametric model has 2 parameters rather than 1,
so now  &theta; = (&theta;<sub>1</sub>,&theta;<sub>2</sub>) and
t = (t<sub>1</sub>,t<sub>2</sub>).
then in addition to generating an equation for the sample mean as above,
we also generate a second equation for the sample variance in terms of
the &theta;<sub>i</sub>; we replace the &theta;<sub>i</sub> by
t<sub>i</sub>; on the right-hand side, we write the sample variance.
that gives us 2 equations in 2 unknowns, and we solve for the
t<sub>i</sub>.

there are variations.  e.g. instead of the variance and sample variance, we
can use the *second moment*, e(x<sup>k</sup>) and its sample analog

(1/n) &sigma;<sub>i</sub><sup>n</sup> x<sub>i</sub><sup>k</sup>

this may be easier if, say, we have 3 parameters to estimate.

**the method of maximum likelihood**

discrete case:

> say we model the population as having a probability mass function
> p<sub>x</sub> that depends on some (scalar or vector) parameter &theta;.
> calculate the probability (i.e. "likelihood") of our observed data, as a
> function of &theta;.  replace &theta; by t everywhere in that
> expression; and finally, then take the mle to be whatever value of t
> maximizes that likelihood.

if our x<sub>i</sub> are independent, then the likelihood expression is

&pi;<sub>i</sub><sup>n</sup> p(x<sub>i</sub>)

we take the logarithm, to make derivatives easier.  

continuous case:

a density function is not a probability -- it can be larger than 1 --
but it does work roughly as a likelihood; if say f(12.3) is large, then
one will see many x that are near 12.3.  so, in the above prescription,
replace p by f.

*asymptotic normality:*  

we mentioned earlier that the clt not only applies to sums but
also extends to smooth (i.e. differentiable) functions of sums.
that fact is important here.  how does that play out with mms and mles?

mms: by definition, we are working with sums! 

mle: the log-likelihood is a sum!

again, we must be working with smooth functions.  consider the *negative
binomial* distribution family,

p(x = j) = c(n-1,i-1) r<sup>i</sup> (1-r)<sup>n-i</sup>

this arises, say, from tossing a coin until we accumulate k heads.

if our parameter is r, with known k, the above is a differentiable
function of r.  but if we don't know k, say we have the data but did not
collect it ourselves, then we don't have differentiability in that
parameter.

*r **mle()** function:*

this function will calculute mles and give standard errors, in smooth
cases.

``` r
> library(stats4)
> n <- 100
> x <- rexp(n,2)
> ll  # user supplies the log likelihood function
function(lamb) {
   loglik <- n*log(lamb) - lamb*sum(x)
   -loglik
}
<bytecode: 0x105ae8368>
> z <- mle(minuslogl=ll,start=list(lamb=1))
> summary(z)
maximum likelihood estimation

call:
mle(minuslogl = ll, start = list(lamb = 1))

coefficients:
     estimate std. error
lamb    2.195     0.2195

-2 log l: 42.76 

```

*extent of usage:*

mles have appealing theoretical properties.  in smooth settings, they
are optimal, i.e. minimal standard error.  they are quite widely used.

mms are less popular.  but they are easier to explain, and by the way,
the inventor of the generalized method of moments won the nobel prize in
econoomics in 2013 that developing that method.

## Lesson ESTDISTRS:  Estimating Entire Distributions

recall the *cumulative distribution function*` (cdf) of a random variable 

f<sub>x</sub>(t) = p(x &le; t)

this is indeed a function; for different values of t, we get different
values of the cdf.

and as always, there are population and sample estimates.  if x is human
height, then f<sub>x</sub>(66.5) is the population proportion of people
with height at most 66.5 inches; the sample estimate of that quantity is
the corresponding sample proportion.

let's plot the ecdf for the geyser data.

``` r
plot(ecdf(erps))
```

![alt text](faithfulcdf.png)

the sample estimate is called the *empirical cdf* (hence the name of the
function).  note:  since it consists of proportions, it is
unbiased.

what about estimating the probability density function (pdf)?  as noted,
pdfs do not really exist in practice, so "the" pdf here is the one that
best approximates the true population distribution, but from here on,
let's just speak of estimating "the" pdf.

actually, the familiar histogram is a pdf estimator, provided we specify
that the total area under it is 1.0.  here it is for the geyser data:

``` r
hist(erps,freq=false)
```

![alt text](histerps.png)

this is interesting, as it is bimodal.  there have been many geophysical
theories on this, e.g. postulating that there are 2 kinds of eruptions.

histograms are considered rather crude estimators of a density, as they
are so choppy.  a more advanced approach is that of *kernel* density
estimators.  

in estimating the density at a given point t, kernel estimators work by
placing more weight on the data points that are closer to t, with the
weights being smooth functions of the distance to t.  a smooth curve
results.

but what does "close" mean?  just like histograms have a *tuning
parameter* (called a *hyperparameter* in machine learning circles) in
the form of the bin width, kernel estimators have something called the
*bandwidth*.  let's not go into the formula here, but the point is that
smaller bandwidths yield a more peaked graph, while larger values
produce flatter curves.

here are the graphs for bandwidths of 0.25 and 0.75:

``` r
plot(density(erps,bw=0.25))
plot(density(erps,bw=0.75))

```

![alt text](denserps025.png)

![alt text](denserps075.png)

the first graph seems to clearly show 2 bells, but the second one is
rather ambiguous.  with even larger values for the bandwidth (try it
yourself), we see a single bell, no hint of 2.

how should we choose the bandwidth, or for that matter, the bin width?
if we make the bandwidth too large, some important "bumps" may not be
visible.  on the other hand, if we make it too small, this will just
expose sampling variability, thus displaying "false" bumps.

we'll address this (but unfortunately not answer it) next.

# Lesson TRADE:  the Bias-Variance Tradeoff

in the above histogram of the **erps** data, the graph seems,
for example, to be increasing from 2.5 o 4.5.  consider in particular
the bin from 3.5 to 4.

the increasing nature of the histogram in that region suggests that the
true population density curve f(t) is also rising from 2.5 to 4.5, and
in particular, from 3.5 to 4.  yet the histogram height is a constant
from 3.5 to 4.  this likely would mean that the true f(t) curve is
higher than the histogram for t near 4, and lower than the histogram for
t near 3.5.  in other words:

> the histogram, as an estimate of f, is likely biased upward (i.e. 
> bias > 0) near 3.5 and downward (< 0) near 4.

and what if the bin width were much narrower? then the bias described
above would still exist, but would be smaller.

on the other hand, the narrower the bin, the fewer the number of data
points in each bin, so the greater the standard error of the histogram
height within a bin.  (the "n" for the standard error is the number of
points in the bin.)

in other words:

> there is a tradeoff here: smaller bins produce smaller bias but larger
> variance (and the opposite for larger bins).

so, even though bias was seen not to be an issue in the context of cis,
it *is* an issue here.

there is no good, universally agreed-to way to choose the bin size.
various math stat people have done some theoretical work that has led to
suggestions, which you can try, such as setting **breaks='fd'** in your
call to **hist()**.

where the bias-variance really becomes an isssue is in
prediction/machine learning contexts, to be covered later.

# Lesson MULTI:  Multivariate Distributions

say we have continuous random variables x and y.  we of course can talk
about their density functions f<sub>x</sub> and f<sub>y</sub>, but it's
also important to talk about how they vary (or not) *together*.  here
are a few facts:

* f<sub>x,y</sub>(u,v) = d/&part;u d/&part;v f<sub>x,y</sub>(u,v) =
p(x &le;u and y &le;v)

* p((x,y) in a) = &int; &int;<sub>a</sub> f<sub>x,y</sub>(u,v) du dv

* f<sub>y | x = v</sub>(u) = f<sub>x,y</sub>(u,v) / f<sub>x</sub>(u)

* unlike the univariate case, there are very few widely-used parametric
  families of multivariate distributions.  the main one is multivariate
normal.

* the mv normal family does have some interesting properties, though:

    - all marginal distributions are mv normal

    - ax is mv normal for mv normal x and constant matrix a

    - conditional distributions, given a singleton value, are mv normal

    - those conditional distributions have mean that is linear in the
      conditioning value, and variance (or covariance matrix) that does
      not depend on that value

* one common measure of the relation between variables u and v is their
  (pearson) *correlation*, e(u - eu)`(v - ev)] / sqrt[var(u var(v)].  they
  need not be normal.  this is a quanttty in [-1,1].

# Lesson PREDICT:  Predictive Modeling -- Preliminaries

from the 19th century linear models to today's fancy machine learning 
(ml) algorithms, a major application of statistics has been prediction.  in
this lesson, we set the stage for discussing prediction.

say we are predicting a scalar y from (a typically vector-valued) x.  let
p(x) denote our prediction.  classically, we wish to minimize the mean
squared prediction error,

e[(y - p(x))<sup>2</sup>]

note that e() will be the average over all possible (x,y) pairs in the
population.  other loss functions besides squared-error are possible,
but this one is mathematically tractable.

the minimizer is easily shown to be 

p(x) = e(y | x)

note that in the case of y being an indicator variable, this reduces
to 

p(x) = p(y = 1 | x)

borrowing from the fact that a typical symbol for the mean is &mu;,
let's define the function

&mu;(t) = e(y | x = t)

which, as noted, in the case of dichotomous y becomes

&mu;(t) = p(y =  | x = t)

this is called the *regression function of y on x*.  note that this is a
general term, not restricted just to the linear model.

our goal, then, is to use our sample data

(x<sub>1</sub>,y<sub>1</sub>),...  (x<sub>n</sub>,y<sub>n</sub>)

to estimate &mu;(t).  denote the sample estimate by m(t).  to predict a
new case, x<sub>new</sub>, we use m(x<sub>new</sub>).  keep in mind,
m(t) is an estimate of  an entire function here, one value for each t.

we will often take as a convenient example predicting weight y from
height x, or a vector x = (height,age).

the bias-variance tradeoff becomes key in such models.  the more
predictors we use (*features* in ml parlance),* the smaller the bias in
m(t) but the larger the variance.  if we keep adding features, at some
point the variance becomes dominant, and we overfit.

# Lesson MLB:  the MLB Dataset

this data, on major league baseball players in the us, is included with
my [**qeml** package](github.com/matloff/qeml) ("quick ml").

``` r
> head(mlb)
        position height weight   age
1        catcher     74    180 22.99
2        catcher     74    215 34.69
3        catcher     72    210 30.78
4  first_baseman     72    210 35.43
5  first_baseman     73    188 35.71
```

we'll usually use just height, weight and age.

# Lesson LIN:  Predictive Modeling -- Linear

as noted, if (x,y) has a multivariate normal distribution, then
the following attributes hold:

* linearity of the regresion function:  &mu;(t) is linear in t

* conditional normality of y given x = t 

* conditional homogeneous variance:  var(y | x = t) is constant in t

so, classically one assumes that

&mu;(t) = 
&beta;<sub>0</sub> +
&beta;<sub>1</sub> +
...
&beta;<sub>p</sub> 

for a p-predictor model, where t = (t<sub></sub>,...,t<sub>p</sub>).

in vector form, our assumption is

&mu;(t) = &beta;'(1,t)

where &beta; and (1,t) are taken to be column vectors and ' means matrix
transpose.

again, the &beta;<sub>i</sub> are population values.

the sample estimates vector b is computed by minimizing

&sigma;<sub>1</sub><sup>n</sup>
[y<sub>i</sub> - b'(1,x<sub>i</sub>)]<sup>2</sup>

a closed-form solution exists, and is implemented in r as **lm()**.

``` r
> z <- lm(weight ~ height+age,data=mlb)
> summary(z)

call:
lm(formula = weight ~ height + age, data = mlb)

residuals:
    min      1q  median      3q     max 
-50.535 -12.297  -0.297  10.824  74.300 

coefficients:
             estimate std. error t value pr(>|t|)    
(intercept) -187.6382    17.9447  -10.46  < 2e-16 ***
height         4.9236     0.2344   21.00  < 2e-16 ***
age            0.9115     0.1257    7.25 8.25e-13 ***
---
signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
...
```

note the standard errors.  an approximate 95% confidence interval for 
&beta;<sub>age</sub> is

``` r
> rad <- 1.96*0.1257
> c(0.9115 - rad, 0.9115 + rad)
[1] 0.665128 1.157872

```

ah, even these professional athletes tend to gain weight as they age,
something like a pound per year.

**but what about those above classical assumptions for the linear model?**  do they matter?

on the one hand, the answer is "not much."  

* the formula for the least-squares estimator b consists of various
  sums, so that the central limit theorem tells us that b is
approximaely normal even if the conditional normality assumption doesn't
hold.

* violations of the homogeneous conditional variance assumption can be
  handled by *sandwich estimators*, e.g.

``` r
> library(sandwich)
> vcovhc(z)
            (intercept)       height          age
(intercept) 323.5135765 -4.151528253 -0.629074510
height       -4.1515283  0.055692865  0.002166495
age          -0.6290745  0.002166495  0.015967653

```

a more accurate standard error for &beta;<sub>age</sub> is thus

``` r
> sqrt(0.015967653)
[1] 0.1263632
```

(not much change here, but there is a larger difference in some cases).

on the other hand, those considerations are important if our goal is
*understanding*, e.g. understanding weight gain in pro ball players.
for the *prediction* goal, those issues are irrelevant; all that really
matters is whether the true &mu;(t) is approximately linear.  (and if it
isn't, we aren't achieving the understanding goal either; our cis will
be invalid.)  that would seem to argue in favor of using ml models,
which do not make linearity assumptions.

but there's more:

one can fit more complex models that are still linear, e.g.
a full quadratic model of the regression of weight on height and age,

mean wt =  &beta;<sub>0</sub> +  &beta;<sub>1</sub> ht + &beta;<sub>2</sub> age + &beta;<sub>3</sub> ht<sup>2</sup> + &beta;<sub>4</sub> age<sup>2</sup> + &beta;<sub>5</sub> ht age

this is still a linear model, as it is linear in &beta;, even though it
is nonlinear in height and age.

we can fit polynomial models using the 
[polyreg](https://cran.rstudio.com/web/packages/polyreg/index.html)
package (or use **qeml::qepoly()**).  but as we fit polynomials of
higher and higher degree, we quickly run into the bias-variance
tradeoff.  the variance becomes large, i.e. the fitted coefficients will
vary a lot from one sample to another, while the reduction in bias slows
down.  so we risk overfitting.

in that case, a nonparametric model, such as from ml, may work much
better.  however, keep in mind that ml models can overfit too.  more on
this shortly.

# Lesson LOGIT:  Predictive Modeling -- Logistic

what about the case in which y is an indicator variable, say diabetic (y
= 1) vs. nondiabetic (y = 0)?  

recall that in this setting, &mu;(t) reduces to p(y = 1 | x = t).  that
makes a linear model untenable, as it would produce values outside the
[0,1] range for probabilities.[

as we did in the lesson on the linear model, again let's start
with the classical normal-distribution based model.  this is known as
*fisher linear discriminant analysis* (lda).

here, the distribution of x, given that y = i, is assumed multivariate 
normal with mean vector &nu;<sub>i</sub> and covariance matrix c that
does not depend on i.  let r denote p(y = 1) (unconditional).

if one then applies bayes' rule (just a probability calculation,
not bayesian statistics), one finds that 

p(y = 1 | x = t)

has a *logistic* form:  if x we have a single predictor (so
&nu;<sub>i</sub> is a scalar and c is just the standard deviation
&sigma;), the form is

p(y = 1 
| x = t) = 1 / [1 + exp(-{&omega;<sub>0</sub> + &omega;<sub>1</sub>t)}]

for parameters &omega;<sub>0</sub> and &omega;<sub>1</sub>
that depend on the v<sub>i</sub> and &sigma;

in the case of multiple features, this is

p(y = 1 | x = t) = 1 / [1 + exp(-&omega;'t)]

(let's call the above, "the logistic equation.")

again as in the case of the linear model discussion above, we must ask,
**what if the assumptions, e.g. normality, don't hold?**

and the answer is that the logistic model (often called *logit*) is
quite popular.  the logistic equation does produce a value in (0,1),
which is what we want for a probability of course, yet we still have a
linear ingredient &omega;'t, the next-best thing to a pure linear model.

moreover, as in the linear case, we can introduce polynomial terms to
reduce model bias.  (it will turn out below that there is also a "polynomial
connection" to some popular ml methods, svm and neural networks.)

by the way, many books present the logit model in terms of "linear
log-odds ratio."  the odds ratio is 

p(y = 1 | x = t) / [p(y = 0 | x = t)]

while it is true that the log of that quantity is linear in the logit
model, this description seems indirect.  we might be interested in the
odds, but why would the *logarithm* of the odds be of interest?  it's
best to stick to the basics:

* a model for a probability should produce values in (0,1).

* it would be nice if the model has an ingredient the familiar linear
  form.

logit, which we have handy from lda, satisfies those desiderata.

# Lesson NBHR:  Predictive Modeling -- a Feature Neighborhood View of Overfitting in ml

one of the simplest ml methods is k-nearest neighbors.  say we are
predicting weight from height and age, and must do so for a new case in
which x = (70,28).  then we find the k rows in our training data that
are closest to (70,28), find the average weight among those data points,
and use that average as our predicted value for the new case.

the bias-variance tradeoff here is clear (and similar to the density
estimation example above).  if k is large, the neighborhood of (70,28)
will be large, and thus will contain some unrepresentative points that
are far from (70,28).  if k is small, the sample mean in our
neighborhood will have a large standard error, as its "n" (k here) is
small.

the situation is similar for tree-based methods (cart, random forests,
gradient boosting).  as we move down a tree, we add more and more
features to our collection, and the nodes have less and less data.  for
a fixed amount of data, the more levels in the tree, the fewer data
points in each of the leaves.  since the leaves are similar to
neighborhoods in k-nn, we see the same bias-variance tradeoff.

# Lesson POLYML:  Predictive Modeling -- a Polynomial View of Overfitting in ML

in Lesson LIN, We discussed overfitting in the context of
polynomial regression.  here polynomials will give us a look into how ml
algorithms can also overfit, specifically in support vector machines 
(svm) and neural networks (nns).

svm is used mainly in classification contexts. here we will assume just
two classes, for simplicity.                                                 

the idea behind svm is very simple. think of the case of two features.
we can plot the situation as follows.  consider this graph, in the
context of predicting diabetes, from blood glucose and age:

![alt text](pimaglucagediab.png)

the red dots are the diabetics.  we would like to draw a line so that
most of the red dots are on one side of the line and most of the black
ones are on the other side.  we then use that line too predict future
cases.  of course if we have more than two features the line becomes a
plane or hyperplane.                    

well, why just limit ourselves to a straight line? why not make it a
quadratic curve, a cubic curve and so on?  each one gives us more
flexibility than the last, hence a potentially better fit.

that is exactly what svm methods do, apply a transformation (a *kernel*)
to the features and then find a straight line separating the transformed
data. this is equivalent to forming a curvy line in the original space. 

clearly we can have a situation in which the line may get so curvy that
it "fits the noise rather than the data," as they say. there is a true
population curve, consisting of all the points t for which &mu;(t) =
0.5, and the curvier we allow our fit, the smaller the bias.  but as in
the linear case, the curvier the fit, the larger the variance.  the
fitted curve varies too much from one sample to another.

and though we motivated the above discussion by using polynomial curves,
just about any common kernel function can be approximated by
polynomials.  the principle is the same.

in the case of nns, we have a set of layers. think of them as being
arranged from left to right. the input data goes in on the left side and
the predicted values come out on the right side. the output of each
layer gets fed in as input to the next layer, being fed through some
transformation (an *activation function*). at each layer, in essence the
input is sent through a linear regression estimation, with the
result then passed on to the next layer.

suppose the activation function is a(t) = t<sup>2</sup>.  this is not a
common choice at all, but it will make the point.  the output of the
first layer in our example here is a linear combination of the glucose
and age features.  the activation function squares that, giving us a
quadratic polynomial in glucose and age.  after the second stage, we
will have a quartic (i.e. fourth degree) polynomial and so on.

and again, almost any activation function can be approximated by a
polynomial, so the same argument holds.  so again it's clear how easily
we can run into overfitting.                           

a very common activation function today is relu, which is piecewise
linear:  a(t) = t for t > 0, but = 0 for t < 0.  this has the effect of
partitioning the x space like a patchwork quilt, except that the
"patches" are of irregular shape.  well, again:  the more layers we
have, the smaller the "patches," i.e. the smaller the neighborhoods.  so
the situation is just like that of k-nn etc.

so basically we are doing a lot of linear regression fits and
as this goes from layer to layer, they gets multiplied. you're
basically building up polynomials of higher and higher degree. 

# Lesson OVER:  Predictive Modeling -- Avoiding Overfitting

how can we try to avoid overfitting?

* most regression models, both parametric and ml, have regularized
versions, again, to guard against extreme data having too much impact.
in the linear case, we have *ridge regression* and the lasso.
regularized versions of svm, nns etc. also exist.

* we can do *dimension reduction* by mapping our features into a 
lower-dimensional subspace, e.g. via principal components analysis of
umap.

* we cam do *cross validation*:  take many subsamples of the data (i.e.
  a subset of the rows).  in each one fit, our model to the subsample
  and use the result to predict the remaining data.  in this way, choose
  among competing models.

# Lesson NOWORRY:  Predictive Modeling -- Ignoring Overfitting

some people take a very different point of view: in some settings, we
can overfit and not worry about it.  a common claim is "ml models
drastically overfit, yet predict new cases very well."  

this is misleading, in my opinion.  in the celebrated ml successes, the
misclasification rate is very small, even without hyperparameter tuning.
in the svm sense, this means that the two classes are almost completely
separable after transformation of x.  thus **many different straight
lines or hyperplanes can achieveo the separation,** which means that
back in the original x space, there are many separating curves,
including some of very high complexity.  in other words, due to the
separability of the data, we can get away with overfitting.

here is an example, using a famous image recognition dataset
(generated by code available [here](https://jlmelville.github.io/uwot/metric-learning.html):

![alt text](umapfmnist.png)

there are 10 classes, each shown in a different color.  here the sne
method (think of it as a nonlinear pca) was applied for dimension
reduction, dimension 2 here.  there are some isolated points here and
there, but almost all the data is separated into 10 "islands."  between
any 2 islands, there are tons of high-dimensonal curves, say high-degree
polynomials, that one can fit to separate them.  so we see that
overfitting is just not an issue, even with high-degree polynomials.


# licensing

the document is covered by a 
[creative commons](http://creativecommons.org/licenses/by-nd/3.0/us/) license,
creative commons attribution-no derivative works 3.0 united states 
![alt text](http://i.creativecommons.org/l/by-nd/3.0/us/88x31.png).  i have
written the document to be *used*, so readers, teachers and so on are
very welcome and encouraged to copy it verbatim.  copyright is retained
by n. matloff in all non-u.s. jurisdictions, but permission to use these
materials in teaching is still granted, provided the authorship and
licensing information here is displayed.  i would appreciate being
notified if you use this book for teaching, just so that i know the
materials are being put to use, but this is not required.  information
displayed.  no warranties are given or implied for this material.

