Download Link: https://assignmentchef.com/product/solved-data606-lab-5a
<br>
In this lab, you will investigate the ways in which the statistics from a random sample of data can serve as point estimates for population parameters. We’re interested in formulating a <em>sampling distribution </em>of our estimate in order to learn about the properties of the estimate, such as its distribution.

<strong>Setting a seed: </strong>We will take some random samples and build sampling distributions in this lab, which means you should set a seed at the start of your lab. If this concept is new to you, review the lab on probability.

<h1><strong>Getting Started</strong></h1>

<strong>Load packages</strong>

In this lab, we will explore and visualize the data using the <strong>tidyverse </strong>suite of packages. We will also use the <strong>infer </strong>package for resampling.

Let’s load the packages.

library(tidyverse) library(openintro) library(infer)

<strong>The data</strong>

A 2019 Gallup report states the following:

The premise that scientific progress benefits people has been embodied in discoveries throughout the ages – from the development of vaccinations to the explosion of technology in the past few decades, resulting in billions of supercomputers now resting in the hands and pockets of people worldwide. Still, not everyone around the world feels science benefits them personally.

<strong>Source: </strong><a href="https://news.gallup.com/opinion/gallup/268121/world-science-day-knowledge-power.aspx">World Science Day: Is Knowledge Power?</a>

The Wellcome Global Monitor finds that 20% of people globally do not believe that the work scientists do benefits people like them. In this lab, you will assume this 20% is a true population proportion and learn about how sample proportions can vary from sample to sample by taking smaller samples from the population. We will first create our population assuming a population size of 100,000. This means 20,000 (20%) of the population think the work scientists do does not benefit them personally and the remaining 80,000 think it does.

global_monitor &lt;- tibble(

scientist_work = c(rep(“Benefits”, 80000), rep(“Doesn’t benefit”, 20000))

)

The name of the data frame is global_monitor and the name of the variable that contains responses to the question <em>“Do you believe that the work scientists do benefit people like you?” </em>is scientist_work.

We can quickly visualize the distribution of these responses using a bar plot.

<table width="632">

 <tbody>

  <tr>

   <td width="632">ggplot(global_monitor, aes(x = scientist_work)) + geom_bar() + labs(x = “”, y = “”,title = “Do you believe that the work scientists do benefit people like you?”) +coord_flip()</td>

  </tr>

 </tbody>

</table>

Do you believe that the work scientists do benefit people like you?

<table width="549">

 <tbody>

  <tr>

   <td rowspan="2" width="27"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td rowspan="2" width="62"> </td>

   <td rowspan="2" width="62"> </td>

   <td rowspan="2" width="62"> </td>

   <td rowspan="2" width="62"> </td>

   <td rowspan="2" width="62"> </td>

   <td rowspan="2" width="62"> </td>

   <td rowspan="2" width="25"> </td>

  </tr>

  <tr>

   <td width="62"> </td>

   <td width="62"> </td>

  </tr>

  <tr>

   <td rowspan="3" width="27"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td rowspan="2" width="62"> </td>

   <td rowspan="2" width="62"> </td>

   <td rowspan="2" width="62"> </td>

   <td rowspan="2" width="62"> </td>

   <td rowspan="2" width="62"> </td>

   <td rowspan="2" width="62"> </td>

   <td rowspan="3" width="25"> </td>

  </tr>

  <tr>

   <td width="62"> </td>

   <td width="62"> </td>

  </tr>

  <tr>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

  </tr>

  <tr>

   <td rowspan="2" width="27"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td rowspan="2" width="25"> </td>

  </tr>

  <tr>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

   <td width="62"> </td>

  </tr>

 </tbody>

</table>

Doesn’t benefit

Benefits

0                                              20000                                          40000                                           60000                    80000

We can also obtain summary statistics to confirm we constructed the data frame correctly.

global_monitor %&gt;%

count(scientist_work) %&gt;% mutate(p = n /sum(n))

## # A tibble: 2 x 3

##           scientist_work     n p ## * &lt;chr&gt;         &lt;int&gt; &lt;dbl&gt;

## 1 Benefits                        80000       0.8

## 2 Doesn’t benefit 20000                  0.2

<h1><strong>The unknown sampling distribution</strong></h1>

In this lab, you have access to the entire population, but this is rarely the case in real life. Gathering information on an entire population is often extremely costly or impossible. Because of this, we often take a sample of the population and use that to understand the properties of the population.

If you are interested in estimating the proportion of people who don’t think the work scientists do benefits them, you can use the sample_n command to survey the population.

samp1 &lt;- global_monitor %&gt;%

sample_n(50)

This command collects a simple random sample of size 50 from the global_monitor dataset, and assigns the result to samp1. This is similar to randomly drawing names from a hat that contains the names of all in the population. Working with these 50 names is considerably simpler than working with all 100,000 people in the population.

<ol>

 <li>Describe the distribution of responses in this sample. How does it compare to the distribution of responses in the population. <strong>Hint: </strong>Although the sample_n function takes a random sample of observations (i.e. rows) from the dataset, you can still refer to the variables in the dataset with the same names. Code you presented earlier for visualizing and summarizing the population data will still be useful for the sample, however be careful to not label your proportion p since you’re now calculating a sample statistic, not a population parameters. You can customize the label of the statistics to indicate that it comes from the sample.</li>

</ol>

The sampel distribution is very close to the population distribution with 0.82 in the sample believing in science and 0.18 not believing in science.

If you’re interested in estimating the proportion of all people who do not believe that the work scientists do benefits them, but you do not have access to the population data, your best single guess is the sample mean.

