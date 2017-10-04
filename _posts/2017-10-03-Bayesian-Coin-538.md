---
layout: post
title: Bayes' Puzzle Powers
visible: 1
description: Solving a 538 puzzle with Bayesian updating using R
tags:
- Bayesian
- Coin flip
- 538
---

This week's [538 Riddler](http://fivethirtyeight.com/features/rock-paper-scissors-double-scissors/) is one of my favorites so far: 

"On the table in front of you are two coins. They look and feel identical, but you know one of them has been doctored. The fair coin comes up heads half the time while the doctored coin comes up heads 60 percent of the time. How many flips — you must flip both coins at once, one with each hand — would you need to give yourself a 95 percent chance of correctly identifying the doctored coin?"

I suspect the answer he is looking for is the [estimator of true probability](https://en.m.wikipedia.org/wiki/Checking_whether_a_coin_is_fair#4), which returns 369 flips needed for 95% confidence. This works, but I think we can do better with a Bayesian approach. 

Instead of flipping both until we are certain they produce two different distributions, what if we pick a coin, start with a prior probability of 50%, and then update our belief after each flip? Simulating this method in R, I find that it only takes an average of 74 flips to be more than 95% certain which coin is fair. The cool thing is, it is possible to be 96% confident in as few as eight flips, if the data come in a certain way. 

In other words, Bayesian updating identifies the correct coin almost 5x faster in terms of flips needed.

I've pasted the code below. I'm not 95% certain this is correct, or that it fits within the rules of Oliver's riddle. I ported the code for updating directly from Bayes' formula, after all, and I devised the test to see if it works. I'm also doubtful it's the only method that arrives at a solution so quickly. There must be a frequentist approach, right? What about [Pearson's test](https://en.wikipedia.org/wiki/Likelihood-ratio_test#Coin_tossing) after each flip? Would that also converge on 74?

That said, this does seem like the ideal kind of problem for Bayesian sequential updating. The prior is certain, and relatively high. Thinking beyond coins, which are always a proxy for more interesting real-world binomial problems, it's easy to see the value in a 254 year old formula.         

{% highlight R %}
#### Method two: Bayesian updating simulation ####
# H = coin1 = fair 

update <- function(prior, coin_randomizer) {
  # This function updates our prior probabilities after looking at both coins
  
  # Flip
  # The biased coin is randomly selected. 
  # They both return .4 or .6 because we plug these figures into Bayes' theorem below
  # But actual heads/tails probabilities are either 50/50 or 60/40
  flip_coin1 <- sample(c(.4,.6), 1, prob = c(coin_randomizer[1], 1 - coin_randomizer[1]))
  flip_coin2 <- sample(c(.4,.6), 1, prob = c(coin_randomizer[2], 1 - coin_randomizer[2]))
  
  # Hypothesis given the data
  # Bayes Theorem = prob(A|X) =
  # Numerator
  # P(X|A)*P(A) /
  # Denominator
  # P(X) = P(X|A)*P(A) + P(X|not A) * P(not A)
  
  # Prob coin1 is fair given flip1
  posterior_after_flip1 <- (.50 * prior) / ((.50 * prior) + flip_coin1 * (1 - prior))
  
  # Prob coin1 is fair given flip2 and the priors informed by flip1
  posterior_after_flip2 <- (flip_coin2 * posterior_after_flip1) / 
    ((flip_coin2 * posterior_after_flip1) + .50 * (1 - posterior_after_flip1))
  
  return(posterior_after_flip2)
}


simulate <- function(x) {
  
  # We randomly assign the coins 
  coin_randomizer <- sample(c(.5, .4), 2)
  
  # Keep track for checking later
  is_fair_coin1 <- if_else(coin_randomizer[1] == .5, 1, .1)
  
  # Start here
  n_flips <- 0
  prior <- .5
  
  while(prior >= .05 & prior <= .95){
    prior <- update(prior, coin_randomizer)
    n_flips <- n_flips + 1
  }
  
  # Just for updating progress
  print(x)
  # Return how long it took to be 95% sure
  return(n_flips)
  
}

n_sims <- 2e6

sim_results <- 1:n_sims %>% 
  map(function(x) simulate(x)) %>% 
  unlist()

summary(sim_results)
hist(sim_results)



# Let's make sure we get the right # of true positives
test_the_sim <- function(x) {
  
  # We randomly assign the coins 
  coin_randomizer <- sample(c(.5, .4), 2)
  
  # Keep track for checking later
  is_fair_coin1 <- if_else(coin_randomizer[1] == .5, 1, .1)
  
  # Start here
  n_flips <- 0
  prior <- .5
  
  while(prior >= .05 & prior <= .95){
    prior <- update(prior, coin_randomizer)
    n_flips <- n_flips + 1
  }
  
  # Just for updating progress
  print(x)
  return(prior * is_fair_coin1)
  
}


# Don't need as many tests
n_tests <- 20000

test_results <- 1:n_tests %>% 
  map(function(x) test_the_sim(x)) %>% 
  unlist()


true_positivies <- sum(test_results > .95)
true_negatives <- sum(test_results < .004)

(true_negatives + true_positivies) / n_tests

# !It works 96% of the time
{% endhighlight %}













