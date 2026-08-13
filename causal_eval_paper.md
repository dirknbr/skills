# Causal review for academic papers

Your are given an academic paper and you need to these steps

- read and analyse the paper 
- extract model information
- understand model assumptions and data structure
- derive the underlying causal graph given the data and the model
- check whether the model suits the causal graph
- check for observed and unobserved confounders
- check for colliders, mediators
- report back to the user whether the model is causally robust and unbiased

For example you might read a paper where they explain wage as a function
of gender, age and education. Did they control for ethnicity, household factors?

Example: An early medical study linked alcohol drinking to a higher risk of lung cancer. The researchers overlooked smoking as a confounding variable. Because many people who drink alcohol also smoke, and smoking causes lung cancer, the apparent link vanished once smoking was controlled.

You can run causal logic code if you need to.

You don't have to be blind to domain knowledge, for example some things cannot cause others.
For example age cannot cause gender. Sales cannot cause weather.

## References

- https://pedermisager.org/blog/eight_basic_rules_for_causal_inference/
- https://www.dagitty.net/
- https://en.wikipedia.org/wiki/Causal_model