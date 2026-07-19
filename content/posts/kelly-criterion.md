---
title: "The Kelly Criterion"
date: 2026-07-11
publishdate: 2026-07-11
lastmod: 2026-07-11
summary: "A Bell Labs engineer solved a telephone noise problem in 1956 and accidentally invented the formula professional gamblers and investors use to size every bet. Betting the mathematically obvious amount is usually a mistake."
tags: ["risk", "strategy", "probability"]
image: /images/kelly-criterion.jpg
draft: true
---

![A Bell Labs engineer solved a telephone noise problem in 1956 and accidentally invented the formula professional gamblers and investors use to size every bet. Betting the mathematically obvious amount is usually a mistake.](/images/kelly-criterion.jpg)
*A roulette table, the game Shannon and Thorp's wearable computer was built to beat. Photo: [Sozi](https://commons.wikimedia.org/wiki/File:Roulette-Tisch.jpg). Public domain.*

## The Kelly Criterion

In the early 1960s, Claude Shannon and Ed Thorp built a computer the size of a cigarette pack, wired it to a shoe, and wore it into a Las Vegas casino. It timed a roulette wheel and predicted which octant the ball would land in, giving them a 44 percent expected edge on that bet{{< cite 1 "Thorp, Edward O. (2017). A Man for All Markets: From Las Vegas to Wall Street, How I Beat the Dealer and the Market. Random House." >}}. The wires kept breaking. The earpiece kept falling out. But the math behind it worked, and it wasn't roulette math. It was a formula for how much to bet, not what to bet on.

That formula came from a colleague of Shannon's at Bell Labs named John Kelly, and he hadn't been thinking about casinos at all.

## What Is the Kelly Criterion?

In 1956, John Kelly published a paper solving a problem in telephone engineering, how to interpret information sent over a noisy line{{< cite 2 "Kelly, J.L. (1956). A New Interpretation of Information Rate. Bell System Technical Journal, 35(4): 917-926." >}}. He wanted to title it "Information Theory and Gambling." AT&T's executives made him change it to the blander "A New Interpretation of Information Rate," worried the company's name shouldn't be anywhere near betting{{< cite 3 "Poundstone, William (2005). Fortune's Formula: The Untold Story of the Scientific Betting System That Beat the Casinos and Wall Street. Hill and Wang." >}}.

Kelly had shown that if you know your edge, the probability that a bet pays off and the odds it pays at, there's an exact fraction of your bankroll you should risk on it. Bet less than that fraction and you're leaving growth on the table. Bet more and you're not just risking more, you're making your long-run outcome worse, not better. The formula is simple. f* = (bp - q) / b, where b is the net odds, p is your probability of winning, and q is the probability of losing.

## Why It's Not About Expected Value

The obvious strategy for a bet with positive expected value is to bet everything you have on it, repeatedly. Expected value maximization says so directly. Kelly's insight was that this obvious strategy is wrong the moment you're betting the same pot more than once, because a single loss at 100 percent staked ends the game. There is no bet size big enough to recover from betting your whole bankroll and losing once.

Kelly maximized something different, the expected logarithm of wealth, which is the same thing as the long-run compound growth rate. That's a subtly different target than expected value, and it produces a subtly different answer. It says bet in proportion to your edge, not in proportion to how good the bet looks in a single shot.

## Where This Shows Up Today

Thorp took the same logic from roulette into blackjack, publishing card-counting strategies that used Kelly sizing, bet small when the deck favors the house, bet large when it favors the player, in a book that became a bestseller and got him banned from casinos across the country{{< cite 1 "Thorp, Edward O. (2017). A Man for All Markets: From Las Vegas to Wall Street, How I Beat the Dealer and the Market. Random House." >}}.

Warren Buffett has never named Kelly directly, but his portfolio behaves like a fractional Kelly bettor's. In 1968, he wrote to his partnership that a single position, American Express, had grown to 40 percent of the fund's assets, the largest concentration he'd ever held, because he was confident enough in the edge to size the bet accordingly{{< cite 4 "Looking Back at Warren Buffett and American Express in 1962. GuruFocus." >}}. He runs concentrated, not diversified, and he almost never uses leverage. Concentration says he sees an edge. No leverage says he still respects the possibility he's wrong.

## Common Mistakes

**Betting full Kelly in practice.** The formula's optimal fraction is also brutally volatile. A bettor sizing every wager at full Kelly should expect a 50 percent drawdown at some point even while playing an actually winning strategy. Most professionals bet a fraction of Kelly, often half, trading some growth for a survivable amount of variance.

**Confusing Kelly with expected value maximization.** They agree on which side of a bet to take. They disagree completely on how much to risk. Treating Kelly's output as "the biggest bet that still makes sense" misses the entire point of the formula.

