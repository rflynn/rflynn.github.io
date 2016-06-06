---
layout: post
title: "humanRandom"
date: 2014-05-05
summary: Sometimes you can improve randomness by making it less random.
---

Sometimes you can improve randomness by making it less random.

<style>
body,table,div { font-family: 'Open Sans', Arial, Helvetica, sans-serif !important; }
.boring { color:#5f7c65; color:green }
.dupe { color:red; background-color:#fffcdc; border:1px solid #f6f0a9; border-right:0 }
.prompt { font-weight:bold; color:#999 }
code { background-color:#f6fff8; background-color:inherit; border-left:1px solid green; display:block; white-space:pre; padding:5px; font-size:smaller; line-height:1.1em; font-family: "Liberation Mono", Courier New, sans-serif; }
</style>

<script>

function randomRange(lo, hi)
{
    return lo + Math.floor(Math.random() * (hi - lo + 1));
}

// human-friendly random numbers
// humans don't respond well to true randomness, so track recent state
var humanRandomState = {};
function humanRandomRange(key, lo, hi)
{
    var REMEMBER = Math.min(Math.floor(Math.sqrt(Math.max(hi - lo, 0))), 10); // sqrt(range), up to 10
    var TRIES = Math.max(REMEMBER * 2, 1);
    var state = humanRandomState[key] || [];
    var r;
    // try a finite number of times to generate a random number not in state...
    for (var i = 0; i < TRIES; i++) {
        r = randomRange(lo, hi);
        if (state.indexOf(r) == -1) break;
    }
    state.unshift(r); // save new state
    if (state.length > REMEMBER) {
        state.pop(); // flush old state
    }
    humanRandomState[key] = state; // persist
    return r;
}

</script>

<!--div style="float:right; font-size:smaller; padding:0.5em; max-width:20em; background-color:#EFF8FB">
examples in javascript
</div-->

When generating user-facing content true randomness is often problematic.

<p>

<code><span class="prompt">&gt;&gt;&gt;</span> from random import randint
<span class="prompt">&gt;&gt;&gt;</span> print(''.join(u'●■◆▲'[randint(0,3)] for _ in range(0,50)))
"●■◆■■●●●●◆▲▲●▲●◆●▲▲▲■■■■▲▲■■◆◆◆●▲■●▲▲▲◆▲◆●■■■◆●●■●"
</code>

<p>

Consider using this sequence to layout a series of test images, select a random quote or to populate some "fill" data.

<p>

Something doesn't look right, they'll say. I don't like it. Doesn't <em>feel</em> right. Why?

<p>

<code><span class="boring">●■◆<span class="dupe">■■</span><span class="dupe">●●●●</span>◆<span class="dupe">▲▲</span>●▲●◆●<span class="dupe">▲▲▲</span><span class="dupe">■■■■</span><span class="dupe">▲▲</span><span class="dupe">■■</span><span class="dupe">◆◆◆</span>●▲■●<span class="dupe">▲▲▲</span>◆▲◆●<span class="dupe">■■■</span>◆<span class="dupe">●●</span>■●</span></code>

<p>

Look at all that contiguous repetition. <em>&quot;Four</em> <span style="color:green;font-family:Courier New,sans-serif">■</span><em>s in a row?! That's not random!&quot;</em>

<p>

Of course it <em>is</em> random &mdash; indeed, the very definition of random &mdash; but people instinctively hate it.

<p>

<em>Improve your randomness by making it less random.</em>

<p>

Let's track the state of previous random choices and ensure that we don't generate the same result too often.

<p>

<code>from math import ceil, sqrt

humanRandomState = {}
def humanRandomRange(key, lo, hi):
    remember = min(ceil(sqrt(max(hi - lo, 0))), 10)
    tries = max(remember * 2, 1)
    state = humanRandomState.get(key) or []
    r = None
    for i in range(0, tries):
        r = randint(lo, hi)
        if r not in state:
            break
    state.insert(0, r)
    if len(state) > remember:
        state.pop()
    humanRandomState[key] = state
    return r
</code>

<p>

Is it worth it? The results:

<p>

<code><span class="prompt">&gt;&gt;&gt;</span> print(''.join(u'●■◆▲'[humanRandomRange('shapes',0,3)] for _ in range(0,50)))
"■▲■◆▲◆▲●◆■◆▲◆▲●▲●■◆●■▲●◆●▲●▲■●●◆▲●◆●◆▲■◆●▲●■●◆■▲●■"
</code>

<p>

Only a single dupe:

<p>

<code><span class="boring">"■▲■◆▲◆▲●◆■◆▲◆▲●▲●■◆●■▲●◆●▲●▲■<span class="dupe">●●</span>◆▲●◆●◆▲■◆●▲●■●◆■▲●■"</span></code>

<p>

Ahh, that looks better. Much more random, they'll say.
<h3>References</h3>
<ul>
    <li><a href="http://seanmonstar.com/post/708989796/a-less-random-generator">A Less-Random Generator</a> &mdash; very clean shuffle-based one. not quite random enough for my needs.
</ul>
