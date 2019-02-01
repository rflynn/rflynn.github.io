---
layout: post
title: "Optimizing Pong"
date: 2012-03-11
summary: A silly exercise in optimizing Pong visually demonstrates serious aspects of programming, optimization and concurrency.
---

<p>
A silly exercise in optimizing Pong visually demonstrates
aspects of programming, optimization and concurrency.
</p>


<style type="text/css">
th { background-color: #eef; font-weight:bold }
th, td { padding: 0.25em }
.score, .msg { background-color: transparent }
table.cols { width:90%; margin:auto; }
td.colright { width:240px; text-align:center }
</style>
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.7.0/jquery.js"></script>
<script src="https://raw.githubusercontent.com/rflynn/jquery-pong/master/jquery.pong.js"></script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script>!window.MathJax && document.write('<script type="text/javascript" src="//d3eoax9i5htok0.cloudfront.net/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"><\/script>');</script>


<h3>Challenge</h3>

Given <a href="http://en.wikipedia.org/wiki/Pong">Pong</a>...

<div style="width:360px; margin:5px; padding:5px; background-color:#eef; border:1px solid #99c">
<div id="pong_regular" style="margin: 0 auto">&nbsp;</div>
<span style="font:12px/11px Courier New, sans-serif">
</span>
</div>

<p>

...how do we run 100 games as quickly as possible without changing the game?

<a name="rules"></a>
<h3>Rules</h3>

<ul>
    <li>Complete 100 games in the least amount of time.
    <li>All of the basic components and rules must remain and the statistics
        must resemble the original, implying:
    <ul>
        <li>First player to 11 wins.
        <li>The paddles must have a chance at returning a given ball.
        <li>The average <em>points per game</em> must resemble the original.
    </ul>
    <li>The maximum speed of any component is 10 (specific to this simulator, we'll see this later).
</ul>

Any change within these bounds is fair game &mdash; the goal after all is to stretch our minds
to fully understand the factors involved in this simplest of games.

<h3>Initial Impression</h3>

Looks pretty slow, there should be lots of low-hanging fruit.
A cursory glance suggests that the speed of the ball is the limiting factor.

<h3>Profile</h3>

We start out by
<a href="http://en.wikipedia.org/wiki/Profiling_(computer_programming)">profiling</a>
obvious factors that relate to the <a href="#rules">Rules</a>, beginning with the mean game
time so we have quantitative feedback what effect our changes have.
Since it was explicitly mentioned that we must preserve
statistical outcomes we'll also profile statistics such as mean total score
and mean paddle touches per point to ensure that our quantitative
optimizations don't break the game.

<p>

<div style="width:360px; margin:5px; padding:5px; background-color:#eef; border:1px solid #99c">
<div id="pong_profile" style="margin: 0 auto">&nbsp;</div>
<span style="font:12px/11px Courier New, sans-serif">
ball:{speed:5}
<br><b>profile:true</b>
</span>
</div>

<p>

So now that we've got profiling we've got a baseline to compare our
optimizations to.

<h3>Obvious Stuff First</h3>

This should be a quick fix, so let's just dive in.
The obvious optimization is to make the ball faster.

<p style="clear:both">

<div style="width:360px; margin:5px; padding:5px; background-color:#eef; border:1px solid #99c">
<div id="pong_fastball" style="margin: 0 auto">&nbsp;</div>
<span style="font:12px/11px Courier New, sans-serif">
ball:{<b>speed:10</b>}
<br>profile:true
</span>
</div>

<p>

Ok, so it took longer than we thought but in the end we've optimized the low-level
timing and ball code and made the ball move twice as fast!
We expect this to double the speed of the game.
In fact, it <em>more</em> than doubles the speed because the paddles
are now too slow and have difficulty returning the ball at all.
No problem, we'll just speed up the paddles.

<p style="clear:both">

<div style="width:360px; margin: 5px; padding:5px; background-color:#eef; border:1px solid #99c">
<div id="pong_fast_paddlestoofast" style="margin: 0 auto">&nbsp;</div>
<span style="font:12px/11px Courier New, sans-serif">
ball:{speed:10}<br>
<b>paddle:{speed:10}</b>
<br>profile:true
</span>
</div>

<p>

The original paddle speed was 3, so our best guy stayed up late over the weekend
to tweak the paddle and timing code to eek out the maximum possible paddle speed.
They should be plenty fast.

<p>

They worked extremely well... too well. The first point never ended.
The unexpected 0 games completed also happened to caused a division-by-zero in
our reporting, but that's beside the point.
It looks like those paddle optimizations went a little overboard, since
we can't speed up the ball any more we'll have to slow the paddles back down...

<p>

<div style="width:360px; margin: 5px; padding:5px; background-color:#eef; border:1px solid #99c">
<div id="pong_fast" style="margin: 0 auto">&nbsp;</div>
<span style="font:12px/11px Courier New, sans-serif">
ball:{speed:10}<br>
<b>paddle:{speed:5}</b>
<br>profile:true
</span>
</div>

<p>

So we just threw a <code>sleep();</code> in there to slow the paddles down to 5
so they have a reasonable chance of returning a ball served at speed:10,
which results in a more balanced outcome.
Our paddle speed-up had thrown off the delicate timing balance that
we didn't fully appreciate was originally there when we started.

<p>

However, this new balance comes at the cost of time.
The longer points result in more time per game and slower overall performance.

<p>

<b>In fact, we notice that for all our low-level uber speedhacks we're more
or less back to where we started(!). Oops.</b>

<p>

Since the ball moves at the maximum possible speed we now have an
effectively optimal single thread of execution &mdash; right?
Are our opportunities for improvement exhausted?

<p>

Since we know we want to play 100 games, we could run multiple games at once.
Because our games don't depend on each other whatsoever they are
<a href="http://en.wikipedia.org/wiki/Embarrassingly_parallel">embarassingly parallel</a>;
little effort is required to run them in parallel.
We can run at least as many games at once as we have CPUs, maybe multiple per CPU.

<div style="width:360px; margin: 5px; padding:5px; background-color:#eef; border:1px solid #99c">
<div id="pong_sync_1" style="border:1px solid #ccc; margin:0 auto">&nbsp;</div>
<div id="pong_sync_2" style="border:1px solid #ccc; margin:0 auto">&nbsp;</div>
<div id="pong_sync_3" style="border:1px solid #ccc; margin:0 auto">&nbsp;</div>
<span style="font:12px/11px Courier New, sans-serif">
ball:{speed:10}
<br>paddle:{speed:5}
<br>profile:false
</span>
</div>

<p>

By using N CPUs we get an N-wise speedup which is great, but because our game
generates very little actual I/O (a few stats must be saved at the end of each game)
our app is
<a href="http://en.wikipedia.org/wiki/CPU_bound">bound by the speed of our memory
bus, CPU cache and clockspeed</a>
so we don't benefit from running multiple games per CPU.

<p>

<a href="http://www.gotw.ca/publications/concurrency-ddj.htm">But there's a
limit to the free lunch a CPU can give our serial software</a> &mdash; beyond a
certain point we need to look beyond the hardware and within the model our
software implements. So let's put aside the parallel hardware stuff for now
and work on optimizing a single instance &mdash; we'll parallelize it at the end.

<p>

We are still limited by time.
How can we do <a href="http://asserttrue.blogspot.com/2009/03/how-to-write-fast-code.html">more work in less time</a>?
Look at the paddles in the game above &mdash; they spend ~75% of their
time idle and only ~1% actually hitting (or missing) the ball.
Can we reduce the amount of time the paddles sit idle?

<h3>Parallelizing a Serial Algorithm</h3>

<table border="0">
<tr>
<td width="50%" valign="top">

Like a real-world ping pong game our games progress one point after another in a
serial fashion:

<p>

<table class="border">
    <tr>
        <th colspan="4">Time <a style="font-size:xx-large;font-family:Courier New">&rarr;</a>
    <tr>
        <td>point 1
        <td>point 2
        <td>point 3
        <td>&hellip;
</table>

<p>

The serial nature of this algorithm mean that even though we know <em>what</em>
we need to do in advance we can't do it because we're waiting for the previous
step to finish.

<td valign="top">

Ideally, we could work on multiple points at once.

<p>

If we did our chart should look more like this:

<p>

<table class="border">
    <tr>
        <th>Time <a style="font-size:xx-large;font-family:Courier New">&rarr;</a>
    <tr>
        <td>point 1
    <tr>
        <td>point 2
    <tr>
        <td>point 3
    <tr>
        <td>&hellip;
</table>

</table>

<p>

So let's try <em>running multiple points at once in the same game</em>.

<h3>Synchronous vs. Asynchronous</h3>

<div style="width:360px; margin: 5px; padding:5px; background-color:#eef; border:1px solid #99c">
<div id="pong_async" style="margin: 0 auto">&nbsp;</div>
<span style="font:12px/11px Courier New, sans-serif">
ball:{speed:10,<b>count:11</b>}<br>
paddle:{speed:10}
<br>profile:true
</span>
</div>

<p>

This is the
<a href="http://en.wikipedia.org/wiki/Asynchronous_I/O">asynchronous or event model</a>
&mdash; because of the huge latency differential between moving the paddle and
moving the balls we can
<a href="http://en.wikipedia.org/wiki/Multiplexing">multiplex</a> multiple balls
between each paddle.

<p>

This greatly reduces the paddle idle time but comes at a cost in the form
of greater code complexity &mdash; asynchronous code is longer, harder to
reason about and more difficult to maintain than equivalent synchronous code.

<p>

Is this complexity overhead acceptable? It makes the code more error-prone and is thus
relatively more expensive to maintain. However, it's also a lot faster and
our goal is speed, so maybe it's worth the cost after all. How can we tell?

<p>

<a href="http://en.wikipedia.org/wiki/Amdahl's_law">Amdahl's law</a> can be used
to describe expected parallel speedup using the following factors:

<ul>
    <li>\(P\): the fraction of the work that's parallelizable from \(0..1\). In our case 100%, represented as \(1\).
    <li>\(S\): the amount we can speed \(P\) up by. in our case it's the number of balls we play at once: \(11\).
</ul>

\(P \gets 1, S \gets 11\)
<blockquote>
<table>
<tr>
<td style="font-size:large">
\(\frac{1}{(1-P)+(\frac{P}{S})}\)
<td>Amdahl's equation

<tr>
<td style="font-size:large">
\(\frac{1}{(1-1)+(\frac{1}{11})}\)
<td>plug in our numbers

<tr>
<td style="font-size:large">
\(\frac{1}{(\frac{1}{11})}\)
<td>simplify

<tr>
<td>\(11\)
<td>done
</table>

</blockquote>

<p>

Hmm, it just reduces to \(S\).
Embarassingly parallel problems, by definition, are completely parallelizable
and can always by sped-up by \(S\).
So, we can expect our parallel version to run 11 times faster than the original,
and we see that this is indeed reflected in the <i>sec/game</i> statistic
between the serial and parallel versions.

<p>

Ultimately in this case the cost of complexity is acceptable because of the
significant performance gains.

<p>

Are there any more opportunities for improvement?

<p>

The limiting factor is still waiting for the balls.
But they're already travelling at the maximum rate &mdash; is there anything
else we can do?

<p>

Let's consider the term
<em><a href="http://en.wikipedia.org/wiki/Velocity">rate</a></em> (velocity)
defined as:
<blockquote style="font-size:x-large">
\(\bar v = \frac{\Delta d}{\Delta t}\)
</blockquote>

<p>

The only way to increase \(\bar v\) for a given \(\Delta t\) is to increase
\(\Delta d\), but we're already moving at the stated maximum speed (10, as defined in the <a href="#rules">Rules</a>).
But what if we flipped this problem on its head and instead of trying to
go faster we reduced the distance we needed to travel in the first place?

<p>

<div style="float:right; padding-left:3px; margin-left:3px">
    <img alt="distance at 45" src="data:image/png;base64,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">
</div>

Let's look at the distance the balls travel between paddles.
At an angle of incidence of ~45&deg; we know by way of the
<a href="http://en.wikipedia.org/wiki/Pythagorean_theorem">Pythagorean theorem</a>
that the length of the path the ball takes within the rectanglar field of play is:

<blockquote>
\(\sqrt{\max(x,y)^2 + \max(x,y)^2}\)
<p>
\(\sqrt{2 \times \max(x,y)^2}\)
<p>
\(\sqrt{2} \times \max(x,y)\)
<p>
\(\approx 1.4142 \times \max(x,y)\)
</blockquote>

<p>
Given \(x \gets 360, y \gets 240\)
<blockquote>
\(\approx 1.4142 \times \max(360,240)\)
<p>
\(d \approx 509.11\)
</blockquote>

<p>

If we reduce the ball's angle of incidence we can decrease the distance they
need to travel to reach the other side.
The lowest possible angle is 0. Let's try that.

<p style="clear:both">

<div style="width:360px; margin: 5px; padding:5px; background-color:#eef; border:1px solid #99c">
<div id="pong_async_angle0" style="margin: 0 auto">&nbsp;</div>
<span style="font:12px/11px Courier New, sans-serif">
ball:{speed:10,count:11,<b>angle:0</b>}<br>
paddle:{speed:10}
<br>profile:true
</span>
</div>

<p>

With angle=0 distance from one side of the rectangle to the other simply becomes \(x\)
<p>
Given \(x \gets 360, y \gets 240\)
<blockquote>
\(d = 360\)
</blockquote>

We have reduced the constant factor \(\sqrt{2}\) to \(1\).

This is backed up empirically by our profiler.

<p>

This optimization has the side-effect of eliminating the familiar floor/ceiling
<a href="http://en.wikipedia.org/wiki/Ricochet">ricochet</a> effect
altogether.
Though the ricochet is an important part of gameplay we see <em>ppg</em> stats
on par with our original, so we can probably do without it.

<p>

Our increasingly optimal software looks less and less familiar and less
intuitive, but this is ultimately the course that optimization above
all else inevitably leads.

<p>

We've taken it pretty far &mdash; single games run 10&times; faster and a parallel
implementation of an embarassingly parallel problem over N CPUs will, by
<a href="http://en.wikipedia.org/wiki/Amdahl's_law">Amdahl's law</a>,
further improve that remaining 10% by N.

<p>

So, that' probably it, right?

<p>

Despite our work, the balls are <em>still</em> the limiting factor.
The speed of the balls is limited by 
<i>x</i> so let's reduce <i>x</i>!


<p>

<div style="width:360px; margin: 5px; padding:5px; background-color:#eef; border:1px solid #99c">
<div id="pong_async_angle0_halfx" style="margin: 0 auto">&nbsp;</div>
<span style="font:12px/11px Courier New, sans-serif">
x:<b>180px</b>
<br>ball:{speed:10, count:11, angle:0}
<br>paddle:{speed:10}
<br>profile:true
</span>
</div>

<p>

We can of course reduce <i>x</i> even further.

<p>

Because the width is so narrow we also increase the paddle height to block
more balls to achieve a similar ppg.

<p>

<div style="width:360px; margin: 5px; padding:5px; background-color:#eef; border:1px solid #99c">
<div id="pong_async_angle0_leastx" style="margin: 0 auto">&nbsp;</div>
<span style="font:12px/11px Courier New, sans-serif">
x:<b>120px</b>
<br>interGamePause:1sec
<br>ball:{speed:10, count:11, angle:0}
<br>paddle:{speed:10, <b>height:35px</b>, <b>width:5px</b>}
<br>profile:true
</span>
</div>

<p>

The gameplay here progresses much faster not because we've sped up the balls
but because we've reduced the distance they must travel.
Our simulations still vaguely resemble Pong and they run so much faster that
the 1 second pause in-between games displaying the WINNER now represents
a significant portion of the total time.
We can now play 100 games in about the same time as we can play 1 game
of the original.

<p>

Next we eliminate the inter-game ("Winner") pause, which speed us up ~2x:

<p>

<div style="width:360px; margin: 5px; padding:5px; background-color:#eef; border:1px solid #99c">
<div id="pong_async_angle0_leastx_nowinner" style="margin: 0 auto">&nbsp;</div>
<span style="font:12px/11px Courier New, sans-serif">
x:120px
<br>interGamePause:<b>0sec</b>
<br>ball:{speed:10, count:11, angle:0}
<br>paddle:{speed:10, height:35px, width:5px}
<br>profile:true
</span>
</div>

<p>

This produces a game that looks very fast, and indeed it is.
Yet the balls here move no faster than they did originally; their speed has not changed.
Instead we have put that speed to better use.

<p>

Now that we're much closer to optimal we can look at parallelizing...

<p>

<div style="margin: 5px; padding:5px; background-color:#eef; border:1px solid #99c; vertical-align:top; display:inline-block; font-size:12px">
<div id="pong_async_angle0_leastx_nowinner1" style="margin: 0 auto;display:inline-block">&nbsp;</div>
<div id="pong_async_angle0_leastx_nowinner2" style="margin: 0 auto;display:inline-block">&nbsp;</div>
<div id="pong_async_angle0_leastx_nowinner3" style="margin: 0 auto;display:inline-block">&nbsp;</div>
</div>

<p>

We now have approximately 300 times as many games playing themselves out in
the same space/time as the original.
All by bending the original rules.

<p>

Are we there yet? In the real world the answer is yes!
We've done a good job.

<!--p>

But if you're the curious-bordering-on-obsessive type and just had to know,
how could we go further?

<p>

For one, the stated goal is to run simulations and record outcomes, so technically
we could probably get away with generating a beginning state, calculating the outcome
without actually playing and recording the outcome. Since the physics are very simple
(the balls don't interact with each other, phew) we can calculate a ball's complete
lifetime trajectory relatively cheaply. Knowing the geometry of the table, the
location, angle and speed of the balls and the maximum speed of the paddle we can work
out whether a ball will be returned or not.

<p-->

<h3>Conclusions</h3>

Through Pong we've visually demonstrated the benefits of program analysis, profiling,
concurrency and parallel algorithm choice associated with modeling and understanding
risks and benefits of optimizing computer programs.

<p>

Even within the trivially simple model of Pong we discovered multiple
different opportunities for concurrency (game-level and point-level)
and a wide variety of separate factors, some subtle, that affect outcome.

<p>

Ultimately an understanding of the software's model and choice of algorithm to
<a href="http://en.wikipedia.org/wiki/Speedup">reduce the limiting factor</a>
of the idle paddles were far more fruitful than focusing on raw low-level
speed improvement.

<p>

Focusing on the most obvious factor at the outset (ball speed)
ultimately yielded far less speedup than the easily overlooked paddle speed,
but this in turn only became obvious after we looked at the overall algorithm.

<p>

The decisions we've made for the sake of optimization result in faster software,
but comes at the cost of being harder to understand and maintain.

<script type="text/javascript">
$(document).ready(
  function(){

    $('#pong_regular').pong(
        '//raw.githubusercontent.com/rflynn/jquery-pong/master/ball-pink.png',
        {
            /*
             * the trick is to balance 2 comp players that wins some
             * and lose some; if they always score or never score then
             * it's boring.
             */
            height: 180,
            width: 360,
            secondComp: true,
            autoStart: true,
            playTo: 11,
            ballSpeed: 5,
            compSpeed: 3,
            playerSpeed: 3,
            paddleHeight: 20
        });


    $('#pong_profile').pong(
        '//raw.githubusercontent.com/rflynn/jquery-pong/master/ball-pink.png',
        {
            profile: true,
            height: 180,
            width: 360,
            secondComp: true,
            autoStart: true,
            playTo: 11,
            ballSpeed: 5,
            compSpeed: 3,
            playerSpeed: 3,
            paddleHeight: 20
        });

    $('#pong_fastball').pong(
        '//raw.githubusercontent.com/rflynn/jquery-pong/master/ball-pink.png',
        {
            profile: true,
            height: 180,
            width: 360,
            secondComp: true,
            autoStart: true,
            playTo: 11,
            ballSpeed: 10,
            compSpeed: 2,
            playerSpeed: 2,
            paddleHeight: 20
        });

    $('#pong_fast').pong(
        '//raw.githubusercontent.com/rflynn/jquery-pong/master/ball-pink.png',
        {
            profile: true,
            height: 180,
            width: 360,
            secondComp: true,
            autoStart: true,
            ballCount: 1,
            playTo: 11,
            ballSpeed: 10,
            compSpeed: 5,
            playerSpeed: 5,
            paddleHeight: 20
        });

    $('#pong_fast_paddlestoofast').pong(
        '//raw.githubusercontent.com/rflynn/jquery-pong/master/ball-pink.png',
        {
            profile: true,
            height: 180,
            width: 360,
            secondComp: true,
            autoStart: true,
            ballCount: 1,
            playTo: 11,
            ballSpeed: 10,
            compSpeed: 10,
            playerSpeed: 10,
            paddleHeight: 20
        });

    $('#pong_async').pong(
        '//raw.githubusercontent.com/rflynn/jquery-pong/master/ball-pink.png',
        {
            profile: true,
            height: 180,
            width: 360,
            secondComp: true,
            autoStart: true,
            ballCount: 11,
            playTo: 11,
            ballSpeed: 10,
            compSpeed: 10,
            playerSpeed: 10,
            paddleHeight: 20
        });

    $('#pong_async_angle0').pong(
        '//raw.githubusercontent.com/rflynn/jquery-pong/master/ball-pink.png',
        {
            profile: true,
            height: 180,
            width: 360,
            secondComp: true,
            autoStart: true,
            ballCount: 11,
            ballAngle: 0,
            playTo: 11,
            ballSpeed: 10,
            compSpeed: 10,
            playerSpeed: 10,
            paddleHeight: 20
        });

    $('#pong_async_angle0_halfx').pong(
        '//raw.githubusercontent.com/rflynn/jquery-pong/master/ball-pink.png',
        {
            profile: true,
            secondComp: true,
            autoStart: true,
            ballCount: 11,
            ballAngle: 0,
            playTo: 11,
            ballSpeed: 10,
            compSpeed: 10,
            playerSpeed: 10,
            paddleHeight: 20,
            height: 180,
            width: 180
        });

    $('#pong_async_angle0_leastx').pong(
        '//raw.githubusercontent.com/rflynn/jquery-pong/master/ball-pink.png',
        {
            profile: true,
            secondComp: true,
            autoStart: true,
            ballCount: 11,
            ballAngle: 0,
            playTo: 11,
            ballSpeed: 10,
            compSpeed: 10,
            playerSpeed: 10,
            paddleHeight: 35,
            paddleWidth: 5,
            height: 180,
            width: 120
        });

    $('#pong_async_angle0_leastx_nowinner').pong(
        '//raw.githubusercontent.com/rflynn/jquery-pong/master/ball-pink.png',
        {
            profile: true,
            secondComp: true,
            autoStart: true,
            ballCount: 11,
            ballAngle: 0,
            playTo: 11,
            ballSpeed: 10,
            compSpeed: 10,
            playerSpeed: 10,
            paddleHeight: 35,
            paddleWidth: 5,
            height: 180,
            width: 120,
            interGamePause: 0
        });

    /* parallel */
    for (var i = 1; i <= 3; i++)
    {
        $('#pong_sync_' + i).pong(
            '//raw.githubusercontent.com/rflynn/jquery-pong/master/ball-pink.png',
            {
                profile: false,
                height: 60,
                width: 360,
                secondComp: true,
                autoStart: true,
                playTo: 11,
                ballSpeed: 10,
                compSpeed: 2,
                playerSpeed: 2,
                paddleHeight: 10
            });

        $('#pong_async_angle0_leastx_nowinner' + i).pong(
            '//raw.githubusercontent.com/rflynn/jquery-pong/master/ball-pink.png',
            {
                profile: true,
                secondComp: true,
                autoStart: true,
                ballCount: 11,
                ballAngle: 0,
                playTo: 11,
                ballSpeed: 10,
                compSpeed: 10,
                playerSpeed: 10,
                paddleHeight: 35,
                paddleWidth: 5,
                height: 180,
                width: 120,
                interGamePause: 0
            });
    }

  }
);
</script>
<h3>See Also</h3>
<ul>
  <li><a href="https://github.com/rflynn/jquery-pong">jquery-pong</a>
  <li>Herb Sutter <a href="http://www.gotw.ca/publications/concurrency-ddj.htm">The Free Lunch Is Over: A Fundamental Turn Toward Concurrency in Software</a>, Dr. Dobb's Journal, March 2005
  <li>M. D. Hill and M. R. Marty. <a href="http://research.cs.wisc.edu/multifacet/papers/ieeecomputer08_amdahl_multicore.pdf">Amdahl's Law in the Multicore Era</a> IEEE Computer, 41(7):33–38, 2008.
  <li><a href="http://en.wikipedia.org/wiki/Speedup">Speedup</a>, Wikipedia
  <li><a href="http://en.wikipedia.org/wiki/Amdahl's_law">Amdahl's law</a>, Wikipedia
  <li>Kas Thomas <a href="http://asserttrue.blogspot.com/2009/03/how-to-write-fast-code.html">Go fast, do less</a> Mar 23 2009
  <li><a href="http://en.wikipedia.org/wiki/Profiling_(computer_programming)">Profiling</a>, Wikipedia
  <li><a href="http://en.wikipedia.org/wiki/CPU_bound">CPU bound</a>, Wikipedia
  <li><a href="http://en.wikipedia.org/wiki/Multiplexing">Multiplexing</a>, Wikipedia
  <li><a href="http://en.wikipedia.org/wiki/Asynchronous_I/O">Asynchronous I/O</a>, Wikipedia
  <li><a href="http://en.wikipedia.org/wiki/Embarrassingly_parallel">Embarassingly Parallel</a>, Wikipedia
  <li><a href="http://en.wikipedia.org/wiki/Quadtree">Quadtree</a>, Wikipedia
</ul>
