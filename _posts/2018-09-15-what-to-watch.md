---
layout: post
title: "Paradox of Choice: What to Watch?"
date: 2018-09-15
---

In August 2017 [Moviepass](https://www.moviepass.com/) announced an offering of [1-movie-per-day in any supported theater for *$9.95/mo*](https://en.wikipedia.org/wiki/MoviePass#2015–2017).  Given that 1 movie-ticket-per-day over 1 month costs $350 in my area the offer was an extraordinary value.  How does one make the most of this?

The offering naturally lead to a number of questions: [What should I watch and when?](https://en.wikipedia.org//wiki/Prioritization)  How can I watch as many of the "best" movies?  How does one define "best"?  How many can one practically attend anyway?  How many movies are there in my area in a month?  Do all theaters just play the same movies?  If not, how do they differ?  How many movie theaters can I actually get to?  Are movies the new tv? What is consuming a movie actually worth to me?  Is it better to watch a "worse" movie more cheaply, or spend more to travel farther to see a "better" one?  How accurately can I predict what I will like?  Should I adjust my expectations to fit the films that are available the most cheaply, or, given greater access should I increase my selectivity?  Should I focus on stuff I'm more likely to enjoy, or should I take greater risk by broadening my horizons?  And perhaps most importantly -- how do I take maximum advantage of this offer before it [inevitably ends](https://en.wikipedia.org/wiki/Burn_rate)?

Given this [paradox of choice](https://en.wikipedia.org/wiki/The_Paradox_of_Choice) I ended up building a "[TV guide](https://www.tvguide.com/listings/) for my local movie theaters" a daily-generated custom report listing the cheapest showing of each movie that day, taking into account the Moviepass discount, transportation cost and travel time:

<img src="/i/making-the-most-of-moviepass-2018/report.png" width="940" height="276">

The report is a custom synthesis of data across a number of sources -- [Fandango](https://www.fandango.com/) for movie listings, [Moviepass](https://www.moviepass.com/movies) for supported films by day, [RottenTomatoes](https://www.rottentomatoes.com/) for critic reviews, [Cinemascore](https://www.cinemascore.com/) for opening weekend audience exit polling, [Metacritic](https://www.metacritic.com/) for critic and audience reviews, Festival and award and "best of" lists for cultural indicators from a number of sources, [BoxOfficeMojo](https://www.boxofficemojo.com/) for box office stats from and costs from my own _theatrecost_ spreadsheet. The report also outputs a number of factors which are currently hardcoded into formulae; eventually these could be used for machine learning to derive better algorithms.

Alas, with Moviepass [switching from 1-per-day to 3-per-month](https://www.cnet.com/news/moviepass-new-3-movies-per-month-for-10-plan-is-live/) to curtail their [unsustainably high burn rate](https://www.hollywoodreporter.com/news/moviepass-parent-discloses-loss-486-million-first-quarter-1112334) it means this type of daily reporting will be much less valuable -- extracting max value will be more dependent on forecasting the most "interesting" movies between billing periods in a highly personalized way, and then deciding what is worth paying for... a lot less "juice for the squeeze" for cinephiles on a budget. Oh well, it was fun while it lasted ¯\\\_(ツ)\_/¯

## Further Reading

1. [*The Paradox of Choice*](https://en.wikipedia.org/wiki/The_Paradox_of_Choice)
2. [how do you pick movies to watch?](https://old.reddit.com/r/movies/comments/b23kid/how_do_you_pick_movies_to_watch/)
3. [*Moviepass* topic on news.google.com](https://news.google.com/topics/CAAqJAgKIh5DQkFTRUFvS0wyMHZNSEptYURKamJoSUNaVzRvQUFQAQ?hl=en-US&gl=US&ceid=US%3Aen)
