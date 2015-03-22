---
layout: post
date: 2015-02-07 08:58
title: USACO training section 1.1
language: "en"
categories: [practice, USACO training]
link:
---

###Foreword

Articles belonging to the *practice* category will be my experiences when doing practice on some oj websites, such as [leetcode](http://oj.leetcode.com), [hihoCoder](http://hihocoder.com), [usaco training](http://train.usaco.org). Specially, articles belonging to the *USACO training* are solutions for problems on [usaco](http://train.usaco.org).

###Section 1.1

Section 1.1 is an introduction for new comers to learn how to submit solutions and what will be included in the whole training process. This section contains the following 4 ad hoc problems for practicing:

* [Your ride is here](http://train.usaco.org/usacoprob2?a=IgNNK912MbQ&S=ride)
* [Greedy gift givers](http://train.usaco.org/usacoprob2?a=IgNNK912MbQ&S=gift1)
* [Friday the thirteenth](http://train.usaco.org/usacoprob2?a=IgNNK912MbQ&S=friday)
* [Broken necklace](http://train.usaco.org/usacoprob2?a=IgNNK912MbQ&S=beads)

<!-- more -->

#### 1.*Your ride is here*
"This is probably the easiest problem in the entire set of lessons." To solve this problem,   just write a **hash** function, which maps a string to an integer in the following way: get the product of all letters in the string('A' is 1 and 'Z' is 26), then return the product mod 47.

#### 2.*Greedy gift givers*
"The hardest part about this problem is dealing with the strings representing people's names." There are two ways to represent people's names. The first way(used in the analysis) is to construct a Person structure to keep the persone's name and its money got or lost, and then write a *lookup* function that given a person's name, returns its Person Structure. The other way is just use hashmap to keep people's names and money. The key of the hashmap is people's names and the
value is just the money.


Both ways are OK. The first way's lookup function is in O(n) complexity, while the second is in O(1) complexity. However, the Person structure is easy to extend when you need to keep more information about each person.

#### 3.*Friday the thirteenth*
"Brute force is a wonderful thing." Just go over days that has passed by since Jan 13th, 1900. Be careful about the leap year. The implementation in the analysis is beautiful. Put the heart part here.
{% highlight c %}
dow = 0;    /* day of week: January 13, 1900 was a Saturday = 0 */
for(y=1900; y<1900+n; y++) {
    for(m=0; m<12; m++) {
        ndow[dow]++; /* ndow is an array keep counts of each day of week */
        dow = (dow+mlen(y, m)) % 7;
    }
}
{% endhighlight %}

And *mlen* is implemented like this:
{% highlight c %}
int mtab[] = { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
/* return length of month m in year y */
int
mlen(int y, int m)
{
    if(m == 1)    /* february */
        return mtab[m]+isleap(y);
    else
        return mtab[m];
}
{% endhighlight %}

#### 4.*Broken necklace*
The most straight forward way is to try breaking the necklace at each point and see how many beads can be collected. There is one hightlight in the analysis code.
{% highlight c %}
/*
    * Return n mod m.  The C % operator is not enough because
    * its behavior is undefined on negative numbers.
    */
int
mod(int n, int m)
{
    while(n < 0)
    n += m;
    return n%m;
}
{% endhighlight %}
Notice that you might count the same beads twice if they can be taken off either side off the break. This will happen only when all beads can be taken off the necklace. Check for that at the end(if the length of beads collected is more than the length of the necklace).

Let's discuss the solution more in detail. Suppose that we have got the number of beads collected breaking at the ith bead, now we will move on to breaking the (i+1)th bead. It is easy to get a transformation equation. So we can solve this problem using a dynamic programming algorithm. See more datail in the [analysis](http://train.usaco.org/usacoanal2?a=IgNNK912MbQ&S=beads).
