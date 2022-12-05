
# fastStat: Fast Lane to Learning Statistics! 

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
learning R.

## <a name="sampling">Lesson 1:  The Notion of a Sample</a> 

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

## <a name="normal">Lesson 2:  The Role of Normal (Gaussian) and Other Parametric Distribution Families</a> 

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

## <a name="normal">Lesson 3:  Conceptual Populations</a> 

In the opinion poll example, it is clear as to which population is
sampled.  In many applications, the issue is more conceptual.  If for
instance we run a clinical trial with 100 diabetic patients, we might
think of them as having been sampled from the population of all
diabetics, even though we did not actually select the patients in our
sample, whether randomly or otherwise.

## <a name="stderr">Lesson 4:  Standard Errors</a> 



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

