# Resampling with code

Chapter \@ref(resampling-method) used simulation and resampling from
tables of random numbers, dice and coins.  Making random choices in this way
can make it easier to understand the process, but of course, physical methods
of making random outcomes can be slow and boring.

On the other hand, short computer programs can do a huge number of resampling
trials in a few seconds.  The flexibility of a programming language makes it
possible to simulate many different outcomes and tests.

In this chapter, we begin using the R language to build up tables of
random numbers, and do simple tasks like counting the number of values in
a row, and taking proportions.

With these simple tools, we can simulate many problems in probability and
statistics.

Here is our first R example program.  Do not expect to follow all
of it straightaway.  For now, read the code below to get an idea of how it
implements the procedure above.  We will be coming back to the specifics later.

The key to reading code is to think about what the computer will do, when it sees the code.

<div class="rmdcomment">

<p>Start of <code>resampling_with_code</code> notebook.</p>

<p>
<div class="nb-links">
<a class="notebook-link" href=resampling_with_code.Rmd>Download notebook</a>

</p>
</div>
</div>


A simple place to start is the *comment*.  A comment is a statement that the
computer will *ignore*.   It is text that we put in the program for our
benefit, to explain what is going on to a human reader.

This is an example of a comment:




```r
# This is a comment. It doesn't have any effect.
```

The comment starts with a hash character `#`.  This character tells the
computer that the rest of the line is a comment, and therefore, that it can
ignore everything on that line that follows the `#`.

<!---

In the next bit of code, we get the *function* we use to get random numbers.
The code has four lines.  The first and third are comments; the computer
ignores them. The second line gets a library we will use in nearly all our
examples, and the last line gets the function to generate random numbers.
Don't worry about this code for now, we will come back to it later.  For now,
assume that, after we run this code, we have a function called `randint`, and
a library called `np`.



-->

The core of the program to solve the ambulances problem above begins with this
command to the computer:

<!---



This uses the `randint` *function* to generate random integers (counting numbers) from 1 up to, *but not including* 11.  Therefore, this command will generate random numbers from 1 through 10.  The 12 in the command tells the function to generate 12 of these numbers.

-->




```r
a = sample(1:10, 12, replace=TRUE)
```

This uses the `sample` function to generate random integers (counting numbers)
from 1 through 10.  The 12 in the command tells R to generate 12 of these
numbers.  `replace=TRUE` tells R to sample *with replacement*.

For example, the chances of getting a particular result - such as a 3 - for the
first random number - are 1 in 10, or p= 1/10 = 0.1.  If we resample *with
replacement* then the chances of getting 3 in the second number are unchanged,
at p=0.1.  It is as if we put 10 balls into a bucket, numbered one through ten,
and then selected 12 balls from the bucket; but after we have selected a ball,
we record the number and *replace* it in the bucket, and shake up the bucket
again.



So --- the command above orders the computer to randomly generate 12 numbers
between "1" and "10." Inasmuch as each ambulance has a 1 in 10 chance of being
defective, we decide arbitrarily that a "1" stands for a defective ambulance,
and the other nine numbers (from "2" to "10") stand for a not-defective
ambulance. The command orders the computer to store the results of the random
drawing in a location in the computer's memory to which we give a name such as
`a` or `ambulances`.  When we run a statement like the one above, `a` is
a *variable* - the *name* `a` refers to the *value*, which is the sequence of
random numbers the computer created using `sample`.

We can show the value of the variable `a` in the notebook or interactive terminal by using the `print` function:




```r
print(a)
#>  [1] 7 5 6 4 6 9 5 5 8 2 7 5
```

This shows the 12 random values that we got from
`sample`.


The next key element in the core of the program is:




```r
b = sum(a == 1)
```

This command orders the computer to count the number of "1's" among the
12 numbers that are in location `a` following the random drawing
carried out by the
`sample`
operation. The result of the count will be somewhere between 0 and 12, the
number of ambulances that might be out-of-order on a given day. The result is
then placed in another location in the computer's memory that we label `b`.




```r
# Show the value of b
print(b)
#> [1] 0
```

Now let us place the commands to generate the random numbers, and count how
many 1s we get, within the entire program that we use to solve this problem,
which is:




```r
# Make an array that has 400 elements.
# We will use this to store the counts for our 400 repetitions
results <- numeric(400)

# Repeat the simulation 400 times
for (i in 1:400) {

    # The commands between the { above and the } below are the procedure for
    # one trial.
    # The computer runs these commands from first to last, for each trial.

    # Generate 12 numbers, each between "1" and "10," and put them in vector a.
    # Each number will represent an ambulance, and we let 1 represent
    # a defective ambulance.
    a <- sample(1:10, 12, replace=TRUE)

    # Count the number of defective ambulances, and put the result in b.
    b <- sum(a == 1)

    # Keep track of each trial's result in "results".
    results[i] <- b

    # End this trial, then go back and repeat the process until all 400 trials
    # are complete.
}

# Now we have finished the 400 trials.

# Determine how many trials resulted in more than 3 ambulances out of order.
bad_day_count <- sum(results > 3)

# Convert to a proportion.
bad_day_prop <- bad_day_count / 400

# Print the result.
print(bad_day_prop)
#> [1] 0.0275
```

<div class="rmdcomment">

<p>End of <code>resampling_with_code</code> notebook.</p>

</div>


The
`results[i] <- b`
statement that follows the
`b = sum(a == 1)`
operation simply keeps track of the results of each trial, placing the number
of defective ambulances that occur in each trial in a location that we usually
call "results". This is done in each of the 400 trials that we make, and the
result eventually is a "vector" with 400 numbers in it.  A vector is just
a sequence of numbers.

In order to make 400 repetitions of our experiment---we could have
decided to make a thousand or some other number of repetitions---we put
`for (i in 1:400) {`
before the statements that generate the random numbers, count how many of these numbers are 1, representing a defective ambulance, and store this result
of a single trial. Then we complete each repetition "loop" by <!---
bringing the indentation of the statements back to the left margin
--> 
adding a closing `}` bracket.


Since our aim is to count the number of days in which more than 3 (4 or more)
defective ambulances occur, we use the
`bad_day_count = sum(results > 3)`
command to count how many times in the 400 days recorded in our results vector
at the end of the 400 trials more than 3 defects occurred, and we place the
result in still another location "bad_day_count".

This gives us the total number of days where 4 or more defective ambulances are
seen to occur. Then we divide the number in "bad_day_count" by 400, the number
of trials. Thus we obtain an estimate of the chance, expressed as a probability
between 0 and 1, that 4 or more ambulances will be defective on a given day.
And we store that result in a location that we decide to call "bad_day_prop" so
that it will be there when the computer receives the next command to `print`
that result on the screen.

Can you see how each of the operations that the computer carries out are
analogous to the operations that you yourself executed when you solved this
problem using a labeled coins or a random-number table? This is exactly the
procedure that we will use to solve every problem in probability and statistics
that we must deal with. Either we will use a device such as coins or a random
number table as an analogy for the physical process we are interested in
(ambulances becoming defective, in this case), or we will simulate the analogy
on the computer using the R program above.

Simple as it is, the program above may not seem simple to you at first glance.
But we think you will find, over the course of this book, that these programs
become much simpler to understand than the older conventional approach to such
problems that has routinely been taught to students for decades.
