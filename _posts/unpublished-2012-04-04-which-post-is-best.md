---
layout: post
title: "Which Post is Best?"
date: 2012-04-04
summary: Many sites allow users to rate content. The smart sites use a simple up/down vote. How does that work?
---

Many sites allow users to rate content.
Some use a scale, but the smart sites use a simple up/down vote.
But how does that work?
<p>

<p>

But how do we determine the order of content given only a set of (up, down) votes for each one?

<table cellpadding="0" cellspacing="0" border="0">
<tr>
<td width="50%" valign="top">

<h3>Percentage</h3>

<div style="float:right; margin-left:5px">
<img width="200" height="123" border="0" alt="" title="" src="data:image/jpeg;base64,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">
</div>

The brain-dead simplest way is by percentage:
<div style="font-size:smaller; white-space:nowrap">
<code>up / max(1, up+down)</code>
</div>
which actually works fine if your content all has about the same number of votes.
But in the real world it quickly becomes clear that it doesn't work
(assuming you aren't Amazon or UrbanDictionary).
The best possible score becomes (1 up, 0 down) which soundly trumps
(99 up, 1 down), which is stupid.

<td width="50%" valign="top">

<h3>Wilson Score Interval</h3>

<div style="float:right; margin-left:5px">
<img width="200" height="123" border="0" alt="" title="" src="data:image/jpeg;base64,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">
</div>

<a href="http://evanmiller.org/how-not-to-sort-by-average-rating.html">Evan Miller has written about</a>
how <a href="http://reddit.com/">reddit</a> solved this problem by using the
<a href="http://en.wikipedia.org/wiki/Binomial_proportion_confidence_interval#Wilson_score_interval">Wilson score interval</a>,
which scales more gradually and evenly with disparate, small numbers of votes.
This smooth scaling allows more churn at the top of a list of content, which exposes more, different content to
more people which results in a more accurate reflection in quality.

<tr>
<td valign="top">

<table class="border">
<tr>
  <th style="border:0" colspan="2">
  <th colspan="11">Downvotes
<tr>
  <th style="border:0" colspan="2">
  <th>0
  <th>1
  <th>2
  <th>3
  <th>4
  <th>5
  <th>6
  <th>7
  <th>8
  <th>9
  <th>10
<tr>
  <th rowspan="11">Up
  <th>0
  <td style="background-color:#2e64fe">0.00
  <td style="background-color:#2e64fe">0.00
  <td style="background-color:#2e64fe">0.00
  <td style="background-color:#2e64fe">0.00
  <td style="background-color:#2e64fe">0.00
  <td style="background-color:#2e64fe">0.00
  <td style="background-color:#2e64fe">0.00
  <td style="background-color:#2e64fe">0.00
  <td style="background-color:#2e64fe">0.00
  <td style="background-color:#2e64fe">0.00
  <td style="background-color:#2e64fe">0.00
<tr>
  <th>1
  <td style="background-color:#fe9a2e">1.00
  <td style="background-color:#ffcd97">0.50
  <td style="background-color:#7397fe">0.33
  <td style="background-color:#628afe">0.25
  <td style="background-color:#5783fe">0.20
  <td style="background-color:#507dfe">0.17
  <td style="background-color:#4b7afe">0.14
  <td style="background-color:#4877fe">0.12
  <td style="background-color:#4575fe">0.11
  <td style="background-color:#4273fe">0.10
  <td style="background-color:#4172fe">0.09
<tr>
  <th>2
  <td style="background-color:#fe9a2e">1.00
  <td style="background-color:#ffbc74">0.67
  <td style="background-color:#ffcd97">0.50
  <td style="background-color:#81a2fe">0.40
  <td style="background-color:#7397fe">0.33
  <td style="background-color:#6990fe">0.29
  <td style="background-color:#628afe">0.25
  <td style="background-color:#5c86fe">0.22
  <td style="background-color:#5783fe">0.20
  <td style="background-color:#5480fe">0.18
  <td style="background-color:#507dfe">0.17
<tr>
  <th>3
  <td style="background-color:#fe9a2e">1.00
  <td style="background-color:#ffb463">0.75
  <td style="background-color:#ffc382">0.60
  <td style="background-color:#ffcd97">0.50
  <td style="background-color:#87a6fe">0.43
  <td style="background-color:#7c9efe">0.38
  <td style="background-color:#7397fe">0.33
  <td style="background-color:#6c92fe">0.30
  <td style="background-color:#668efe">0.27
  <td style="background-color:#628afe">0.25
  <td style="background-color:#5e87fe">0.23
<tr>
  <th>4
  <td style="background-color:#fe9a2e">1.00
  <td style="background-color:#ffaf58">0.80
  <td style="background-color:#ffbc74">0.67
  <td style="background-color:#ffc688">0.57
  <td style="background-color:#ffcd97">0.50
  <td style="background-color:#8aa8fe">0.44
  <td style="background-color:#81a2fe">0.40
  <td style="background-color:#7a9cfe">0.36
  <td style="background-color:#7397fe">0.33
  <td style="background-color:#6e93fe">0.31
  <td style="background-color:#6990fe">0.29
<tr>
  <th>5
  <td style="background-color:#fe9a2e">1.00
  <td style="background-color:#ffab51">0.83
  <td style="background-color:#ffb76a">0.71
  <td style="background-color:#ffc07d">0.62
  <td style="background-color:#ffc78b">0.56
  <td style="background-color:#ffcd97">0.50
  <td style="background-color:#8daafe">0.45
  <td style="background-color:#85a4fe">0.42
  <td style="background-color:#7e9ffe">0.38
  <td style="background-color:#789bfe">0.36
  <td style="background-color:#7397fe">0.33
<tr>
  <th>6
  <td style="background-color:#fe9a2e">1.00
  <td style="background-color:#ffa94c">0.86
  <td style="background-color:#ffb463">0.75
  <td style="background-color:#ffbc74">0.67
  <td style="background-color:#ffc382">0.60
  <td style="background-color:#ffc88e">0.55
  <td style="background-color:#ffcd97">0.50
  <td style="background-color:#8eabfe">0.46
  <td style="background-color:#87a6fe">0.43
  <td style="background-color:#81a2fe">0.40
  <td style="background-color:#7c9efe">0.38
<tr>
  <th>7
  <td style="background-color:#fe9a2e">1.00
  <td style="background-color:#ffa749">0.88
  <td style="background-color:#ffb15d">0.78
  <td style="background-color:#ffb96d">0.70
  <td style="background-color:#ffbf7a">0.64
  <td style="background-color:#ffc586">0.58
  <td style="background-color:#ffc98f">0.54
  <td style="background-color:#ffcd97">0.50
  <td style="background-color:#8facfe">0.47
  <td style="background-color:#89a7fe">0.44
  <td style="background-color:#84a3fe">0.41
<tr>
  <th>8
  <td style="background-color:#fe9a2e">1.00
  <td style="background-color:#ffa646">0.89
  <td style="background-color:#ffaf58">0.80
  <td style="background-color:#ffb667">0.73
  <td style="background-color:#ffbc74">0.67
  <td style="background-color:#ffc17f">0.62
  <td style="background-color:#ffc688">0.57
  <td style="background-color:#ffca90">0.53
  <td style="background-color:#ffcd97">0.50
  <td style="background-color:#90acfe">0.47
  <td style="background-color:#8aa8fe">0.44
<tr>
  <th>9
  <td style="background-color:#fe9a2e">1.00
  <td style="background-color:#ffa543">0.90
  <td style="background-color:#ffad54">0.82
  <td style="background-color:#ffb463">0.75
  <td style="background-color:#ffba6f">0.69
  <td style="background-color:#ffbf79">0.64
  <td style="background-color:#ffc382">0.60
  <td style="background-color:#ffc78a">0.56
  <td style="background-color:#ffca91">0.53
  <td style="background-color:#ffcd97">0.50
  <td style="background-color:#91adfe">0.47
<tr>
  <th>10
  <td style="background-color:#fe9a2e">1.00
  <td style="background-color:#ffa441">0.91
  <td style="background-color:#ffab51">0.83
  <td style="background-color:#ffb25f">0.77
  <td style="background-color:#ffb76a">0.71
  <td style="background-color:#ffbc74">0.67
  <td style="background-color:#ffc07d">0.62
  <td style="background-color:#ffc485">0.59
  <td style="background-color:#ffc78b">0.56
  <td style="background-color:#ffca91">0.53
  <td style="background-color:#ffcd97">0.50
</table>

<p>

Next we'd attempt to solve percentage's obvious shortcoming to favor content with more
votes above those with fewer &mdash; but this is not as easy as it looks.
Simplistic implementations end up with
<a href="http://blog.reddit.com/2009/10/reddits-new-comment-sorting-system.html">a feedback
loop where a few early (possibly mediocre) pieces of content move to the top and dominate everything else that comes after</a>.
Since they're on top more people see them and the more votes they get so they're on top and more people see them... ad infinitum.
Any content posted after the first few, no matter how good it is, can never rise to the top.
This approach rewards timeliness over quality, and that's not what we want.

<td valign="top">

<table class="border">
<tr>
  <th style="border:0" colspan="2">
  <th colspan="11">Downvotes
<tr>
  <th style="border:0" colspan="2">
  <th>0
  <th>1
  <th>2
  <th>3
  <th>4
  <th>5
  <th>6
  <th>7
  <th>8
  <th>9
  <th>10
<tr>
  <th rowspan="11">Up
  <th>0
  <td style="background-color:#2e64fe">0.00
  <td style="background-color:#628afe">0.25
  <td style="background-color:#6a90fe">0.29
  <td style="background-color:#688ffe">0.28
  <td style="background-color:#658dfe">0.26
  <td style="background-color:#628afe">0.25
  <td style="background-color:#5f88fe">0.24
  <td style="background-color:#5d86fe">0.23
  <td style="background-color:#5a85fe">0.22
  <td style="background-color:#5983fe">0.21
  <td style="background-color:#5782fe">0.20
<tr>
  <th>1
  <td style="background-color:#ffc78b">0.56
  <td style="background-color:#ffcd97">0.50
  <td style="background-color:#90adfe">0.47
  <td style="background-color:#8ba9fe">0.45
  <td style="background-color:#86a5fe">0.42
  <td style="background-color:#82a2fe">0.40
  <td style="background-color:#7e9ffe">0.38
  <td style="background-color:#7a9cfe">0.37
  <td style="background-color:#779afe">0.35
  <td style="background-color:#7498fe">0.34
  <td style="background-color:#7296fe">0.33
<tr>
  <th>2
  <td style="background-color:#ffb668">0.73
  <td style="background-color:#ffbf79">0.64
  <td style="background-color:#ffc484">0.59
  <td style="background-color:#ffc88c">0.55
  <td style="background-color:#ffcb92">0.52
  <td style="background-color:#95b1fe">0.50
  <td style="background-color:#91adfe">0.47
  <td style="background-color:#8daafe">0.45
  <td style="background-color:#89a7fe">0.44
  <td style="background-color:#86a5fe">0.42
  <td style="background-color:#83a3fe">0.41
<tr>
  <th>3
  <td style="background-color:#ffaf58">0.80
  <td style="background-color:#ffb769">0.72
  <td style="background-color:#ffbc74">0.67
  <td style="background-color:#ffc07d">0.63
  <td style="background-color:#ffc483">0.59
  <td style="background-color:#ffc689">0.57
  <td style="background-color:#ffc98e">0.54
  <td style="background-color:#ffcb93">0.52
  <td style="background-color:#ffcd97">0.50
  <td style="background-color:#93aefe">0.48
  <td style="background-color:#8facfe">0.47
<tr>
  <th>4
  <td style="background-color:#ffaa4f">0.84
  <td style="background-color:#ffb25e">0.77
  <td style="background-color:#ffb769">0.72
  <td style="background-color:#ffbb72">0.68
  <td style="background-color:#ffbe79">0.65
  <td style="background-color:#ffc17f">0.62
  <td style="background-color:#ffc484">0.59
  <td style="background-color:#ffc688">0.57
  <td style="background-color:#ffc88d">0.55
  <td style="background-color:#ffca90">0.53
  <td style="background-color:#ffcb94">0.52
<tr>
  <th>5
  <td style="background-color:#ffa84a">0.87
  <td style="background-color:#ffae57">0.81
  <td style="background-color:#ffb361">0.76
  <td style="background-color:#ffb769">0.72
  <td style="background-color:#ffba70">0.69
  <td style="background-color:#ffbd76">0.66
  <td style="background-color:#ffc07b">0.63
  <td style="background-color:#ffc280">0.61
  <td style="background-color:#ffc484">0.59
  <td style="background-color:#ffc688">0.57
  <td style="background-color:#ffc78b">0.56
<tr>
  <th>6
  <td style="background-color:#ffa646">0.89
  <td style="background-color:#ffab51">0.83
  <td style="background-color:#ffb05b">0.79
  <td style="background-color:#ffb463">0.75
  <td style="background-color:#ffb769">0.72
  <td style="background-color:#ffba6f">0.69
  <td style="background-color:#ffbc75">0.67
  <td style="background-color:#ffbf79">0.64
  <td style="background-color:#ffc17d">0.62
  <td style="background-color:#ffc281">0.60
  <td style="background-color:#ffc485">0.59
<tr>
  <th>7
  <td style="background-color:#ffa443">0.90
  <td style="background-color:#ffa94d">0.85
  <td style="background-color:#ffae56">0.81
  <td style="background-color:#ffb15e">0.77
  <td style="background-color:#ffb464">0.74
  <td style="background-color:#ffb76a">0.72
  <td style="background-color:#ffba6f">0.69
  <td style="background-color:#ffbc73">0.67
  <td style="background-color:#ffbe77">0.65
  <td style="background-color:#ffc07b">0.63
  <td style="background-color:#ffc17f">0.62
<tr>
  <th>8
  <td style="background-color:#ffa340">0.91
  <td style="background-color:#ffa84a">0.87
  <td style="background-color:#ffac52">0.83
  <td style="background-color:#ffaf59">0.80
  <td style="background-color:#ffb25f">0.77
  <td style="background-color:#ffb565">0.74
  <td style="background-color:#ffb76a">0.72
  <td style="background-color:#ffb96e">0.69
  <td style="background-color:#ffbb72">0.68
  <td style="background-color:#ffbd76">0.66
  <td style="background-color:#ffbf7a">0.64
<tr>
  <th>9
  <td style="background-color:#ffa23f">0.92
  <td style="background-color:#ffa747">0.88
  <td style="background-color:#ffaa4f">0.84
  <td style="background-color:#ffad56">0.81
  <td style="background-color:#ffb05c">0.78
  <td style="background-color:#ffb361">0.76
  <td style="background-color:#ffb566">0.74
  <td style="background-color:#ffb76a">0.72
  <td style="background-color:#ffb96e">0.70
  <td style="background-color:#ffbb72">0.68
  <td style="background-color:#ffbd75">0.66
<tr>
  <th>10
  <td style="background-color:#ffa23d">0.93
  <td style="background-color:#ffa645">0.89
  <td style="background-color:#ffa94c">0.86
  <td style="background-color:#ffac53">0.83
  <td style="background-color:#ffaf58">0.80
  <td style="background-color:#ffb15d">0.78
  <td style="background-color:#ffb362">0.75
  <td style="background-color:#ffb566">0.73
  <td style="background-color:#ffb76a">0.71
  <td style="background-color:#ffb96e">0.70
  <td style="background-color:#ffbb71">0.68
</table>

<p>

</table>

<h3>Please Rate This Post</h3>
<form>
<table id="ratepost" style="border:1px solid #ccc">
<tr>
    <td><input type="radio">A+++++++ would read again
    <td><input type="radio">4/5
    <td><input type="radio">Very Good
    <td><input type="radio">OK
<tr>
    <td><input type="radio">Thumbs Up
    <td><input type="radio">3/5
    <td><input type="radio">Helpful
    <td><input type="radio">C++
<tr>
    <td><input type="radio">Excellent
    <td><input type="radio">Pretty Good
    <td><input type="radio">&radic;2/5
    <td><input type="radio">Meh
<tr>
    <td><input type="radio">Thumbs Down
    <td><input type="radio">D--
    <td><input type="radio">-1/5
    <td><input type="radio">Ugh
</table>
</form>
<h3>See Also</h3>
<ol>
    <li>Evan Miller, <a href="http://evanmiller.org/how-not-to-sort-by-average-rating.html">How Not To Sort By Average Rating</a>
    <ul>
        <li><span style="background-color:#99eeff">reddit/r/programming</span>: <a href="http://www.reddit.com/r/programming/comments/rt6q3/how_not_to_sort_by_average_rating/">How Not to Sort By Average Rating</a>
        <li><span style="background-color:#f60;color:#fff">Hacker News</span>: <a href="http://news.ycombinator.com/item?id=3792627">How Not To Sort By Average Rating</a>
    </ul>
    <li><a href="http://blog.reddit.com/2009/10/reddits-new-comment-sorting-system.html">Reddit's New Comment System</a>
    <li><a href="http://t16web.lanl.gov/Kawano/gnuplot/plotpm3d-e.html">gnuplot not-so-FAQ: How do I draw a colored 3D figure?</a>
</ol>
