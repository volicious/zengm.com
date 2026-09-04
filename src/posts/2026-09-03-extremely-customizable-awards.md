---
layout: post
title: Extremely customizable awards
date: 2026-09-03
tags:
  - post
  - Awards
  - Custoimzation
  - Settings
---

Previously, all the awards in ZenGM were hardcoded. Want to add a new award? Change the number of All-League teams? Edit how the MVP is determined? Couldn't be done, but now all of that and more is possible!

The main new stuff is at Tools > Award Settings where you can add/edit/delete awards. There is documentation there if you click the "Show documentation" button at the top, but I'll explain some of the coolest features here.

<!--more-->

Setting the formula is the main part of customizing an award. It's kind of like formulas in the GOAT Lab but better. The GOAT Lab just let you operate on raw stat variables, but the awards formulas also support any derived stats (like FG%).

The default formulas are all the same as they were before. Some are very simple (like baseball awards are generally just WAR) but others are pretty complicated. You can now do whatever you want!

I also added `min(x,y)`, `max(x,y)`, and `abs(x)` functions to the formulas, which let you do some pretty clever stuff. Like `-abs(per-15)` gives the players whose PER is closest to 15, which is like a ranking of the most average players. You can use `min` to implement a gradual games played cutoff, like `per * min(1, gp / 60)` equals PER for someone who played 60+ games, and gradually decreases below that. If you want the cutoff to be shaper, add an exponent! `per * min(1, gp / 60)^1000` will be basically 0 for anyone under 60 games played.

There are also some special variables that allow you to implement voter fatigue, which is a commonly requested feature. I thought about just making a "voter fatigue" button, but I think using these variables gives you more control of exactly how it works. Like if you write `ws * 1/(numWon.MVP+1)` - `numWon.MVP` is the number of prior MVPs this player won. So if that value is 0, the result is just ws. But if it's 1, then it's 50% of WS. If 2, then 33% of WS, etc. If that sounds too extreme, you can use an exponent to make the dropoff slower, like `ws * (1/(numWon.MVP+1))^(1/4)`.

In addition to the formula, there are a bunch of other award settings, including:

<figure class="overflow-auto"><img src="/files/extremely-customizable-awards-1.png"></figure>

- Set the name and abbreviation of the award, which will then be used in various places in the UI.

- Previously all awards were leaguewide, but now they can alternatively be done separately for each conference or division. The default ZGMB awards are now by conference.

- Awards can be based on regular season stats, playoff stats, individual playoff series stats (for any round!), or regular season and playoffs combined. (I actually think combined stats make a lot of sense and I'm not sure why no professoinal league does that, at least that I'm aware of)

- Pick which stats to show on the Award Races and Season Summary pages associated with an award (like DPOY will show defensive stats)

- Make individual or team awards, and for teams, specify how many teams and even different formulas for different positions (in the non-basketball sports where that matters more)

- Select filters: rookies only, bench players only, MIP mode (compare to past seasons)

Speaking of rookies, the definition of "rookie" has been improved. Previously a rookie was just a player drafted in the previous draft. That was unsatisfying for players injured or benched their whole first season. Now, "rookie" means "first season a player played" (or in ZGMB there are IP/AB limits similar to MLB). This is a little tricky when creating a new league with no historical stats. In that situation, any players drafted before the previous draft will never be eligible for rookie awards because there is no way to know how much they might have played in prior seasons.

This also means that imported/created players will be considered rookies, which I think makes sense, like in American pro sports when you have older players come from foreign leagues they are treated as rookies.

## More details

There are some nice UI features related to awards too. For instance, on player profile pages, there is now an "Awards" column showing the summary of awards won (and top 5 finishes) each year. This also works for historical seasons in real players leagues!

<figure class="overflow-auto"><img src="/files/extremely-customizable-awards-2.png"></figure>

Related to this, when editing award winners in God Mode you can pick the top 5, not just the winner.

When you customize the awards, the UI automatically displays your custom awards in all the places it should - the season summary, the league history table, the award records page, the awards summary on player profiles, etc. A nice little detail on the awards summary: when you hover over team awards, it groups them by number.

<figure class="overflow-auto"><img src="/files/extremely-customizable-awards-3.png"></figure>

The Award Races page used to only show individual regular season awards, but now it shows everything, including playoff awards and team awards. It also has a "Score" column showing the numeric value of the award formula, which previously was never displayed anywhere.

