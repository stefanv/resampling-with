---
jupyter:
  jupytext:
    metadata_filter:
      notebook:
        additional: all
        excluded:
        - language_info
    text_representation:
      extension: .Rmd
      format_name: rmarkdown
      format_version: '1.0'
      jupytext_version: 0.8.6
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
resampling_with:
    ed2_fname: 05-Chap-1
---

# The Resampling method {#resampling-method}

This chapter is a brief introduction to the resampling method of solving
problems in probability and statistics. A simple illustrative problem is
stated, and then the step-by-step solution with resampling is shown,
using both by-hand methods and a computer program. The older conventional
formulaic approach to such a problem is then discussed. The conventional
analytic method requires that you understand complex formulas, and too often
one selects the wrong formula. In contrast, resampling requires that you first
understand the physical problem fully. Then you simulate a statistical model of
the physical problem with techniques that are intuitively obvious, and you
estimate the probability with repeated random sampling.

## The resampling approach in action

Recall the problem from section \@ref(what-problems) in which the contractor
owns 12 ambulances. The chance that any one ambulance will be unfit for
service on any given day is about 1 in 10, based on past experience. You want
to know the probability that on a particular day---tomorrow--- *three or more*
ambulances will be out of action. The resampling approach produces the
estimate as follows.

### Randomness from physical methods

We collect 10 coins, and mark one of them with a pen or pencil or tape
as being the coin that represents "out-of-order;" the other nine coins
stand for "in operation." This set of 10 coins is a "model" of a
situation where there is a one-in-ten chance---a probability of .10 (10
percent)---of *one* particular ambulance being out-of-order on a given day.
Next, we put the coins into a little jar or bucket, draw out one coin, and
mark down whether or not that coin is the coin marked "out-of-order."
That drawing of the single coin from the bucket represents the chance that
any one given ambulance among our 12 ambulances (perhaps the one with the
lowest license-plate number) will be out-of-order tomorrow.

Then we put the drawn coin back in the bucket, shake all the coins, and
again draw out a coin. We mark down whether that second-drawing coin is
or is not the "out-of-order" coin, and that outcome stands for a second
ambulance in the fleet. We do this *12* times to represent our 12
ambulances, replacing the coin after each drawing, of course. Those 12
drawings represent one day.

At the end of the 12 draws we count how many out-of-orders we have
got for that "day," checking whether there are *three or more*
out-of-orders. If there are three or more, we write down in another
column "yes"; if not, we write "no." The work we have done up to now
represents one experimental trial of the model for a single day.

Then we repeat perhaps 50 or 100 times the entire experiment described
above. Each of those 50 or 100 experimental trials represents a single
day. When we have collected evidence for 50 or 100 experimental days, we
determine the proportion of the experimental days on which three or more
ambulances are out of order. That proportion is an estimate of the
probability that three or more ambulances will be out of order on a given
day---the answer we seek. This procedure is an example of Monte Carlo
simulation, which is the heart of the resampling method of statistical
estimation.

A more direct way to answer this question would be to examine the firm's
actual records for the past 100 days or 500 days to determine how many
days had three or more ambulances out of order. But the resampling procedure
described above gives us an estimate even if we do not have such
long-term information. This is realistic; it is frequently the case in
the workaday world that we must make estimates on the basis of
insufficient history about an event.

A quicker resampling method than the coins could be obtained with 12
ten-sided dice or spinners. Each one of the dice, marked with one of its
ten sides as "out-of-order," would indicate the chance of a single ambulance
being out of order on a given day. A single pass with the 12 dice or
spinners allows us to count whether three or more ambulances turn up out of
order. So in a single throw of the 12 dice we can get an
experimental trial that represents a single day. And in a hundred quick
throws of the 12 dice---which probably takes less than 5
minutes---we can get a fast and reasonably-accurate answer to our
question. But getting hold of ten-sided dice might be a nuisance.

## Randomness from your computer