**Trusting an estimated edge as if it were a known one.** Kelly's formula is only as good as your estimate of p, the probability of winning. Overestimate your edge, which almost everyone does, and the formula will confidently tell you to overbet.

**Ignoring correlation between bets.** The formula assumes each bet's outcome is independent of the others. A portfolio of five "independent" bets that are actually all exposed to the same underlying risk isn't five small Kelly bets. It's one big one wearing a disguise.

## Put It Into Practice

Before sizing any bet, whether it's money, time, or headcount, write down your actual estimated edge as a number, not a feeling. If you can't state a probability and a payoff, you don't have enough information to size the bet at all, let alone maximize it.

Default to fractional Kelly, not full Kelly, for anything you plan to do more than once. Half the theoretical optimum gives up less growth than the drawdown it avoids is worth.

Ask whether your current bets are actually independent or secretly correlated. A string of decisions that all depend on the same underlying assumption is a single concentrated bet, whether or not it looks that way from the inside.

## Go Further

**The full story.** William Poundstone's Fortune's Formula covers Shannon, Kelly, and Thorp in far more depth than fits here, including the mob connections and Wall Street chapters this post doesn't touch{{< cite 3 "Poundstone, William (2005). Fortune's Formula: The Untold Story of the Scientific Betting System That Beat the Casinos and Wall Street. Hill and Wang." >}}.

**The software translation.** Kent Beck has argued the Kelly Criterion explains his Explore, Expand, Extract framework for how product teams should size their bets differently at different stages{{< cite 5 "Kent Beck (2021). Post on deriving 3X from the Kelly Criterion. X (formerly Twitter)." >}}, worth reading as a direct application outside gambling and investing entirely.

---

## References

<ol class="references">
  <li id="ref-1">Thorp, Edward O. (2017). <em>A Man for All Markets: From Las Vegas to Wall Street, How I Beat the Dealer and the Market</em>. Random House. <a href="https://www.penguinrandomhouse.com/books/178551/a-man-for-all-markets-by-edward-o-thorp/">https://www.penguinrandomhouse.com/books/178551/a-man-for-all-markets-by-edward-o-thorp/</a></li>
  <li id="ref-2">Kelly, J.L. (1956). "A New Interpretation of Information Rate." <em>Bell System Technical Journal</em>, 35(4): 917-926. <a href="https://archive.org/details/bstj35-4-917">https://archive.org/details/bstj35-4-917</a></li>
  <li id="ref-3">Poundstone, William (2005). <em>Fortune's Formula: The Untold Story of the Scientific Betting System That Beat the Casinos and Wall Street</em>. Hill and Wang. <a href="https://us.macmillan.com/books/9780809045990/fortunesformula/">https://us.macmillan.com/books/9780809045990/fortunesformula/</a></li>
  <li id="ref-4">"Looking Back at Warren Buffett and American Express in 1962." GuruFocus. <a href="https://www.gurufocus.com/news/956265/looking-back-at-warren-buffett-and-american-express-in-1962">https://www.gurufocus.com/news/956265/looking-back-at-warren-buffett-and-american-express-in-1962</a></li>
  <li id="ref-5">Kent Beck (2021). Post on deriving 3X from the Kelly Criterion. X (formerly Twitter). <a href="https://x.com/KentBeck/status/1379564472335880195">https://x.com/KentBeck/status/1379564472335880195</a></li>
</ol>

---

## Outtakes

**Wearable computers in casinos are now specifically illegal because of Shannon and Thorp.** Nevada Revised Statutes 465.075, added in 1985, makes any device used to predict or analyze a game's outcome a felony, a direct response to what the two of them built decades earlier ([Nevada Legislature, 1985](https://www.leg.state.nv.us/nrs/nrs-465.html)).

**Shannon quietly out-invested almost everyone.** From the late 1950s to 1986, his personal stock portfolio returned about 28 percent annually, and an August 1986 Barron's survey of 1,026 mutual funds found only one that had beaten him ([Poundstone, 2005](https://us.macmillan.com/books/9780809045990/fortunesformula/)).

**Thorp beat Black and Scholes to their own formula.** In 1967, years before their famous 1973 paper, Thorp had already derived essentially the same options-pricing formula and kept it proprietary for his own trading, a story he told himself years later ([Thorp, 2002](https://www.wilmott.com/i-knew-i-knew-part-i/)).

**Ant colonies run the same math without knowing it.** When two foraging patches have a 20 percent and an 80 percent chance of holding food, colonies split their foragers between the sites in close to that same ratio, the Kelly-optimal deployment for maximizing colony growth ([Baddeley, Franks, and Hunt, 2019](https://pmc.ncbi.nlm.nih.gov/articles/PMC6731492/)).

---

## Changelog

**2026-07-11** Initial release.  
