# Concentration inference

This file is based on Erik's notes about GFP concentration estimations, and aiming at reasonnably measuring GFP concentrations in our 96 well plate measurements with our robot.

GFP concentration inference is performed the following way:
From the data...

- Data manual curation: we want to keep only experimental data where cells are exponentially growing, or more generally, data where there seems to be a linear relationship between the fluorescence and the OD. David working on that.
- Compute $A_{i,p}$ and $B_{i,p}$ from experimental data.
- Perform a dichotomic search to find the optimal $\beta$, and \alpha_p.
  - Compute the derivative of $\frac{L_*(\beta)}{d\beta}$.
  - If the derivative is negative, then set $\beta$=0
  - Else set $\beta_min$=0
  - Set $\beta$ for a large value and search for a derivative that has a negative value (that can be done by doubling $\beta$), and stop when beta is negative again. Set $\beta_min$ to that value.
  - Figure out in the middle what is the sign of the derivative and set either $\beta_min$ or $\beta_max$ to that value.
  - The following can be iterated until $2*|\beta_min-\beta_max|/(\beta_min+\beta_max) < 0.001$
  - Once optimal $\beta$ has been found, use it to compute $\alpha$.
  - Compute the variance of \alpha_p


# Setting up the computation

- The best way to do that would be to simply do everything in Rstudio. And parrallelize computation if necessary.
- I will certainly have to generate a proper data set first, through simulations.

- To do that, I simply need to write a small simulation that actually looks like a OU process to simulate increasing OD over time with a given chosen noise level.

- For each experiment then, I pick from a gaussian distribution a given growth rate.
- I start from an OD of 0.01 in each case, at each delta t, I am growing by a certain amount. Actually, I do not need any generative process there. 
- On top of this, I will add some noise, which we'll be gaussian distributed around my "true" data (experimental noise on the OD).

- Once this is done, I need to choose a given \beta (around ~50) and alpha_p as well as alpha_0 (autofluorescence), so that
- fluo=OD*(alpha_p + alpha_0)+beta+noise (again, noise is randomly distributed there).

- Once I have my data, I can start working on the implementation of the inference.



