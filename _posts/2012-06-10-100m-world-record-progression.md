---
layout: post
title: "100 Meter World Record Progression"
date: 2012-06-10
summary: With the Olympics coming up I wondered what a single race composed of every 100-meter sprint world record performance would look like.
---

With the Olympics coming up I wondered what a single race composed
of every 100-meter sprint world record performance would look like.

## The Result

<a href="/vis/100-meter-world-record-progression/100m-1600.png"><img style="border:1px solid #ccc; cursor:zoom-in" height="258" width="800" title="Click for fullsize image" alt="10.6 10.4 10.3 10.2 10.1 10.0 9.9 9.8 9.7 9.6" src="/vis/100-meter-world-record-progression/100m-800.png"></a>

<a rel="license" href="http://creativecommons.org/licenses/by/3.0/"><img alt="Creative Commons License" style="border-width:0" src="//i.creativecommons.org/l/by/3.0/80x15.png" /></a>

## Goals

Any good infographic allows the data to shape the graphics &mdash; not the other way around.
To make big trends obvious at a distance and make interesting details available upon closer inspection.
The trend here is
faster times &mdash; I wanted to see how many world-record performances there had been
and what the spacing would have looked like had they all been running in the same race.
I also wanted to honor the sport by accurately displaying the sprint finish:
<a href="http://www.terragalleria.com/north-america/canada/vancouver/picture.cabc10833.html">body position with the chest thrust forward</a>
and aligned with the times, and also the uncertainty inherent in the measurement's precision.
I also wanted to see what 1/10 and 1/100 of a second means in terms of scale at world-class speeds.


## Rationale

Given <a href="http://en.wikipedia.org/wiki/Men's_100_metres_world_record_progression">our data set</a>,
first off we have to decide which performances to include and which to omit.

### Included

* World record-tying performances by different athletes
* World record-tying performances of a single record by the same athlete (condensed to a single entry)


### Omitted

* Unofficial pre-IAAF records before 1912
* since <a href="http://en.wikipedia.org/wiki/Ben_Johnson_(sprinter)#Olympic_scandal">Ben Johnson's infamous drug scandal in the 1988 Olympics</a>
several records have been recognized briefly only to be rescinded due to cheating &mdash; those are also omitted.


### What works
I think the overlapping sprinters and the second labels tells the story of progression
and competition well at a distance &mdash; and shows the astounding progress of Usain Bolt's
from 9.69 to 9.58 in a way that a table of numbers never could to a layman.
If you look more closely you see the slow improvement over the decades at lower precision
(the list of names doing double-duty as a bar chart) and the fierce back-and-forth between
Carl Lewis and Leroy Burrell from 1988-94. Indeed, something
we realize from the data set that we <em>can't</em> see is that these sorts of duels
probably existed earlier between sprinters like Jim Hines/Charles Greene in the 1960s or
Bobby Morrow/Ira Murchison in the 1950s &mdash; sadly the precision was just too low for the
numbers to tell the whole story.

### What didn't work

#### Year
I wondered if the records were driven by the quadrennial Olympic cycle.
I tried to incorporate a second axis for year as
<a href="http://en.wikipedia.org/wiki/File:World_record_progression_100m_men.svg">the existing Wikipedia graphic does</a>,
but no matter what it greatly decreases information density, elucidates little and generally looks terrible.
There are interesting trends in there, periods of inactivity (1941-48 for example) punctuated by great
flurries of activity (the mid-{30s,50s,70s} and the last 2000s) &mdash; I just wasn't able to incorporate
it into this image.

#### Country
It's immediately obvious from the data set that the United States dominated the 100m in the 20th century,
with a smattering of other countries (some of which no longer formally exist) in the mix.
But so far the 21st century belongs to the Jamaicans.
Attempts at including country in text or graphic form did not pan out.

## Precision
IAAF-mandated precision from 1912-1976 was 1/10 second. After decades of many, many ties
precision was increased in 1977 to 1/100 second.
This causes headaches where sets overlap &mdash; seemingly identical times measured
at different precisions are not necessarily equal.
Jim Hines' 9.9 is different and not perfectly comparable to Leroy Burrell's 9.90.
Hines may have run a 9.89 or a 9.91 &mdash; we'll never know for certain.
In order to display them properly we need to indicate precision along with the times.
This is difficult to do given the labelling scheme that is required.
With smaller or fewer labels it may have been possible to align the 1/10 and 1/100
more closely, but because each point needs 1+ names it just didn't fit.
<p>
We then realize that what we really have is a collection of sets for each athelete,
most with only a single data point. Therefore, instead of encoding data from a small
number of sets each with a distinct icon or color we must label each, which eats up space.
This is compounded by the fact that the range of the data is moderately large in
proportion to its precision &mdash; 10.6 to 9.6 at 1/100 requires a scale with 1000 distinct
points on the x axis meaning that similar performances such as the {9.86,9.85,9.84} from
1991-96 are squished very close to each other no matter what we do.

### More Questions

#### Should some or all rescinded records be shown?

On the one hand one can stick to the IAAF official records or take the moral
high ground and argue that cheaters don't deserve to be recognized, and that's fine, I guess.
But the raw data is so much more interesting.
After reading about
<a href="http://en.wikipedia.org/wiki/Ben_Johnson_(sprinter)#Olympic_scandal">Ben Johnson's overturned
1988 Olympics gold medal and world record</a>
it appears that
<a href="http://en.wikipedia.org/wiki/Carl_Lewis#Stimulant_use">many athletes competing
in that event were indeed taking banned substances but cleared anyway</a>
&mdash; furthermore Johnson was punished more for his honesty (by having his 1987 record rescinded)
than he would have been if he'd kept his mouth shut. The counter-argument would be that I'm not
qualified to choose which records should count and which shouldn't and it should be left to the
professionals... but on the other hand the raw data is still more interesting.
I still haven't made up my mind.

## References

# Wikipedia, [Men's 100 metres world record progression](http://en.wikipedia.org/wiki/Men's_100_metres_world_record_progression) Fetched Jun 1, 2012
# IAAF.org, [Statistics, 100 meters Outdoors](http://www.iaaf.org/statistics/records/inout=o/discType=5/disc=100/detail.html)
# IAAF.org, [100m for the expert](http://www.iaaf.org/community/athletics/trackfield/newsid=4666.html)
# BBC, [Gatlin denied outright 100m mark](http://newsvote.bbc.co.uk/mpapps/pagetools/print/news.bbc.co.uk/sport2/hi/athletics/4989558.stm)

