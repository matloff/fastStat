
# Statistics--the Bare Minimum (or even less)

This document presents a very brief overview of the main ideas in
practical statistics.  The only background assumed is high school
algebra, e.g. familiarity with the slope of a line.  We also assume that
the reader has been exposed (superficially) to the notions of mean,
standard deviation and histograms. 

Occasionally there are references to data frames etc. for readers who
use R or Python for data science, but these references are not essential
to the narrative.

In addition there are also references to my [fastStat
tutorial](https://github.com/matloff/fastStat), which is more detailed
and presumes more background (a calculus-based probability course) on
the part of the reader.  These may be safely ignored by readers without
this background.

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

#  Sampling from populations, conceptual or real 

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


#  Standard errors 

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
population.  Even though we take only one sample

We hope that T doesn't vary much from one sa

(Lesson STDERRS):  Consider for instance Xbar, the
average of the X_i.  Since the latter are random, then Xbar is too.
Thus it has a standard deviation, which we call the "standard error."
We hope it is small, as that means it doesn't vary much from one sample
to another, and thus should be a pretty accurate estimate of the
corresponing population value, say the population mean.

4.  Confidence intervals (Lessons CI and CIAPPROX):  Instead of just
reporting the mean weight of our sampled 100 UCD students (a "point
estimate"), we'd like an "interval estmate," e.g. "We are 95% confident
that the true mean weight of all UCD students is in the interval (a,b)."
Generally we take the interval to be, e.g. in the Xbar example, Xbar +-
1.96 stderr(Xbar).  Standard errors are reported in R output.

4.  Prediction (Lesson PREDICT):  To predict some variable Y 
from X = t, the optimal rule is to guess Y to be the mean Y 
among all population entities have X = t.  E.g. predict weight from
height and age, say height 70 and age 28.  Our guess is the mean weight
of all people of height 70 and age 28.

5.  Linear models (Lesson LIN):  We assume a lienar model, e.g. 

mean weight = beta_0 + beta_1 height + beta_2 age

The beta_i are population values, which we estimate from the data.  Call
the estimates b_i.  Note that the b_i are random (the beta_i are fixed
but unknown), and thus have standard errors.

Example:

> z <- lm(weight ~ height+age,data=mlb)  # mlb dataset, MLB players
> summary(z)
...
coefficients:
             estimate std. error t value pr(>|t|)    
(intercept) -187.6382    17.9447  -10.46  < 2e-16 ***
height         4.9236     0.2344   21.00  < 2e-16 ***
age            0.9115     0.1257    7.25 8.25e-13 ***

(Ignore the last two columns for now.)

6.  Machine learning predictive methods, e.g. k-nearest neighbors
(Lesson KNN):

One of the simplest ML methods is k-Nearest Neighbors. Say we are
predicting weight from height and age, and must do so for a new case in
which X = (70,28). Then we find the k rows in our training data that are
closest to (70,28), find the average weight among those data points, and
use that average as our predicted value for the new case.

7.  Indicator variables (Lesson INDICATORS):  Often has only the values only 1
and 0, indicating the presence or absence of some trait.  E.g. for UCD
students, we have have a CSmajor column, filled with 1s and 0s, for CS
and non-CS students.  Major point:  The mean of an indicator variable is
the proportion of 1s; on the population level, that becomes the
probability of a 1..

8.  "Ceteris paribus" estimation of effects:  E.g. the famous UCB
admissions data, admissions to grad school.  Say Y = 1 or 0, according
to admitted or not.  Turns out that P(Y = 1) for men is larger than that
for women, suggesting UCB is biased against women.  HOWEVER, now let X =
department applied to (6 of them).  Now, we find that admission
probability, i.e. P(Y = 1 | X = d) is HIGHER for women than men or about
the same, depending on which department d one considers.  The problem
was that women had been applying to the more selective departments.

9.  Linear models are often used for "ceteris paribus" analysis of, e.g.
gender pay gaps.

> head(pef)
       age     educ occ sex wageinc wkswrkd
1 50.30082 zzzOther 102   2   75000      52
2 41.10139 zzzOther 101   1   12300      20
3 24.67374 zzzOther 102   2   15400      52
4 50.19951 zzzOther 100   1       0      52
5 51.18112 zzzOther 100   2     160       1
6 57.70413 zzzOther 100   1       0       0
> coef(lm(wageinc ~ .,data=pef))
 (Intercept)          age       educ16 educzzzOther       occ101
occ102 
 -12669.3225     469.8373    7597.2521  -14167.3888    1838.5510
13688.3012 
      occ106       occ140       occ141         sex2      wkswrkd 
   1380.0254   11732.5837   10791.0158   -8595.5831    1323.1999 

Here the gender variable is coded 1 for men, 2 for women. So the
negative coefficient here suggests that, for equal age, education and so
on, women are paid about $8600 less than men.