In real players leagues in BBGM, awards change over time just as they did in real life. So when you start a league in 1947, the only award is All-League (can you believe there was no MVP until 1956?). The number of teams for team awards also changes over time.

Also for team awards in default leagues, I changed the number of teams to be more in line with the equivalent pro leagues.

On the Team Records page it shows how many times a team won its division. Previously it only tracked that for conferences or the entire league.

## Achievements

What about achievements? Well you don't need God Mode to edit the award settings, so you can still win achievements in a league with edited awards. But some achievements are based on awards. Those are affected by your award settings, and it works really well!

All of the award-based achievements mention specific awards (MVP, ROY, etc.) in their descriptions. If you don't edit any of the awards used by an achievement, then you can still get that achievement even if you edit other awards. You can also edit superficial parts of the award, like the name. But once you start editing things that change the results of the award (such as the formula) then that award will no longer be used by achievements.

The one exception is if you edit the "group" of the award to make it _more_ difficult to win. Currently this is only possible in ZenGM Baseball, which has conference-based awards by default. So for instance, if you switch the MVP from conference to leaguewide, that actually makes MVP harder to win. So that will still count toward ZenGM Baseball achievements.

I mentioned above that you can change superficial parts of the award like the name and still be eligible for achievements. This is true even for the abbrev, like if you edit the MVP award so the abbrev is now "ABC" or whatever, that will still count as an MVP award for achievements as long as all the meaningful parts of the award are unedited. It works by scanning all the details of the award setting and comparing it to the default awards, not by naively matching the name/abbrev!

## Upgrading existing leagues

When you load/import an existing league, it will be upgraded to the new format. This will be a little slow if you have thousands of seasons, but shouldn't take more than a few minutes in a huge league, there have been slower updates in the past.

There are a few limitations for upgraded leagues.

It doesn't fill in the 2nd-5th place finishers for past season awards because that would be a lot slower.

In some cases past awards may only be partially upgraded. Like if you create a real players league without all historical data, it will still list all the awards for past season (like "2015 MVP") even though there is no season data from 2015. When upgrading, awards like that will not be converted to the new format and will not be displayed/counted with the "real" awards in every single place. Although many things will still work fine.

## This was hard!

This wound up being [an incredibly complicated feature](https://github.com/zengm-games/zengm/pull/538/changes). It's the type of thing that would have been easier if I had made awards customizable originally. Then as I added features related to awards, I would have gradually improved the customizable awards. Instead, as I added features, I was basically ossifying the hardcoded awards. And then to switch, I had to reimplement all of the awards features that ZenGM had accumulated over the past decade+ of development. Very tricky stuff!

Fortunately I have many users willing to beta test, and they helped me iron out some bugs. I'm sure some slipped through though, please [let me know](https://zengm.com/contact/) if you notice any. Also if you want to keep up to date on any future beta versions (well, usually there aren't any, but sometimes there is...) you can join [the mailing list](https://landing.mailerlite.com/webforms/landing/z7d2z9) or [the ZenGM Discord](https://zengm.com/discord/) which are usually the places I announce things first.

I started working on this at the beginning of August and it took me the whole month to do. I also had to get a little less active in replying to message people sent me, so I apologize if you asked me a question and it fell through the cracks!

Some people may be wondering if I am using AI to help me code these days. The answer is mostly no. I've still never used Claude Code or anything like that. I do rarely ask ChatGPT questions like I would on Stack Overflow in the old days. Overall, ChatGPT probably wrote like 100 lines of code, out of 27,850 in the PR linked above. Specifically, ChatGPT was the one who added support for min/max/abs functions in formulas, and it also helped me convert some TypeScript types to Zod. I could have done all that myself if I spent an extra day working on it. But I'm not really an anti AI coding ideologue, I just enjoy programming myself, and I want to keep doing it.

## Future features

Because it took me so long to implement all this stuff, I had to leave some features out or I never would have finished! A lot of those are little details you might not notice. But some of the bigger ones I want to add in the future are:

- Editing the formulas for the Hall of Fame and All-Star teams. Those were not affected by this update.

- Adding/editing/deleting awards in past seasons. Currently you can edit the winners of awards in past seasons, but you can't actually edit the awards themselves.

We'll get there eventually! But right now I will probably do some smaller unrelated improvements first, and also try to respond to some of the messages I've been ignoring the past month.
