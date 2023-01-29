---
layout: post
title: "Multi Armed Bandits"
tags: ["bandits", "RL"]
mathjax: true
---

This blog provides a brief walkthrough of the multi-armed bandits. I have tried to structure
this blog referring from the book "Reinforcement Learning: An Introduction" by Sutton & Barto.
This blog is intended for anyone who has some knowledge of probability and basic mathematics.

### Literal resemblance

> Bandit: someone who tries to deplete you of your money (or other resources)

In the context of a casino (from which RL derives the analogy), a bandit or a slot machine
is a machine with a lever, wherein you put in some money to pull the lever and the machine 
processes the output, and for some specific outputs, you win (you get monetary reward). You keep
repeating this with the hope of winning, while paying the machine for each lever pull (now you might
get why its called a **bandit**)

### The n-armed bandit

Consider a case where you enter the casino and see not just one, but n different slot machines. They
are different in the sense that some machines give much higher reward (say they generate the winning 
output much more frequently) than others. Let us suppose that you have enough money to pull lever for 
100 turns (assume that pulling lever for all slot machine costs the same). Ofcourse you would want to 
know the slot machine which gives you best expected reward for 100 pulls. This is exactly the
setting for an n-armed bandit.

> n-armed bandit: Given n different possible actions, determine the best action that yields maximal expected reward

We formalize this by saying that there is an agent, that takes an action $a_t$ from among ${A_1, A_2,\ldots,A_n}$
at timestep $t$ and receives a reward $r_t$. The agent does this for $T$ steps. At each step, the agent 
tries to choose $a_t$ such that the expected reward over **T** steps ie, $E[R_T]$ is maximized. 

**Value of an action:** The value of a chosen action at timestep $t$ is the expected reward from that action 
$a_t$. Consider the example of n slot machines: each machine $i$ is designed in such a way that the reward is stochastic
and follows some probability distribution $P_i(r)$. We then say that the true value of pulling the $ith$ lever is
$q(A_i) = E_{P_i}[r]$. In reality however, it isn't often possible to know the true value $q(A_i)$. Instead, 
what we might have is an estimate of the true value ie,$Q(a_t)$.

There can be different ways of estimating the value function $Q(a_t)$. One simple way could be to take the average of
past rewards from that action. For instance, if $a_t = A_i$ (ie, we pull lever from $ith$ machine at timestep $t$),
then $Q(a_t) = average({R_i})$ where ${R_i}$ is the set of past rewards obtained by exercising machine $i$.

Next, we'll briefly go over two basic methods for solving the n-armed bandit problem, *with the assumption that we
somehow know the estimated value function $Q(a_t)$. *

---

### Method 1: Greedy

As the name suggest, this method tries to take action greedily so as to maximize immediate returns. The agent does
this by choosing action with maximum value:

**$a_t = argmax_{A_i} Q_{t} (A_i)$** , where $Q_{t}(A_i)$ is the estimated value function at timestep t.

>Note: We use $Q_t$ because estimation of value function can be an iterative process and thus $Q$ can be different at different timesteps.

While this guarantees that your immediate return is the best possible given your current knowledge, more famously 
called the *exploitation*, this approach overlooks the other counterpart ie, *exploration*; essentially, what it 
means is that it might not be so wise to completely rely on the current knowledge of the value estimates, and the 
agent should rather try to go against the greed to explore some non-greedy action as it might turn out to be more 
rewarding on a long run.



### Method 2: Epsilon-Greedy

To mitigate the issue of being too exploitative by being greedy, there is an alternate method called **epsilon-greedy**.
In this method, the agent still tries to be greedy most of the time, but randomly chooses an action for $\epsilon$
fraction of the time:

$a_t$ = $argmax_{A_i} Q_{t} (A_i)$ if rand>$\epsilon$
$a_t$ = randomly_choose$({A_i})$ if rand<=$\epsilon$, where **rand** is a random variable picked from a uniform distribution $\in$ **[0,1]**

Here, $\epsilon$ is a hyperparameter that controls exploitation v/s exploration ratio. Although it 
might seem very trivial, the method works better than the greedy method for many cases. Infact it is asymptotically
guaranteed to be better than or equal to the greedy method $\forall \epsilon < 1$