Your computer gives you many convenient ways of getting random numbers for
resampling.  You can get random numbers from spreadsheet programs like Excel,
Numbers or LibreOffice [^excel-rand], or from websites like
<https://www.random.org>. We can use these numbers to simulate our problem.
For example, we can ask the computer to make us a random number between 0 and
9 (inclusive) to represent one ambulance. If we say that the digit
0 represents "out-of-order" and the digits 1 -- 9 represent "in operation,"
then any one random digit gives us a trial observation for a single ambulance.
To get an experimental trial for a single day we look at 12 digits and count
the number of zeros. If the number of zeros is three or more, then we write
"yes." We then look at one hundred or two hundred sets of 12 digits and count
the proportion of sets whose 12 digits show three or more ambulances being
"out-of-order." Once again, that proportion estimates the probability that
three or more ambulances will be out-of-order on any given day.

[^excel-rand]: For random numbers in spreadsheets, see the `RAND` and
  `RANDBETWEEN` functions in any of Excel, Numbers or LibreOffice.  But read on,
  and you will soon find that you have outgrown spreadsheets.

Soon we will do all these steps with some Python code, but for now,
let's say we have used one of the methods above to get 12 random numbers from
0 through 9, and we have done that 25 times, to simulate 25 days of 12
ambulances. We could arrange those numbers in a table like table
\@ref(tab:veh-numbers):