samp1 %&gt;%

count(scientist_work) %&gt;% mutate(p_hat = n /sum(n))

## # A tibble: 2 x 3

##           scientist_work     n p_hat ## * &lt;chr&gt; &lt;int&gt; &lt;dbl&gt; ## 1 Benefits        37 0.74

## 2 Doesn’t benefit                     13 0.26

Depending on which 50 people you selected, your estimate could be a bit above or a bit below the true population proportion of 0.26. In general, though, the sample proportion turns out to be a pretty good estimate of the true population proportion, and you were able to get it by sampling less than 1% of the population.

<ol start="2">

 <li>Would you expect the sample proportion to match the sample proportion of another student’s sample? Why, or why not? If the answer is no, would you expect the proportions to be somewhat different or very different? Ask a student team to confirm your answer.</li>

 <li>Take a second sample, also of size 50, and call it samp2. How does the sample proportion of samp2 compare with that of samp1? Suppose we took two more samples, one of size 100 and one of size 1000. Which would you think would provide a more accurate estimate of the population proportion?</li>

 <li>How many elements are there in sample_props50? Describe the sampling distribution, and be sure to specifically note its center. Make sure to include a plot of the distribution in your answer.</li>

</ol>

<h1><strong>Interlude: Sampling distributions</strong></h1>

The idea behind the rep_sample_n function is <em>repetition</em>. Earlier, you took a single sample of size n (50) from the population of all people in the population. With this new function, you can repeat this sampling procedure rep times in order to build a distribution of a series of sample statistics, which is called the <strong>sampling distribution</strong>.

Note that in practice one rarely gets to build true sampling distributions, because one rarely has access to data from the entire population.

Without the rep_sample_n function, this would be painful. We would have to manually run the following code 15,000 times

<table width="632">

 <tbody>

  <tr>

   <td width="632">global_monitor %&gt;%sample_n(size = 50, replace = TRUE) %&gt;% count(scientist_work) %&gt;% mutate(p_hat = n /sum(n)) %&gt;%filter(scientist_work == “Doesn’t benefit”)</td>

  </tr>

 </tbody>

</table>

## # A tibble: 1 x 3

##           scientist_work     n p_hat ## &lt;chr&gt;     &lt;int&gt; &lt;dbl&gt;

## 1 Doesn’t benefit                       6 0.12

as well as store the resulting sample proportions each time in a separate vector.

Note that for each of the 15,000 times we computed a proportion, we did so from a <strong>different </strong>sample!

<ol start="5">

 <li>To make sure you understand how sampling distributions are built, and exactly what the rep_sample_n function does, try modifying the code to create a sampling distribution of <strong>25 sample proportions </strong>from <strong>samples of size 10</strong>, and put them in a data frame named sample_props_small. Print the output. How many observations are there in this object called sample_props_small? What does each observation represent?</li>

</ol>

<h2>Sample size and the sampling distribution</h2>

Mechanics aside, let’s return to the reason we used the rep_sample_n function: to compute a sampling distribution, specifically, the sampling distribution of the proportions from samples of 50 people.

ggplot(data = sample_props50, aes(x = p_hat))


geom_histogram(binwidth = 0.02)

The sampling distribution that you computed tells you much about estimating the true proportion of people who think that the work scientists do doesn’t benefit them. Because the sample proportion is an unbiased estimator, the sampling distribution is centered at the true population proportion, and the spread of the distribution indicates how much variability is incurred by sampling only 50 people at a time from the population.

In the remainder of this section, you will work on getting a sense of the effect that sample size has on your sampling distribution.

<ol start="6">

 <li>Use the app below to create sampling distributions of proportions of <em>Doesn’t benefit </em>from samples of size 10, 50, and 100. Use 5,000 simulations. What does each observation in the sampling distribution represent? How does the mean, standard error, and shape of the sampling distribution change as the sample size increases? How (if at all) do these values change if you increase the number of simulations?</li>

</ol>

(You do not need to include plots in your answer.)

<h2>More Practice</h2>

So far, you have only focused on estimating the proportion of those you think the work scientists doesn’t benefit them. Now, you’ll try to estimate the proportion of those who think it does.

Note that while you might be able to answer some of these questions using the app, you are expected to write the required code and produce the necessary plots and summary statistics. You are welcome to use the app for exploration.

<ol start="7">

 <li>Take a sample of size 15 from the population and calculate the proportion of people in this sample who think the work scientists do enhances their lives. Using this sample, what is your best point estimate of the population proportion of people who think the work scientists do enhances their lives?</li>

 <li>Since you have access to the population, simulate the sampling distribution of proportion of those who think the work scientists do enchances their lives for samples of size 15 by taking 2000 samples from the population of size 15 and computing 2000 sample proportions. Store these proportions in as sample_props15. Plot the data, then describe the shape of this sampling distribution. Based on this sampling distribution, what would you guess the true proportion of those who think the work scientists do enchances their lives to be? Finally, calculate and report the population proportion.</li>

 <li>Change your sample size from 15 to 150, then compute the sampling distribution using the same method as above, and store these proportions in a new object called sample_props150. Describe the shape of this sampling distribution and compare it to the sampling distribution for a sample size of 15. Based on this sampling distribution, what would you guess to be the true proportion of those who think the work scientists do enchances their lives?</li>

 <li>Of the sampling distributions from 2 and 3, which has a smaller spread? If you’re concerned with making estimates that are more often close to the true value, would you prefer a sampling distribution with a large or small spread?</li>

</ol>