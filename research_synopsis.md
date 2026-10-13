# Sharing The Load: A Controlled Study of Emergency Water Allocation

With growing populations comes increasing constraints on essential resources, and our metropolitan hubs face this challenge every year. Perhaps the most prominent resource continually in the headlines is water.

Regular water disruptions create an equity and allocation problem. When emergency resources are limited, should distribution only account for today’s shortages or also try to resolve the deprivation accumulated over time? This project examines that question through computational experiments motivated by Delhi's water infrastructure.

Auditing Delhi’s public official water records, it was learnt that the available data covered schedules and administrative availability estimates which can not establish comparable household delivery. Hence, observed records were separated from hypothetical inputs in the model presented in the project. A water balance model compared six allocation strategies against a zero-history baseline strategy under identical disruptions, storage limits, delivery capacities and emergency budgets. Synthetic cases were complemented by historical inputs from twelve engineering reporting units in the Comptroller and Auditor General's Delhi Jal Board audit, containing two published target benchmarks.

The goal of the experiment was to find the allocation strategy that performed the best at allocating water resources in a host of simulated water disruptions and outages.

At a high level, the results showed that simply taking past shortages into account is better than just looking at today's shortage. However, an approach that specifically tries to maximize the minimum total water someone gets over time (a cumulative minimum-service rule) performed even better under tight constraints. Neither strategy solved the underlying shortage, but they changed who bore the burden.

The findings support the need for studying stronger resource allocation rules that do more than just try to add a daily band-aid on places with outages. For an equitable access of resources, this paper highlights the need for conditional comparisons of how allocation rules affect distribution, in the absence of a universally superior algorithm to devise water allocation. This paper serves as a stepping stone for future work on integrating real-time measurement tools to optimize physical water-tanker dispatch and operational response in cities like Delhi.

## Introduction & Objective

Disruption in water-supply raises fairness questions around how to share limited emergency water resources when water shortages are repetitive and distributed across the city. It is unclear whether a greedy approach of only looking at today’s water disruption when distributing emergency resources is the best way to approach the problem. It is important to study whether this approach overlooks the shortages that may accumulate across earlier days. For instance, the water needs between a neighbourhood that lost water for the first time in the last month may look very different from a town that has received water for only a fraction of the past month. However, remembering past shortages does not automatically produce the fairest outcome.

This project defines metrics to study emergency water distribution and then evaluates 7 potential algorithms in an experimental setting. It tests these algorithms across different simulated scenarios, including isolated outages, persistent neighborhood disruptions, and wider correlated failures, under varying emergency budgets and storage capacities.

Delhi's official records provide motivation and historical administrative context. They do not establish measured household demand or daily delivery, so the central experiment is computational and simulated. The aim is a defensible comparison and to lay a foundation for further research for cities like Delhi.

## Innovation

The project's core focus is fairness across repeated disruptions rather than sharing water for a single day. It combines matched policy comparisons, a zero-history baseline, strong daily and cumulative minimum-supply benchmarks, and separate resource/input stress tests. Two administrative target definitions set by the Delhi Jal Board and Comptroller and Auditor General are also used. The main contribution is a proof for the need to study conditional allocation policy and build a framework to study these for actual city data. Further, it constructs algorithms that are well served to handle this issue.

## Methodology

Official DUSIB, Delhi Jal Board and CAG records were researched for background, geographic identifiers and benchmarks. Raw evidence was preserved and normalized for our usecase. The historical computational case used scaled 2021-22 CAG availability for twelve engineering reporting units and both published target benchmarks.

A Python simulation tracked daily normal supply, starting storage, emergency allocation, served quantity and remaining deficit. Emergency water could meet today's shortage but could not refill storage. Six strategies and a zero-history baseline shared identical inputs, budgets and delivery limits and used no future information. Two minimum-service benchmarks used linear programming to create a system of solvable equations.

The experimental scenarios represented isolated, persistent and correlated disruptions. The same scenarios were repeated across the different policies by hash driven randomness. A 45-day main evaluation generated 288 scenarios and 2,016 runs. Separate locked sensitivity experiments generated 840 scenarios and 5,880 runs across assumed supply changes, resource settings and both targets.

To judge the success of an algorithm, we primarily looked at the worst-off neighborhood—specifically, the minimum proportion of its target water need that was met over the whole period. We also tracked how spread out the shortfalls were, how many days in a row a place went without enough water, and the overall total unmet demand across the whole simulated system. Paired comparisons assessed minimum cumulative target attainment, dispersion, deficit streaks, total unmet fraction and emergency use. Unit tests were used to ensure the sanity of the code. News and historical pilot reports were used for context when devising the experiments.

## Results & Conclusions

The results showed that looking at past shortages when allocating water improves the situation for the most deprived areas compared to only looking at the current day's shortage. However, a rule that explicitly tries to raise the absolute minimum level of service over time (a cumulative minimum approach) was generally the strongest performer when budgets were tight.

In historical reference settings, history-based sharing improved minimum cumulative target attainment over current-shortfall allocation by approximately 0.5-0.74 percentage points, but trailed cumulative minimum-service allocation by approximately 4.74-8.71 points across targets and disruption patterns. The sensitivity study preserved history's advantage over current shortfall, while cumulative minimum service won every reference, lower-budget and tested storage-variant comparison. Higher-budget rankings and synthetic results were mixed.

Total shortage and emergency use were unchanged across matched policies: the model redistributes scarcity rather than removing it. Historical maximum deficit streaks reached the 45-day horizon, preventing claims of improved shortage persistence.

The hypothesis received partial, benchmark-dependent support. Accounting for past shortages can help, but stronger cumulative protection may help more. These are conditional computational findings, not causal effects, measured household benefits or validated Delhi operating recommendations.

## Acknowledgement & References

The project draws on public records from DUSIB, Delhi Jal Board, the Comptroller and Auditor General of India, and JICA. AI assistance supported research, cleaning up government datasets, and plotting graphs. Key sources: CAG, Performance Audit on Functioning of Delhi Jal Board, Report 3 of 2025, Annexure 3.1; JICA/DJB, Final Report on the Assistance Related to Delhi Water Supply Improvement Project, March 2018; SciPy, linprog documentation.