Table: (\#tab:veh-numbers)25 simulations of 12 ambulances

|   | T1| T2| T3| T4| T5| T6| T7| T8| T9| T10| T11| T12|
|:--|--:|--:|--:|--:|--:|--:|--:|--:|--:|---:|---:|---:|
|1  |  6|  4|  5|  3|  5|  8|  4|  4|  7|   1|   6|   4|
|2  |  4|  1|  5|  3|  1|  2|  8|  5|  6|   4|   5|   8|
|3  |  5|  9|  4|  1|  0|  3|  7|  5|  0|   5|   1|   2|
|4  |  0|  8|  1|  4|  7|  0|  2|  9|  2|   1|   6|   9|
|5  |  2|  5|  3|  9|  0|  6|  5|  5|  2|   8|   1|   7|
|6  |  0|  0|  6|  5|  8|  9|  1|  5|  6|   9|   5|   4|
|7  |  6|  2|  0|  4|  0|  5|  0|  0|  1|   4|   6|   0|
|8  |  3|  6|  6|  7|  1|  7|  8|  2|  8|   5|   8|   4|
|9  |  2|  1|  6|  3|  3|  1|  8|  1|  2|   2|   0|   9|
|10 |  6|  4|  3|  0|  2|  6|  0|  5|  6|   9|   7|   5|
|11 |  0|  3|  1|  5|  4|  9|  9|  2|  3|   5|   1|   8|
|12 |  4|  2|  1|  4|  9|  3|  2|  7|  3|   8|   0|   6|
|13 |  3|  7|  9|  7|  6|  8|  8|  9|  8|   2|   8|   2|
|14 |  0|  6|  4|  8|  3|  5|  7|  6|  2|   7|   2|   7|
|15 |  7|  1|  6|  1|  9|  0|  4|  9|  1|   8|   8|   0|
|16 |  6|  3|  0|  8|  8|  9|  3|  5|  1|   9|   1|   9|
|17 |  9|  1|  4|  2|  3|  1|  4|  4|  5|   2|   0|   7|
|18 |  7|  5|  7|  2|  1|  7|  6|  9|  4|   7|   9|   4|
|19 |  2|  3|  0|  8|  1|  6|  2|  9|  7|   2|   0|   0|
|20 |  8|  9|  9|  4|  1|  9|  7|  1|  3|   4|   6|   3|
|21 |  3|  3|  2|  4|  5|  4|  2|  3|  8|   4|   5|   6|
|22 |  6|  9|  5|  4|  7|  1|  9|  7|  1|   5|   5|   0|
|23 |  4|  4|  5|  8|  0|  0|  8|  3|  1|   7|   6|   6|
|24 |  9|  6|  5|  4|  9|  8|  5|  7|  2|   9|   9|   1|
|25 |  9|  6|  6|  3|  6|  9|  1|  8|  9|   5|   9|   1|

To get the answer for each day, we count the number of zeros in each row.  The
counts go in the final column called "#0" (for "number of zeros").


Table: (\#tab:veh-numbers-counts)25 simulations of 12 ambulances, with counts

|   | T1| T2| T3| T4| T5| T6| T7| T8| T9| T10| T11| T12| #0|
|:--|--:|--:|--:|--:|--:|--:|--:|--:|--:|---:|---:|---:|--:|
|1  |  6|  4|  5|  3|  5|  8|  4|  4|  7|   1|   6|   4|  0|
|2  |  4|  1|  5|  3|  1|  2|  8|  5|  6|   4|   5|   8|  0|
|3  |  5|  9|  4|  1|  0|  3|  7|  5|  0|   5|   1|   2|  2|
|4  |  0|  8|  1|  4|  7|  0|  2|  9|  2|   1|   6|   9|  2|
|5  |  2|  5|  3|  9|  0|  6|  5|  5|  2|   8|   1|   7|  1|
|6  |  0|  0|  6|  5|  8|  9|  1|  5|  6|   9|   5|   4|  2|
|7  |  6|  2|  0|  4|  0|  5|  0|  0|  1|   4|   6|   0|  5|
|8  |  3|  6|  6|  7|  1|  7|  8|  2|  8|   5|   8|   4|  0|
|9  |  2|  1|  6|  3|  3|  1|  8|  1|  2|   2|   0|   9|  1|
|10 |  6|  4|  3|  0|  2|  6|  0|  5|  6|   9|   7|   5|  2|
|11 |  0|  3|  1|  5|  4|  9|  9|  2|  3|   5|   1|   8|  1|
|12 |  4|  2|  1|  4|  9|  3|  2|  7|  3|   8|   0|   6|  1|
|13 |  3|  7|  9|  7|  6|  8|  8|  9|  8|   2|   8|   2|  0|
|14 |  0|  6|  4|  8|  3|  5|  7|  6|  2|   7|   2|   7|  1|
|15 |  7|  1|  6|  1|  9|  0|  4|  9|  1|   8|   8|   0|  2|
|16 |  6|  3|  0|  8|  8|  9|  3|  5|  1|   9|   1|   9|  1|
|17 |  9|  1|  4|  2|  3|  1|  4|  4|  5|   2|   0|   7|  1|
|18 |  7|  5|  7|  2|  1|  7|  6|  9|  4|   7|   9|   4|  0|
|19 |  2|  3|  0|  8|  1|  6|  2|  9|  7|   2|   0|   0|  3|
|20 |  8|  9|  9|  4|  1|  9|  7|  1|  3|   4|   6|   3|  0|
|21 |  3|  3|  2|  4|  5|  4|  2|  3|  8|   4|   5|   6|  0|
|22 |  6|  9|  5|  4|  7|  1|  9|  7|  1|   5|   5|   0|  1|
|23 |  4|  4|  5|  8|  0|  0|  8|  3|  1|   7|   6|   6|  2|
|24 |  9|  6|  5|  4|  9|  8|  5|  7|  2|   9|   9|   1|  0|
|25 |  9|  6|  6|  3|  6|  9|  1|  8|  9|   5|   9|   1|  0|

Each value in the last column of \@ref(tab:veh-numbers-counts) is the count of
zeros in that row, and therefore, the result from our simulation of one day.

We can estimate how often three or more ambulances would break down by looking for
values of three or greater in the last column.  We find there are
2 rows with three or more in the last column.
Finally we divide this number of rows by the number of trials (25)
to get an estimate of the *proportion* of days with three or more breakdowns.
The result is 0.08.

## How resampling differs from the conventional approach

In the standard approach the student learns to choose and solve a
formula. Doing the algebra and arithmetic is quick and easy. The
difficulty is in choosing the correct formula. Unless you are a
professional mathematician, it may take you quite a while to arrive at
the correct formula---considerable hard thinking, and perhaps some
digging in textbooks. More important than the labor, however, is that
you may come up with the wrong formula, and hence obtain the wrong
answer. Most students who have had a standard course in probability and
statistics are quick to tell you that it is not easy to find the correct
formula, even immediately after finishing a course (or several courses)
on the subject. After leaving school, it is harder still to choose the
right formula. Even many people who have taught statistics at the
university level (including this writer) must look at a book to get the
correct formula for a problem as simple as the ambulances, and then we are
not always sure of the right answer. This is the grave disadvantage of
the standard approach.

In the past few decades, resampling and other Monte Carlo simulation
methods have come to be used extensively in scientific research. But in
contrast to the material in this book, simulation has mostly been used
in situations so complex that mathematical methods have not yet been
developed to handle them. Here are examples of such situations:

<!---
Better examples.  These are out of date.
-->

1.  For a spaceship that will travel to Mars, calculating the correct flight
    route involves a great many variables, too many to solve with formulas.
    Hence, the Monte Carlo simulation method is used.

2.  The Navy might want to know how long the average ship will have to
    wait for dock facilities. The time of completion varies from ship to
    ship, and the number of ships waiting in line for dock work varies
    over time. This problem can be handled quite easily with the
    experimental simulation method, but formal mathematical analysis
    would be difficult or impossible.

3.  What are the best tactics in baseball? Should one bunt? Should one
    put the best hitter up first, or later? By trying out various
    tactics with dice or random numbers, Earnshaw Cook (in his book
    *Percentage Baseball* ), found that it is best never to bunt, and
    the highest-average hitter should be put up first, in contrast to
    usual practice. Finding this answer would have been much more difficult
    with the analytic method.

4.  Which search pattern will yield the best results for a ship
    searching for a school of fish? Trying out "models" of various
    search patterns with simulation can provide a fast answer.

5.  What strategy in the game of Monopoly will be most likely to win?
    The simulation method systematically plays many games (with a
    computer) testing various strategies to find the best one.

But those five examples are all complex problems. This book and its
earlier editions break new ground by using this method for *simple
rather than complex problems* , especially in statistics rather than
pure probability, and in teaching *beginning rather than advanced*
students to solve problems this way. (Here it is necessary to emphasize
that the resampling method is used to *solve the problems themselves
rather than as a demonstration device to teach the notions found in the
standard conventional approach* . Simulation has been used in elementary
courses in the past, but only to demonstrate the operation of the
analytical mathematical ideas. That is very different than using the
resampling approach to solve statistics problems themselves, as is done
here.)

Once we get rid of the formulas and tables, we can see that statistics
is a matter of *clear thinking, not fancy mathematics* . Then we can get
down to the business of learning how to do that clear statistical
thinking, and putting it to work for you. *The study of probability* is
purely mathematics (though not necessarily formulas) and technique. But
*statistics has to do with meaning* . For example, what is the meaning
of data showing an association just discovered between a type of
behavior and a disease? Of differences in the pay of men and women in
your firm? Issues of causation, acceptability of control, design of
experiments cannot be reduced to technique. This is "philosophy" in the
fullest sense. Probability and statistics calculations are just one
input. Resampling simulation enables us to get past issues of
mathematical technique and focus on the crucial statistical elements of
statistical problems.

If you intend to go on to advanced statistical work, the older standard
method can be learned alongside resampling methods. Your introduction to
the conventional method may thereby be made much more meaningful.

## Resampling can make logic clearer.

A problem in probability is at the heart of every problem in statistics.
Problems in probability often can be very confusing to the intuition.
And formulas often either do not aid the intuition, or lead to the wrong
answer, or both. But simulation can often get you the right answer and
also help you understand why it is correct. Let's dramatize the point
with puzzles:

### The other child

Problem: **If a family has two children and one of them is a boy, what is the
probability that the other will also be a boy? **

Most people---even professional statisticians---often quickly answer
"one half." That is not correct.

The solution is not obvious even after the person has decided to tackle
the problem by simulation. It is not unusual for a person to say: I flip
one coin, and if it is a head (boy), I then flip another coin and see
how often that will be a boy, also, and then actually flip the coin once
and record the outcomes. But that experiment does not resemble the
situation of interest. A proper modeling throws *two* coins, examines to
see if there is a head on *either* , and then examines the *other*.

Or consider table \@ref(tab:otherboy), where we asked the computer to do this
work for us.  The computer has done 50 trials, where one trial is one family of
two children.  It decided the gender of the two children, by choosing at random
from "Boy" or "Girl", and then classified the pair of children as to whether
the other child was a boy. Where both children were girls, it leaves the
classification blank.

The first 50 lines are enough to suggest the correct probability, and
also to make clear the mechanism: Two-girl pairs, a fourth of the cases, are
excluded from the sample. And mixed pairs---which give a "No" answer---are
two-thirds of the remaining pairs, whereas the only pairs that give "Yes"
answers---two boys---are only a third of the remaining pairs. The point of
presenting the puzzle is this: Simulation gets it right very quickly and
easily, whereas the deductive method of mathematical logic can result in much
confusion.





<table class="kable_wrapper">
<caption>(\#tab:otherboy)50 simulations of a family with two children</caption>
<tbody>
  <tr>
   <td> 

| Trial|First |Second |Other is boy? |
|-----:|:-----|:------|:-------------|
|     1|Boy   |Boy    |Yes           |
|     2|Girl  |Girl   |              |
|     3|Boy   |Girl   |No            |
|     4|Girl  |Boy    |No            |
|     5|Girl  |Girl   |              |
|     6|Boy   |Girl   |No            |
|     7|Boy   |Girl   |No            |
|     8|Girl  |Girl   |              |
|     9|Girl  |Girl   |              |
|    10|Girl  |Boy    |No            |
|    11|Girl  |Girl   |              |
|    12|Boy   |Boy    |Yes           |
|    13|Boy   |Boy    |Yes           |
|    14|Boy   |Boy    |Yes           |
|    15|Boy   |Boy    |Yes           |
|    16|Girl  |Boy    |No            |
|    17|Boy   |Boy    |Yes           |
|    18|Boy   |Girl   |No            |
|    19|Boy   |Boy    |Yes           |
|    20|Girl  |Boy    |No            |
|    21|Boy   |Boy    |Yes           |
|    22|Boy   |Boy    |Yes           |
|    23|Girl  |Boy    |No            |
|    24|Girl  |Boy    |No            |
|    25|Girl  |Boy    |No            |

 </td>
   <td> 

| Trial|First |Second |Other is boy? |
|-----:|:-----|:------|:-------------|
|    26|Boy   |Boy    |Yes           |
|    27|Girl  |Boy    |No            |
|    28|Boy   |Boy    |Yes           |
|    29|Boy   |Girl   |No            |
|    30|Boy   |Boy    |Yes           |
|    31|Boy   |Girl   |No            |
|    32|Boy   |Girl   |No            |
|    33|Boy   |Girl   |No            |
|    34|Boy   |Boy    |Yes           |
|    35|Boy   |Boy    |Yes           |
|    36|Boy   |Girl   |No            |
|    37|Boy   |Girl   |No            |
|    38|Boy   |Boy    |Yes           |
|    39|Girl  |Girl   |              |
|    40|Girl  |Boy    |No            |
|    41|Girl  |Girl   |              |
|    42|Girl  |Girl   |              |
|    43|Boy   |Boy    |Yes           |
|    44|Boy   |Girl   |No            |
|    45|Boy   |Boy    |Yes           |
|    46|Boy   |Boy    |Yes           |
|    47|Boy   |Girl   |No            |
|    48|Girl  |Boy    |No            |
|    49|Boy   |Boy    |Yes           |
|    50|Boy   |Boy    |Yes           |

 </td>
  </tr>
</tbody>
</table>

This puzzle illustrates the power of simulation. And it supports by
analogy the general use of the resampling method in probability and
statistics because it reveals the limitations of human deductive
capacity, even among those who are mathematically adept.

Someone might wonder whether formal mathematics can help us with this
problem. Formal (even though not formulaic) analysis can certainly
provide an answer. We can use what is known as the "sample space"
approach which reasons from first principles; here it consists of making
a *list of the possibilities* , and examining the proportion of
"successes" to "failures" in that list.

First we write down the equally-likely ways that two coins can fall:

|   | First coin | Second coin |
| - | ----- | ------ |
| 1 | Heads |  Heads |
| 2 | Heads |  Tails |
| 3 | Tails |  Heads |
| 4 | Tails |  Tails |

Note that it is very easy to make a mistake in writing this list;
great mathematicians have made such mistakes in the past even with
problems as easy as this one (as we will see with the example of
D'Alembert in just a minute).

Now we notice that if we have observed at least one head, the list of
possibilities for the ways the two coins fell shrinks to:

|   | First coin | Second coin |
| - | ----- | ------ |
| 1 | Heads |  Heads |
| 2 | Heads |  Tails |
| 3 | Tails |  Heads |

And now we can see that in only one of three of these possibilities would the
"other" coin be a head. So the probability we ask about is 1/3.

The crucial question is: Does this formal approach make the problem
harder or easier than the simulation approach? That depends on your
mental makeup. In practice, it turns out that the formal method seems to
lead to a higher rate of error if *everyone* employs it than does the
simulation approach. Hence we emphasize the simulation approach here, to
lead to the highest rate of success (and enjoyment). But it is best if
everyone finds the mode that is best for him or her.

Here's a puzzle I call D'Alembert's Misery. Great mathematicians have
blundered on even simple problems in probability because they attacked
them with only logical tools. One famous example was D'Alembert, living
in the 18th Century (1717-1783; story in @schuh1968master, p 165).

D'Alembert asked: What is the chance of throwing at least one head in
two tosses of a coin? He reasoned that there are three cases: tail on
both tosses, tail then head, head on the first toss (no second toss
necessary). Of these three cases, two are successes, and therefore the
probability sought is 2/3, he concluded.

What's the answer?

1.  Toss two coins, and record number of heads.

2.  Repeat (1) a hundred times.

3.  Count number of outcomes with one or two heads (or more easily, the
    number with two tails), and divide by 100 to find the probability
    sought.

Compare D'Alembert's conclusion to the result of your Monte Carlo
experiment performed as above. Some other fascinating puzzles in
probability are found in Chapter XXX.

### The Monty Hall problem

The Monty Hall Problem is a probability problem that is famous for its deceptive simplicity.  It has its own long Wikipedia page:
<https://en.wikipedia.org/wiki/Monty_Hall_problem>.

Here is the problem in its most famous form; a letter to the columnist Marilyn
vos Savant, published in Parade Magazine [-@savant1990monty]:

> Suppose you’re on a game show, and you’re given the choice of three doors.
> Behind one door is a car, behind the others, goats. You pick a door, say #1,
> and the host, who knows what’s behind the doors, opens another door, say #3,
> which has a goat. He says to you, "Do you want to pick door #2?" Is it to
> your advantage to switch your choice of doors?

In fact the first person to propose (and solve) this problem was Steve Selvin,
a professor of public health at the University of California, Berkeley
[@slevin1975monty].

Most people, including at least one of us, your humble authors, quickly come to
the wrong conclusion.  The most common but incorrect answer is that it will
make no difference if you switch doors or stay with your original choice.  The
obvious intuition is that, after Monty opens his door, there are two doors that
might have the car behind them, and therefore, a 50% chance it will be behind
any one of the two. It turns out that answer is wrong; you will double your
chances of winning by switching doors. Did you get the answer right?

If you got the answer wrong, you are in excellent company.  As you can see
from the commentary in @savant1990monty, many mathematicians wrote to Parade
magazine to assert that the (correct) solution was wrong.  Paul Erdős was one
of the most famous mathematicians of the 20th century; he could not be
convinced of the correct solution until he had seen a computer simulation
[@vazsonyi1999door], of the type we will do below.

To simulate a trial of this problem, we need to select a door at random to
house the car, and another door at random, to be the door the contestant
chooses.  We number the doors 1, 2 and 3.   Now we need two random choices from
the options 1, 2 or 3, one for the door with the car, the other for the
contestant door.  To chose a door for the car, we could throw a die, and chose
door 1 if the die shows 1 or 4, door 2 if the die shows 2 or 5, and door 3 for
3 or 6.  Then we throw the die again to chose the contestant door.

But throwing dice is a little boring; we have to find the die, then throw it many times, and record the results.   Instead we can ask the computer to chose the doors at random.

For this simulation, let us do 25 trials.  We ask the computer to create two
sets of 25 random numbers from 1 through 3. The first set is the door with the
car behind it ("Car door").  The second set have the door that the contestant
chose at random ("Our door").   We put these in a table, and make some new,
empty columns to fill in later.  The first new column is "Monty opens".  In due
course, we will use this column to record the door that Monty Hall will open on
this trial.  The last two columns express the outcome.  The first is "Stay
wins".  This has "Yes" if we win on this trial by sticking to our original
choice of door, and "No" otherwise.  The last column is "Switch wins". This has
"Yes" if we win by switching doors, and "No" otherwise. See table
\@ref(tab:montyblank).




Table: (\#tab:montyblank)25 simulations of the Monty Hall problem

|   | Car door| Our door|Monty opens |Stay wins |Switch wins |
|:--|--------:|--------:|:-----------|:---------|:-----------|
|1  |        3|        3|            |          |            |
|2  |        3|        1|            |          |            |
|3  |        2|        3|            |          |            |
|4  |        1|        1|            |          |            |
|5  |        3|        3|            |          |            |
|6  |        3|        2|            |          |            |
|7  |        1|        3|            |          |            |
|8  |        2|        3|            |          |            |
|9  |        1|        2|            |          |            |
|10 |        1|        3|            |          |            |
|11 |        2|        3|            |          |            |
|12 |        1|        1|            |          |            |
|13 |        1|        2|            |          |            |
|14 |        1|        1|            |          |            |
|15 |        3|        3|            |          |            |
|16 |        2|        2|            |          |            |
|17 |        3|        2|            |          |            |
|18 |        3|        1|            |          |            |
|19 |        3|        3|            |          |            |
|20 |        1|        2|            |          |            |
|21 |        1|        3|            |          |            |
|22 |        3|        1|            |          |            |
|23 |        2|        3|            |          |            |
|24 |        3|        3|            |          |            |
|25 |        3|        1|            |          |            |





In the first trial in \@ref(tab:montyblank), the computer selected door 3 for
car, and door 3 for the contestant.  Now Monty must open a door, and he cannot
open our door (door 3) so he has the choice of opening door 1 or door 2; he
chooses randomly, and opens door 2.  On this trial, we win if we stay with our
original choice, and we lose if we change to the remaining door, door 1.

Now we go the second trial.  The computer chose door 3 for the car, and door 1 for our choice.  Monty cannot choose our door (door 1) or the door with the car behind it (door 3), so he must open door 2.   Now if we stay with our original choice, we lose, but if we switch, we win.

You may want to print out table \@ref(tab:montyblank), and fill out the blank
columns, to work through the logic.

After doing a few more trials, and some reflection, you may see that there are
two different situations here: the situation when our *initial guess was
right*, and the situation where our *initial guess was wrong*.   When our
initial guess was right, we win by staying with our original choice, but when
it was wrong, we always win by switching.   The chance of our *initial guess*
being correct is 1/3 (one door out of three).  So the chances of winning by
staying are 1/3, and the chances of winning by switching are 2/3.  But
remember, you don't need to follow this logic to get the right answer.  As you
will see below, the resampling simulation shows us that the Switch strategy
wins.

Table \@ref(tab:montyfull) is a version of table \@ref(tab:montyblank) for
which we have filled in the blank columns using the logic above.


Table: (\#tab:montyfull)25 simulations of the Monty Hall problem, filled out

|   | Car door| Our door| Monty opens|Stay wins |Switch wins |
|:--|--------:|--------:|-----------:|:---------|:-----------|
|1  |        3|        3|           2|Yes       |No          |
|2  |        3|        1|           2|No        |Yes         |
|3  |        2|        3|           1|No        |Yes         |
|4  |        1|        1|           2|Yes       |No          |
|5  |        3|        3|           1|Yes       |No          |
|6  |        3|        2|           1|No        |Yes         |
|7  |        1|        3|           2|No        |Yes         |
|8  |        2|        3|           1|No        |Yes         |
|9  |        1|        2|           3|No        |Yes         |
|10 |        1|        3|           2|No        |Yes         |
|11 |        2|        3|           1|No        |Yes         |
|12 |        1|        1|           2|Yes       |No          |
|13 |        1|        2|           3|No        |Yes         |
|14 |        1|        1|           2|Yes       |No          |
|15 |        3|        3|           2|Yes       |No          |
|16 |        2|        2|           1|Yes       |No          |
|17 |        3|        2|           1|No        |Yes         |
|18 |        3|        1|           2|No        |Yes         |
|19 |        3|        3|           1|Yes       |No          |
|20 |        1|        2|           3|No        |Yes         |
|21 |        1|        3|           2|No        |Yes         |
|22 |        3|        1|           2|No        |Yes         |
|23 |        2|        3|           1|No        |Yes         |
|24 |        3|        3|           2|Yes       |No          |
|25 |        3|        1|           2|No        |Yes         |

The proportion of times "Stay" wins in these 25 trials is
0.36.
The proportion of times "Switch" wins is
0.64; the Switch strategy wins about twice as often as the Stay strategy.

Doing these simulations has two large benefits.   First, it gives us the right answer, saving us from making a mistake.  Second, the process of simulation forces us to think about how the problem works.  This can give us better understanding, and make it easier to reason about the solution.

We will soon see that these same advantages also apply to reasoning about
statistics.
