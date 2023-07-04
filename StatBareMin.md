
# Statistics--the Bare Minimum (or even less)

This document presents a very brief overview of the main ideas in
practical statistics.  The only background assumed is high school
algebra, e.g. familiarity with the slope of a line.  We also assume that
the reader has been exposed (superficially) to the notions of mean,
standard deviation and histograms. 

Occasionally there are references to data frames etc. for readers who
use R or Python for data science, but these references are not essential
to the narrative.  Near the end of the document, examples of the
concepts that use actual R statistical operations are shown, but this is
not intended as a tutorial on R, and can be understood with knowing the
R details.  (See my [fasteR tutorial](https://github.com/matloff/fasteR)
for a quick introduction to R.)

In addition there are also references to my [fastStat
tutorial](https://github.com/matloff/fastStat), which is more detailed
and presumes more background (a calculus-based probability course) on
the part of the reader.  These may be safely ignored by readers without
this background.

## Coverage

Main topics:

* central importance of sampling variation

* standard errors; statistical inference

* prediction, especially with linear models

* effect estimation, confounding 

Notably missing:

* normal, chi-square, F distributions 

* inordinate emphasis of hypothesis testing and p-values

##  Data notation  

Say we have data on height, age and weight for 100 people.  Then the
fact that we have 3 variables is denoted p = 3.  The number of people,
100 here, is denoted n = 100.

Of course, data points need not be people.  For instance, we may have
data on a sample of n = 25 elementary schools in a US state, with the
data on any school being the number of students and number of teachers.
We'd have p = 2 here.

By the way, the proper statistical phrasing is that, say in the school
example, is "We have a sample of size 25," not "We have 25 samples."

We'll use X<sub>1</sub> to denote the first data point in our sample.
In the people example above, X<sub>1</sub> denotes the height, age and
weight of the first person in our sample.  X<sub>2</sub> is our second
data point, say the numbers of students and teachers for the second
school in the school example.  X<sub>3</sub>, X<sub>4</sub> and so on
are defined similarly.

Storage in a data frame would have n rows and p columns.

##  Sampling from populations, conceptual or real 

Our 100 people is considered a sample from some population, maybe the
population of all UC Davis students.  Our 25 elementary schools may have
been chosen randomly among all such schools in the state.

Since the data is considered to have been sampled randomly, the
X<sub>i</sub> are random.  This randomness is at the heart of
statistical inference, as you will see below.

In some cases, the sampled "population" may be more conceptual than
real.  Say we collect data on *all* UCD students.  They might be
considered to be a "sample" from the set of all such students, past,
present and future.  There may be issues here, e.g. change in student
characteristics over time, so some caution must be exercized in applying
statistical methods in such contexts.

##  Standard errors 

This section is longer but involves the very essence of statistical
inference. Some extra effort on the reader's part here will pay large
dividends.

Around election times, one often here on TV newscasts the results of
opinion polls, statements along the lines of "Candidate X is supported
by 54.5% of the voters, according to a survey released today.  The margin
of error is 3.2%."  We'll discuss the exact meaning of the margin of
error later, but for now our focus is on why we need something like
a margin of error, or more broadly, a *standard error*.

Suppose the poll data had n = 1200, i.e. 1200 people were surveyed, say
via random selection from a list of phone numbers.  Let's use Q to
denote the proportion of people in our poll who say they will vote for
Candidate X, so Q = 0.545 above.

If, hypothetically, the poll were to be conducted a second time, we
would have a different set of 1200 people, and this time there might be,
say, 51.7% of them saying they will vote for Candidate X, i.e. Q =
0.517.  In other words, there is *sampling variation* in Q.

Similarly, say we are interested in the mean weight of all UCD students,
based on a sample of 100.  Our estimate of that population mean will be
the average weight in our sample, denoted X<sub>bar</sub>.  Well,
X<sub>bar</sub> will be subject to sampling variation too; each
different set of 100 students will have a different value of
X<sub>bar</sub>.

In general, let T be our estimate of some population quantity
&theta;.  E.g. in the weight example above, T would be X<sub>bar</sub>,
and &theta; would be the mean weight of students over the entire
population.  Is T probably pretty accurate, i.e. probably pretty close
to the true (though unknown) value of &theta;?  We reason as follows.

We just have our one sample, and hope that it yields a T
value near &theta;, but we have to wonder:  Is this sample
representative?  Might other samples have values of T that are closer to
the true (but unknown) value of &theta;?

*Key point:* If we know that T doesn't vary much from one sample to
another, that concern is addressed!  If the sampling variability of T is
small, then for most samples T will be near &theta;, so our value of T
is likely near &theta;.  No guarantees of course.

So, what measure do we use to describe the variability of T?  We use the
same measure that is used to describe the variability of any
quantity--its standard deviation.  And that's what the standard error of
T is--its standard deviation over all possible samples.  Let's denote it
as se(T).

But wait a minute...that standard deviation is a population quantity, so
we don't know it anymore than we know the true value of &theta;.  Have
we hit a dead end here?  It turns out that the answer is no.  We can
estimate that standard deviation too.

There is also a related question:  What if most of the values of T are near
each other, BUT not centered near &theta;?  This is a question of
(near-) *unbiasedness* of T.  It's beyond the scope of "bare minimum or
less" tutorial, but see Lesson STDERRS of the *fastStat* tutorial.

##  Confidence intervals 

Recall the political poll example, and the notion of margin of error.
Recall that we used Q to denote the proportion of Candidate X supporters
in our sample?  How does the margin of error relate to standard error?
Actually, the relation is simple:

margin of error = 1.96 x se(Q)

So to get a range for q, the true population proportion of voters
supporting X, we compute the interval

Q &pm; 1.96 x se(Q)

This is then an approximate 95% *confidence interval* for q, the true
population proportion of voters favoring Candidate X.

What does it mean to say we are "95% confident" than q is in that
interval?  It simply means that 95% of all possible samples will yield
intervals that contain the true q.

Of course, we don't know the situation for our particular sample.
Instead, the above probability interpretation is analogous to, say,
dice.  If we roll 2 dice, the probability that their sum is at least 11
is 8/9.  So if we roll 2 dice 900 times, about 800 of the roles will
come out 11 or higher, so we say we "probably" will get a sum of at
least 11.

Lessons CI and CIAPPROX in the *fastStat* tutorial go into the details.

## Prediction

These days machine learning is really in vogue.  The basic goal is to
preduct one variable, i.e. guess its value, from others.  We might want
to predict whether a loan applicant will pay off the loan, based on job
status, previous credit history, etc.  Or  we may wish to predict
whether someone has a certain disease, based on physical measurements,
family history and so on.

Here is the basic approach:

> To predict some variable Y from X = t, the optimal rule is to guess Y to
> be the mean Y among all population entities have X = t.  

E.g. to predict someone's weight from their height and age, say height
70 and age 28:  Our guess is the mean weight of all people of height 70
and age 28.  Note that this is a *population* quantity.

The issue is then:

> How can we find subgroup means of this sort?

This is usually not so easy as it sounds.  If, say, our sample
(*training data* in machine learning parlance) consists of only n = 100
people, we may not have any people of height 70 and age 28, so we cannot
find the desired mean directly from our data.  Or, we might have
just, say, 2 or 3 such people, hardly enough to get a reliable estimate
of the mean.

What can be done?

* We could look at people in our sample whose height and weight are
  *near* 70 and 28, and find their average weight.  That would be our
estimate of the mean weight of all people in the population of height 70
and age 28.  This is what many machine learning algorithms boil down to,
especially the k-Nearest Neighbors and Random Forests methods.

* We could assume that, viewed as a function of the two variables height
  and age, the quantity mean(weight for a given height and age) follows
a certain mathematical formula, typically the *linear model*. 

In our "bare minimum, even less" context here, we will focus on linear
models.  See the *fastStat* tutorial for machine learning methods,
starting with Lesson PREDICT.

**Important note:**  Linear and other prediction models can be used for
more than prediction!  A very common use is effect measurement.  In a
study of gender pay gap, we may posit that several variables affect
wages, such as education, age, type of job and so on.  We might use a
linear model to gauge the effect of gender, in the presence of the other
variables.  We'll do more on this later in the tutorial.

## Linear models 

We assume a linear model, e.g. for predicting weight from height and
age:

mean weight = &beta;<sub>0</sub> + &beta;<sub>1</sub> height + &beta;<sub>2</sub> age

The &beta;<sub>i</sub> are population values, which we estimate from the
data.  Call the estimates b<sub>i</sub>.  Note that the b<sub>i</sub>
are random, since they are calculated from our sample data, and thus
have standard errors.  For instance, b<sub>2</sub>, the estimated age
coefficient, will vary from one sample to another; its standard error
helps us assess whether our b<sub>2</sub> is likely reasonably close to
&beta;<sub>2</sub>.

Here is an example in R, using a dataset **mlb** on Major League Baseball
players.

``` r
> z <- lm(Weight ~ Height+Age,data=mlb)  # mlb dataset, MLB players
> summary(z)

coefficients:
             estimate std. error t value pr(>|t|)    
(intercept) -187.6382    17.9447  -10.46  < 2e-16 ***
height         4.9236     0.2344   21.00  < 2e-16 ***
age            0.9115     0.1257    7.25 8.25e-13 ***
```

(Ignore all but the first column for now.)

Now, what about our earlier question of what value we should use to
predict the weight of a person who is 70 inches tall and is 28 years
old?  Remember, our model is

mean weight = &beta;<sub>0</sub> + &beta;<sub>1</sub> height + &beta;<sub>2</sub> age

so our prediction for the weight of such a person is

&beta;<sub>0</sub> + &beta;<sub>1</sub> 70 + &beta;<sub>2</sub> 28

The &beta;<sub>i</sub> are known, so we replace them by our estimates:

b<sub>0</sub> + b<sub>1</sub> 70 + b<sub>2</sub> 28 =
-187.64 + 4.92 x 70 + 0.91 x 28

and that will be our predicted value.  Actually, R does that for us:

``` r
> predict(z,data.frame(Height=70,Age=28))
       1 
182.5367 
```

We predict a weight of about 182 pounds.

See more in Lesson LIN of the *fastStat* tutorial.

##  Estimation of effects

According to our above model, players of height 70 and age 28 have mean weight

&beta;<sub>0</sub> + &beta;<sub>1</sub> 70 + &beta;<sub>2</sub> 28

For players of that height but age 29, the figure is 

&beta;<sub>0</sub> + &beta;<sub>1</sub> 70 + &beta;<sub>2</sub> 29

Subtracting the age 28 expression from the age 29 one, we have

mean weight (height 70, age 29) - mean weight (height 70, age 28) =
&beta;<sub>2</sub>

The other terms canceled out.  In fact, they would have canceled out if
we had compared 32-year-olds to those of age 33, etc.  And furthermore,
we see that this also would have been true for players of height 73.

In other words:

> The average effect on weight of one extra year of age is &beta;<sub>2</sub>.

Our estimate of the unknown &beta;<sub>2</sub> is b<sub>2</sub>, which
we found above to be 0.9115.  So, mean weight among the players
increases almost a pound per year of age, perhaps surprising for such a
physically fit group.  

But wait--might this be a sampling artifact?   Let's look at the
standard error of this figure, shown in the above output to be 0.1257.
This gives an approximate 95% confidence interval for the true
population &beta;<sub>2</sub>:

0.9915 &pm; 1.96 x 0.1257 = (0.6651,1.1579)

So it does seem that there is a weight gain here, at least about 2/3 of
a pound per year.

##  Indicator variables 

Often a variable of interest is dichotomous.  For instance, in
predicting human weight from height and age, we might also have gender
data.  These are typically coded 1 and 0, say 1 for male, 0 for female.
These variables are known as *indicator variables*.  Our model of mean
weight would then be

mean weight = &beta;<sub>0</sub> + &beta;<sub>1</sub> height + 
&beta;<sub>2</sub> age + &beta;<sub>3</sub> I<sub>male</sub>

where I<sub>male</sub> is equal to 1 for a man, 0 for a woman.

More in Lesson INDICATORS in the *fastStat* tutorial.

## *Ceteris paribus* comparisons

We saw above that linear models can be used not only for
prediction but also for effect estimation.  In fact, they may be used
for *fairer, more meaningful* estimation, as will be shown in this section.

Let's first consider the famous UC Berkeley data on admissions to grad
school.  It was alleged that Berkeley was biased against female
applicants, which seemed odd, so statistics professor Peter Bickel
took a fresh look at the data.

It turned out to be true the over proportion

(male applicants admitted) / (male applicants)

was greater than the corresponding fraction for females,

(female applicants admitted) / (female applicants)

But it turned out that the overall lower rate for women was largely due
to their applying to the more selective departments.  Comparing
admissions rates in individual departments, the seeming anomaly
disappeared; male and female rates were similar in most departments, and
in some the female rare was considerably higher.

The lesson to be learned is:

> Relations between two variables, in this case Admission and Gender, can
> be highly impacted by a third variable, Department here.

The Latin term *ceteris paribus*, "all else being equal," applies here.
In comparing male and female admissions rates, we must make sure that
all other variables are equal, e.g. compare the male admissions rate *in
Department A* to the female rate *in Department A.*

Linear models are often used for ceteris paribus analyses.  Let's look
at an analysis of a possible gender pay gap.  The **pef** dataset gives
census data in Silicon Valley, back in the year 2000.  Six different
tech job titles were present.

Here is what the first six rows of the data look like:

``` r
> head(pef)
       age     educ occ sex wageinc wkswrkd
1 50.30082 zzzOther 102   2   75000      52
2 41.10139 zzzOther 101   1   12300      20
3 24.67374 zzzOther 102   2   15400      52
4 50.19951 zzzOther 100   1       0      52
5 51.18112 zzzOther 100   2     160       1
6 57.70413 zzzOther 100   1       0       0
```

A linear model was used, with mean income modeled in terms of
age, education, job title, gender and number of weeks worked.  The
occupation column was converted to indicator variables.  Here are the
results:

``` r
> coef(lm(wageinc ~ .,data=pef))
 (Intercept)          age       educ16 educzzzOther       occ101
occ102 
 -12669.3225     469.8373    7597.2521  -14167.3888    1838.5510
13688.3012 
      occ106       occ140       occ141         sex2      wkswrkd 
   1380.0254   11732.5837   10791.0158   -8595.5831    1323.1999 
```


R created a new indicator variable here, **sex2**, which indicated
female; its value is 1 for women, 0 for men.
Here the gender variable is coded 1 for men, 2 for women. 

In the same manner as where the age effect in the baseball player
example could be assessed by the coefficient of the Age variable, the
coefficient for **sex2**, -8595.5831, is our estimate of the gender pay
gap:  Comparing men and women of the *same age*, *same education* and so
on, we found that women were paid about $8600 less than comparable men.

## Significance testing and p-values

This is one of the tragedies of statistics:  An old method has long been
shown to be at worst misleading and at best noninformative, yet was so
entrenched that it continues to be very widely used today.  Here we give
a brief description of the method, *significance* testing, and explain
its pitfalls.

The method first sets a *null hypothesis* H<sub>0</sub>.  In the gender
pay gap example, we might have

H<sub>0</sub>:  &beta;<sub>sex2</sub> = 0

Here H<sub>0</sub> hypothesizes that there is no gender pay gap.

The decision is made on the basis of "innocent until proven guilty":  We
believe H<sub>0p</sub> is true until and unless we find strong evidence
to the contrary.  Well, then, what constitutes "strong evidence"?

The answer is that we look at the quotient

Z = b<sub>sex2</sub> / (standard error of b<sub>sex2</sub>

*If H<sub>0</sub> were true*, Z would be between -1.96 and 1.96 with
probability 0.95.  (Yes, these numbers do also appear in our section on
confidence intervals, but the usage and interpretation will be different
here.)  Again, *if H<sub>0</sub> were true*, that quotient would have
only a 5% chance of being larger than 1.96 or less than -1.96.  So, if
the quotient does fall into that outside range, we say, "This would be
unlikely under H<sub>0</sub>, so we will abandon our belief in
H<sub>0</sub>."

SAT ex; don't just see if CI contains 0; p-value "greed"
