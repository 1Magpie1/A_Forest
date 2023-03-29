---
title: "Bayes in R"
tags:
- Guide
- Bayesian Analysis
- R
---

First thing first! Make sure you have the package `brms` installed and loaded. This walkthrough also uses the `tidyverse` package, which is not strictly necessary. 

```{r}
install.packages("brms") # this is a fairly large package and may take some time to download.
install.packages("tidyverse")

library("brms")
library("tidyverse")
```

Next, we need to load our data. I simulated this data based on the following design. The data can be found at [this link].

> In the first part of this study, 20 people (referred to as Senders) were videotaped, making one true statement and one false statement. This resulted in 20 videos of true statements and 20 videos of lies. Each Sender filled out a survey on their interpersonal warmth. 
> 
> In the second part, participants were asked to watch six videos from the last part of the study. Unbeknownst to the participant, half were lies, and half were true statements. After each video, the participants rate how much they want to spend time with the Sender. After watching all six videos, participants filled out an emotional intelligence questionnaire. 
>
> Likability, warmth, and emotional intelligence are measured on a scale of 1 - 7 (midpoint at 4.)

```{r}
Lie.Data <- read_csv("https://magpie-roost.github.io/Pine-Roost/notes/BayesianLearn.csv")
head(Lie.Data)
```

## Analysis Map

Before we start, let's set up a plan for the analysis. We will run five analyses to understand the impact of different variables and different types of analysis. 

- Model 1: `Likability ~ Message`
	- Effectively a t-test to see if there is a difference between true statements and lies.
- Model 2: `Likability ~ Sender.Warmth*Participant.EI + Message`
	- Standard linear regression model.
- Model 3: `Likability ~ Message + (1|Sender.ID) + (1|Participant.ID) `
	- Incorporating random effects into our first model.
- Model 4: ` Likability ~ Sender.Warmth*Participant.EI + Message + (1|Sender.ID) + (1|Participant.ID)`
	- Model 2 with Random effects. 

## Priors

Before we set our priors, we should check the defaults. Ultimately we want to build up to a linear mixed effects model so that we will look at the defaults for that model. To see what the priors are for a model, we can use `get_prior`

```{r}
get_prior(Likeability ~ Sender.Warmth*Participant.EI + Message + (1|Sender.ID) + (1|Participant.ID), data = Lie.Data, family = gaussian)
```

The first part of this function is the model we would like to test. Here we are seeing if the warmth of the sender, the participant's emotional intelligence and the type of message predict the perceived likability of the senders, letting individual differences of senders and participants be random effects. 

>*The final argument `family = gaussian` tells the function what type of data we expect for `Likeability`. If your DV is a continuous measure (i.e. you are dealing with anything related to linear regression), then you do not need the argument. Refer to more advanced statistical guides if you use dichotomous or count data as your DV.* 

The output of this command is a table with no legend. The primary columns we are concerned with are `prior`, `class`, `coef`, and `group`. 
- The `prior` column states the default prior for that variable. For a fixed effect variable, that will typically be a flat, uninformative distribution.
- `class` tells you if you are dealing with a fixed or random effect variable. Fixed effects are class `b`, while random effects are class `sd`. Random effects are denoted `sd` because the prior describes the distribution of standard deviations for that variable. 
	- You will also notice classes `intercept` and `sigma`. `intercept` is what you expect, while `sigma` is the random error not described by the model.
- `coef` and `group` are the names `R` will assign to each variable. 

This table allows us to determine the classes and names required to set our priors.

We will assign our priors now to make our code more readable. 

For our priors, let's say that the average difference between the effect of lies and factual statements on likability is .5 with a standard deviation of 2. We will set the mean for warmth and emotional intelligence at 0 with a variance of 1. We won't touch any other variables. 
> I very much pulled these values at random. It would be best if you based them on the results of previous research. 

```{r}
Model1.Prior <- set_prior("normal(.5,2)", class = "b", coef = "MessageT")

Model2.Priors <- c(
	set_prior("normal(.5,2)", class = "b", coef = "MessageT"),
	set_prior("normal(0,1)", class = "b", coef = "Sender.Warmth"),
	set_prior("normal(0,1)", class = "b", coef = "Sender.Warmth")
)

```

## Analysis

Now for arguably the most straightforward step, running the models! 

```
Model1 <- brm(
	Likeabiliy ~ Message,
	data = Lie.Data,
	prior = Model1.Prior,
	save_pars = save_pars(all = T),
	iter = 8000,
	warmup = 2000
)

Model2 <- brm(
	Likability ~ Sender.Warmth*Participant.EI + Message,
	data = Lie.Data,
	prior = Model2.Priors,
	save_pars = save_pars(all = T),
	iter = 8000,
	warmup = 2000
)

Model3 <- brm(
	Likability ~ Message + (1|Sender.ID) + (1|Participant.ID),
	data = Lie.Data,
	prior = Model1.Prior,
	save_pars = save_pars(all = T),
	iter = 8000,
	warmup = 2000
)

Model4 <- brm(
	Likability ~ Sender.Warmth*Participant.EI + Message + (1|Sender.ID) + (1|Participant.ID),
	data = Lie.Data,
	prior = Model2.Priors,
	save_pars = save_pars(all = T),
	iter = 8000,
	warmup = 2000
)

```

Now, to understand what is happening here, let's break down `Model1`. The first three lines tell the function how we would like to model our data, the data we are using, and the priors we chose. The fourth line states that we want to save all the parameters generated by the model. The fifth and sixth lines relate to how Bayesian methods calculate parameter values. These last three lines allow us to calculate the Bayes factor later on. 
>One of the downsides of this package is that it will take a while to run, with more complicated models taking significantly longer. As such, `Model4` may take a few minutes to run. To speed things up, delete each model's fourth and fifth lines.

Once we have run the above code, we can use `summary()` to see the results.

## Hypothesis Testing

There are two easy ways to test hypotheses in `brms`: `hypothesis()` and `bayes_factor()`

### hypothesis()

This function allows you to test different hypotheses about relationships between variables *within a model*.

For instance, we can test if lying impacted how likable a person was in `Model3`

```{r}
hypothesis(Model3, "MessageT = Intercept")

```

This will test if there is a significant difference between the baseline (the impact of lies on likability in this case) and the variable `MessageT` (the effect of true statements on likability) with 95% certainty. 

### bayes_factor()

This function allows you to see if there is a difference *between models* and, if so, which model has more support.

For instance, we can test if adding emotional intelligence and warmth to the model made a difference.

```
bayes_factor(Model1, Model2)

```

This will tell us the degree to which the data favours `Model1` over `Model2`.

Or, we could see if mixed effects models are helpful in this case.

```
bayes_factor(Model2,Model4)

```