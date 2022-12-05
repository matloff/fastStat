
# fasteR: Fast Lane to Learning R! 

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

## <a name="sampling"> </a> Lesson 1:  The Notion of a Sample

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

