# us-election-model

The goal of this project is to create a *simple* model to predict state-level election results (and overall result) from polling data, while accounting for systematic errors across states and elections. The model incorporates both state-specific *fixed* effects and national-level *random* effects to capture deviations from expected vote shares.

## The Model

In this model, the predicted vote share for democrats, $\ p_{s,t}$ for state $s$ in election $t$ is given by:
     
$$p_{s,t} = \text{logistic}(\text{logit}(X_{t}) + \alpha_s + \gamma_t)$$

where $X_{t}$ represents the national polling data for that year (e.g. $X_{t} = 54$%), $\alpha_s$ is the state-specific effect (fit over many years), and $\gamma_t$ is the national-level error for election $t$.

So essentially we assume the national polling data will represent the outcome for each state (hence the logic transform), and then adjust this with our fixed and random effects to make our state-level prediction:
- The state-specific effect $\alpha_s$ is a **fixed effect** (i.e. is assumed to be constant over all years). This captures the propensity of states to vote more democrat or republican (e.g. for Texas this will skew republican and California skew democrat).
- The polling error $\gamma_t$ is assumed to be a normally distributed **random effect** (not state specific), hence this changes each year. This should capture whether polls are systematically wrong for a particular year.

The observed vote share $y_{s,t}$ is modeled using a normal distribution centered around the predicted vote share $p_{s,t}$ as follows:

$$y_{s,t} \sim \mathcal{N}(p_{s,t}, \sigma_{vote}^2)$$

The idea here is to account for any other random errors in the results (not correlated across states). In Logistic regression you would often use something like bernoulli error, however since the error to counts will be tiny here, the thing we really want to model are werid messy systematic effects not captured by other components of the model. The normal distribution can capture this.


## The Code
