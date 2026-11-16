
### Project 5: Policies

#### Due: Wed Nov 25, 11:59pm.


Grading rubric:
- Exemplary: exemplary on both tasks.
- Satisfactory: At least satisfactory on both tasks.
- Unsatisfactory: At least one task unsatisfactory

In this project, you'll see a few different ways to construct a policy. We'll solve for 
it mathematically, and then learn it. If you complete the optional question, you'll also 
implement the algorithms that allow us to discover an optimal policy in the case where we can't
find it mathematically.

**Question 1. Value of Information** In this question, you'll use your knowledge of probability, 
utility and information to assist in a search-and-rescue operation. You can either write your answers
out by hand and take a picture or create a PDF.

Suppose that we have been asked by the National Park Service to assist in a search-and-rescue operation.

A hiker has been lost in a canyon, and we need to find them before nightfall. We know that they are 
in one of three areas, with these probabilities:

| Area | Side of the ridge | Chance the hiker is there |
| --- | --- | --- |
| A: Eagle Canyon | East | 0.5 |
| B: Pine Flats | West | 0.3 |
| C: Lake Basin | West | 0.2 |

We only have time to search one area. We decide to model this as a utility problem so that we
can use what we know about policies. We decide that a successful rescue is worth 1000, and an 
unsuccessful rescue is worth 0. 

*Step 1* Without any other information, what is the expected utility of searching each area? Which
one should we choose?

*Step 2* Perfect Information. Suppose that a magical being told you exactly where the hiker was. What is the
difference between this utility and your answer in step 1? This is called *perfect information.*  This is our upper bound on
the problem.

We now have the opportunity to gather some information. 

| Test | What it reports | How reliable | Time needed |
| --- | --- | --- | --- |
| Drone scan | Flies over one area you choose and reports "seen" or "not seen" | Sees the hiker 80% of the time if they are there; never reports a false sighting | 1 hour |
| Phone ping | Whether the hiker's phone last connected east or west of the ridge | Correct 90% of the time | 15 minutes |
| Witness report | Another hiker says which side of the ridge they saw someone | Correct 60% of the time | None: the report is already in |

We've only got time to use one of them, so we need to determine which is the most useful.

*Step 3.* Consider the drone scan. 

- For each area A, B, C, what is P(area | seen) and p(area | !seen)? 
- For each area, what is the best area to search if the hiker is seen in that area? 
What is the expected utility of that search?
- For each area, what is the best area to search if the hiker is not seen in that area? 
What is the expected utility of that search?
- What is the value of information of searching each area? Which area yields the most information?
- What is the difference in expected utility before and after searching that area? That is the *value of information*
gained from that test.

*Step 4* Repeat step 3 for the phone ping. Which area should we ping? How much is this information worth?

*Step 5* Repeat step 3 for the witness report. Why is the witness report not helpful?

*Step 6* Which of the three tests should we use and why?

*Step 7* We need to also account for time. It turns out that the probability of finding the hiker
declines as it gets dark. 

| Team leaves | Chance of finding the hiker in the right area |
| --- | --- |
| Now, or after reading the witness report | 100% |
| After the phone ping (15 minutes) | 95% |
| After a drone scan (1 hour) | 80% |

Multiply each gain in expected utility by the decline in probability of success. Does this change
your recommendation? 

**Question 2: Blackjack with Q-Learning** 

In this question, you'll return to the Blackjack setup from Project 1. You may use Claude (or whatever coding agent you like)
to assist you.

You should extend your program from Project 1 so that it implements Q-learning in order to learn a policy for Blackjack.
Keep the same assumptions and setup as you had in Project 1 regarding dealing, holding, etc. Your state representation
should remain the same as well.

Allow the user to modify the reward, discount factor and exploration strategy and compare the 
performance of Q-learning to Monte Carlo in learning a policy. This can include time to convergence 
as well as effectiveness of the learned policy. You might want to have a discussion with Claude about how to
measure and evaluate this. 

You should provide:
- All code, either written by you or a coding agent
- All prompts or design documents you create. (I strongly suggest getting your coding agent to
first create a design spec or PDD and then use that to generate the app.)
- A short (<5 min please) video demonstrating your result. You should compare the result of Q-learning
and Monte Carlo, show how you know these results are correct, and explain how your code works sufficiently well
to show that you know what it does.

**Question 3: (Grad students only) Reinforcement Learning with Human Feedback**

Reinforcement Learning has proved to be an effective way to fine-tune the question-answering 
abilities of large language models. Once the models are initially trained on a corpus, 
humans are used to then generate feedback that helps the LLM learn to distinguish between 
answers.

[This article from HuggingFace explains more about it](https://huggingface.co/blog/rlhf). It's great, but still
is pretty dense, with a lot of terminology.

Use your favorite LLM-based tool (I recommend [NotebookLM](https://notebooklm.google.com/) for this, but you can use
whatever you want) to create a summary of this article that explains the main ideas and helps you to
understand them. Include the results in your repo.

I'm looking for thoroughness, creativity, and effective use of LLMs as a tool for helping to learn complex ideas.
"Hey ChatGPT summarize this article" is not the idea. Feel free to generate links, diagrams, lists,
or whatever learning tools you think are most effective.


**Question 4: (optional exam replacement question) Value and Policy Iteration**

For this problem, you'll implement the value iteration and policy iteration algorithms. 

I've provided some setup code for you that loads in the states and probabilities, and computes their expected utility.
I've also provided the setup for two problems: 

1. The map shown in R&N, and in the slides. This is in rnGraph. This is a good problem for getting 
started and testing, but not very exciting.

2. A simplified Mars map. In this problem, we return to the Mars lander. Good news: we've gotten the sensors working,
so we know where we are now! Also, we've gotten the rockets working, so we can try to navigate 
to the landing site. We need to land quickly; there are sandstorms coming!

Of course, there are challenges. There are two sandstorms here already; if the lander flies 
through them it will be damaged. Even worse, the rockets are not *quite* working. With p=0.8, 
they go in the intended direction, but with p=0.2, they go in the opposite direction! 

More specifically:
For every state (x,y) there are four possible actions: left, right, up, down.
With p=0.8, the lander moves in the intended direction. with p=0.2, the lander moves in the
opposite direction. If a movement would cause the lander to move off the map, it remains in place.

The lander starts in 1,1, and needs to reach the landing site in 5,5. There are sandstorms in 2,3
and 5,2.

If the lander reaches the landing site, it receives a reward of 1 and lands. Hooray!
If the lander reaches either sandstorm, it receives a reward of -1 and is forced to the ground. 
Bummer. We'll have to go get it after the storm is over.

 
*Step 1*. Complete computePolicy(). Given utilities set in self.utilities, compute the 
resulting policy and return it as a dictionary mapping states to actions.

I've given you a unit test for this. Test it with rnGraph.

*Step 2* Complete the lander.actions file with the transitions for all the remaining states.

*Step 3* Implement the value iteration algorithm using r=-0.04 and gamma=0.8.

*Step 4* How can you test the resulting policy? We can use Monte Carlo simulation.
Write a function that takes the generated policy as input, along with a starting state and a number 
of iterations, and returns a list of final states.

*Step 5* Implement the policy iteration algorithm r=-0.04 and gamma=0.8. You can use
Monte Carlo to test this as well.


