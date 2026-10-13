# Emergency water allocation for Delhi under repeated supply disruptions

Research paper draft

## Abstract

Helping Delhi address water availability problems requires examining both the amount of water available and how limited emergency supplies are shared during outages. This study investigates whether allocation that remembers past shortages can improve protection for repeatedly underserved areas under constrained resources.

Official records were audited before modeling. Facility registers, intended schedules and historical administrative availability estimates could not establish comparable household delivery. Observed evidence and assumed operational conditions were therefore kept separate. A water balance simulation compared six allocation strategies and a zero-history control using identical disruptions, storage limits, delivery capacities and emergency budgets. Synthetic cases were complemented by twelve engineering reporting units from the Comptroller and Auditor General's Delhi Jal Board audit, retaining two published target benchmarks.

Definitions and inputs were locked before evaluation. The main experiment comprised 288 scenarios and 2,016 policy runs; sensitivity experiments comprised 840 scenarios and 5,880 runs. The primary outcome was the least-served unit's cumulative served-to-target ratio over 45 simulated days.

History-based sharing improved this outcome relative to current-shortfall allocation in tested historical cases. However, cumulative minimum-service allocation performed better throughout reference, lower-budget and tested storage-variant settings. Higher-budget rankings and synthetic comparisons were mixed. Aggregate shortage remained unchanged across policies because emergency water could not replenish reserves.

The results identify conditional opportunities for fairer outage response, not verified improvements in Delhi delivery or transport efficiency. Practical use requires measured operational inputs, validated service-area mapping and prospective comparison with actual allocation decisions. Efficient outage management remains a future application rather than a demonstrated outcome.

<!-- PAGE -->

## Contents

<!-- TOC -->

## 1 Introduction and objectives

Water availability isn't just about producing more water. When an outage happens, the city has to decide how to direct a limited emergency supply among many competing needs. If a decision is only based on today's largest shortage, it might ignore an area that has already suffered from a lack of water for several days. On the flip side, a rule that only compensates for past deprivation might fail to protect an area that is currently facing the worst overall service. To actually help a city like Delhi manage water, we need clear goals, strong points of comparison, and an honest understanding of what our models can and cannot do.

This project focuses on the allocation side of the problem. The practical motivation is to find decision-making methods that could eventually help Delhi respond to outages using limited emergency resources. The current contribution of this paper is a controlled computer simulation that compares how different fixed rules distribute unmet water targets over time. We do not attempt to estimate exact household water consumption, map out the pipe network, or propose real-world operational tanker routes.

Our main research question is: when disruptions happen repeatedly and emergency budgets are tight, how do different allocation rules affect the neighborhood that receives the least water over time? Our hypothesis was that accounting for past water shortages would improve outcomes for these worst-off areas, compared to strategies like sharing equally, sharing based on target size, or sharing based only on today's shortfall. We also tested against stronger daily and cumulative minimum-service benchmarks. Whether the history-based rule actually performs well against these benchmarks is something we tested computationally, rather than just assuming it works.

A secondary goal is to see how the rankings of these rules change depending on when disruptions happen, how much storage is available, the size of the emergency budget, and the assumed normal supply. By matching scenarios, we ensure our comparisons are fair: every rule faces the exact same sequence of events and limits. We examined results separately for synthetic data and for two historical administrative targets. Importantly, we didn't cherry-pick the target that gave us the best result.

The audit of public data is a core part of this research, not just a preliminary step. It helps us draw a clear line between what is actually observed in the real world and what our model has to assume. We want to find a useful method to support future decisions without making the false claim that our simulation alone has solved Delhi's water crisis.


## 2 Delhi evidence and related practice

### 2.1 What the public records measure

The DUSIB (Delhi Urban Shelter Improvement Board) register lists facilities and often repeats locations. We downloaded the data and kept 840 rows after careful cleaning. We identified 408 rows that specifically mentioned JJ (jhuggi jhopri) clusters, grouping them into 342 candidate locations. For 246 of these groups, we couldn't find information about their water source. We cannot treat this facility data as a household survey, and we kept missing numeric values as "unknown" rather than assuming they meant zero.

We also looked at a sample of 19 water schedules from the Delhi Jal Board (DJB). This is just a small, manually selected excerpt from two official documents, not a representative sample of the whole city. Eighteen of these records showed the duration of water supply in minutes per day, and one ambiguous record was excluded. The sample included areas getting water every other day, areas getting continuous 24-hour service, and thirteen areas where the frequency wasn't specified. Because we couldn't automatically verify the original PDF layouts, these excerpts serve as feasibility evidence only.

We used Assembly Constituency codes to connect all nineteen DJB excerpts, but those specific areas only covered 66 of the 840 DUSIB records. When we tried to match the exact text of locality names, we found zero matches. Therefore, linking by these codes provides context, but it doesn't give us a clear picture of household exposure or directly link infrastructure to service quality.

### 2.2 Historical administrative inputs

Our main historical data source is the Comptroller and Auditor General (CAG) Report 3 of 2025 on the DJB. Annexure 3.1 of this report provided sixty annual records for twelve engineering reporting units from 2017-18 to 2021-22. We archived the original PDF and visually verified the data. For our case study, we used the 2021-22 reported water availability and two published benchmarks: "DJB requirement" and "desired availability". It is important to note that these are high-level administrative estimates, not measured daily household demand or confirmed delivery.

Coverage and metering are significant limitations. The reported zonal availability for 2021-22 totals 694.73 MGD (million gallons per day), but a separate aggregate figure is 742.81 MGD. This 48.08-MGD gap remains unresolved. Furthermore, engineering-unit labels are not the same as Assembly Constituency identifiers, and we cannot verify if they cover the entire city. Figure 1 shows the ratios of these sources without making claims about whether household needs are actually being met.

### 2.3 A relevant Delhi equity-control precedent

An official JICA/DJB report from March 2018 describes a trial where the Haiderpur plant supplied the Pitampura UGR/BPS and three pilot metered areas. It describes a system using valves and automated control (SCADA) to manage inlet flow, proving that equitable distribution is a real, practical concern in Delhi. We use this source as related practice, not to claim our rules are entirely novel.

Two of the pilot areas were not hydraulically isolated, meaning inlet flow cannot simply be treated as household receipt. The report also has conflicting dates, mentioning both December 2017 and February 2017, leaving the exact timeline unclear. This historical data does not provide a verified map of 2024 services. The trial was different from emergency allocation and does not serve as a numerical validation of our current simulation.

<!-- LANDSCAPE -->

## Figure 1 Historical administrative context

![Figure 1](figures/01_historical_context.png)

Figure 1. Reported 2021-22 administrative availability divided by each published target for all twelve CAG reporting units. Ratios describe the variation in supply, not actual household access, and do not represent a complete map of the city. Source: archived CAG Annexure 3.1 and the saved historical-context table.

<!-- PORTRAIT -->

## 3 Simulation model and sharing rules

### 3.1 Daily accounting and information

For each neighborhood (or unit) on any given day, our simulation tracks a target demand, normal supply, starting water stock, storage capacity, emergency delivery capacity, and a shared emergency budget for the whole system. The initial shortfall is calculated as the target minus the starting stock and normal supply. Any emergency allocation given to a unit must not exceed its delivery capacity, and the total emergency water distributed across all units cannot exceed the overall budget. The final served quantity is the minimum of either the target demand or the sum of stock, normal supply, and emergency allocation. Whatever is left over is the unmet quantity.

Any surplus water after meeting the target can be stored for the next day, up to the storage capacity limit; anything beyond that overflows. Every day's calculation must balance: starting stock plus all supplies must equal the served quantity plus ending stock and overflow. This is a basic daily accounting model, not a complex plumbing or hydraulic model. We do not simulate water pressure, pipe leaks, time of delivery, household distribution, or water quality.

Normal supply can refill storage reserves, but emergency water is strictly capped to only meet that day's remaining deficit. Unused emergency budgets cannot be saved for tomorrow. Because of these rules, the total amount of emergency water used and the overall storage levels end up being the same no matter which policy is used. Therefore, our study compares *how* the shortage is distributed among units, rather than claiming to reduce the total shortage or cut transportation costs. Real-world limits like delivery stops or travel time could change this, but we did not introduce them after seeing the results.

All policies make decisions using only information available up to the current day; they cannot look into the future to see upcoming failures. The historical daily baselines we used are assumptions derived from annual figures, not actual day-by-day production logs. All water quantities in the simulation are arbitrary or normalized units, not measured liters delivered to households.

### 3.2 Policies and control

| Rule | Allocation principle |
|---|---|
| Equal sharing | Distributes water equally among all units that have unmet targets. |
| Target proportional | Distributes water based on the size of a unit's target demand. |
| Current shortfall | Distributes water based on today's deficit before emergency supply. |
| Shortage history | Multiplies today's deficit by a factor based on past unmet targets. |
| Daily minimum service | Tries to maximize the lowest daily service floor for any unit today. |
| Cumulative minimum service | Tries to maximize the lowest accumulated service floor over time. |
| Zero-history control | A baseline check that ignores history; it acts just like the 'Current shortfall' rule. |

The first four rules use a capped proportional system, meaning they distribute resources based on ratios and reallocate any leftover water to units that still need it. The "shortage history" rule uses a simple multiplier based on past deprivation. We do not claim this is a brand-new invention.

The two minimum-service benchmarks use linear programming (via SciPy/HiGHS) to find the mathematical optimum for fairness. They try to make the worst-off unit as well-off as possible on that specific day, using available history. They do not optimize the entire future timeline or use complex, multi-tiered fairness logic. The zero-history control is just a way to double-check our work, not a completely new strategy.


## 4 Experimental design and validation

### 4.1 Historical normalization and synthetic control

We scaled the total reported 2021-22 baseline for all twelve units to equal exactly 1,000 units of water. Both historical targets were multiplied by this same scale factor. This keeps the ratios between sources accurate without pretending we know exactly how much water was delivered daily. Storage capacities, delivery limits, and emergency budgets are tied to these baseline numbers and are kept identical across all tests.

We also built a synthetic control group with twelve completely hypothetical units. These had varying demands, and their normal supply was set to 1.1 times their base demand. Daily demands fluctuated randomly by plus or minus ten percent. These are not real Delhi settlements. We use this synthetic group to check if the results from the historical data are just a quirk of that specific dataset or a general trend.

We tested three types of water disruptions: 
1. **Isolated:** Randomly selected days where supply fails.
2. **Persistent:** A continuous block of days where supply fails for a single unit.
3. **Correlated:** A continuous block of days where supply fails for a group of four units at the same time.
During a failure, the supply drops to just twenty-five percent of the normal baseline. These failure patterns are controlled assumptions, not actual probabilities of failure in Delhi.

### 4.2 Locked evaluations

Our main evaluation ran over a 45-day simulated period using eight pre-selected random seeds (numbers that control randomness). We tested four scenarios combining the three failure patterns with the different input types (synthetic demand and the two historical targets). This resulted in 288 scenarios and 2,016 total policy runs. In our standard "reference" setup, we assumed twelve failure days, storage capacity equal to half a day's baseline, half-filled initial tanks, an emergency budget of eight percent of the total baseline, and delivery caps at sixty percent of the baseline. We also tested variations: "thin storage" (0.1 days), "higher budget" (twenty-five percent), and "higher exposure" (eighteen failure days).

A second, locked sensitivity study used four different random seeds and tested seven different changes to the water supply across five resource settings, producing 840 scenarios and 5,880 runs. We tested increasing or decreasing supply uniformly by ten and twenty percent, and also tested swapping supply between different groups of units (which are just arbitrary groups, not actual neighborhoods). Resource budgets remained tied to the original baseline supply.

These stress tests are assumed scenarios, not calculated real-world error margins. Changing the storage capacity also changed how much water was in the tanks at the start. The model assumes perfect information; we did not simulate delayed or incorrect reports from the field. Both studies were locked in before we ran them, meaning we didn't tweak the rules to get better results.

### 4.3 Outcomes and checks

Our primary measure of success was looking at the worst-off unit—specifically, the minimum ratio of total water served compared to its total target over the 45-day period. This metric treats every unit equally, regardless of population size. We also tracked the spread of attainment, the maximum number of consecutive days a unit faced a deficit, the total fraction of unmet demand, and the total emergency water used. All comparisons are paired, meaning we compared rules that faced the exact same scenario.

We ran 99 automated tests to verify our accounting, check limits, ensure the model couldn't peek into the future, and validate our data parsing. We saved all complete results, even the ones that didn't favor our hypothesis. We did not change any frozen definitions while writing this paper.


## 5 Results

### 5.1 Reference comparisons

In the main historical tests (using the standard reference assumptions), the history-based sharing rule consistently outperformed the basic current-shortfall rule. The worst-off unit saw an improvement in its total water attainment by about 0.5 to 0.74 percentage points on average. However, the stronger cumulative minimum-service rule outperformed the history rule by a much larger margin—about 4.74 to 8.71 percentage points. Because this stronger benchmark performed so well, it changes our conclusion: the history rule is an improvement over the basics, but it is not the best possible approach.

The daily minimum-service rule also frequently beat the history rule, though the results varied depending on the exact target and disruption pattern. There was no single rule that was universally the best in every single case. Figure 2 shows these results across all patterns and targets.

When looking at the synthetic (hypothetical) data, the results were mixed. Against the strong cumulative minimum-service rule, the history rule performed worse during isolated disruptions, but slightly better during persistent and correlated disruptions. This proves that no policy is universally superior; their success is conditional on the exact scenario.

### 5.2 Resource and input sensitivity

Across all 840 historical sensitivity tests, the history rule always beat the basic current-shortfall rule in helping the worst-off unit. However, the cumulative minimum-service rule beat the history rule in every single standard, lower-budget, thin-storage, and deeper-storage scenario. The only time the history rule had a chance to win was when the emergency budget was significantly higher (see Table 1 and Figure 3).

| Higher budget target | History wins | History losses | Ties | Paired scenarios |
|---|---:|---:|---:|---:|
| DJB requirement | 31 | 51 | 2 | 84 |
| Desired availability | 23 | 48 | 13 | 84 |

Table 1. History versus cumulative minimum service when given a higher emergency budget. The history rule wins some matchups here, but loses all matchups in tighter budget scenarios. 

Figure 4 shows how the average outcomes changed when we artificially stressed the water supply. These ranges show how the model reacts to different assumed supply changes, but they do not represent real-world measurement uncertainty.

### 5.3 Accounting and duration limitations

As expected by our model's design, the total amount of unmet water demand and the total emergency water used were identical across all policies. This confirms our accounting logic works, but it means we cannot claim that our rules reduce the overall water shortage or save transportation costs in the real world. Across thousands of simulated runs, the maximum streak of consecutive days with a water deficit always hit the maximum 45-day limit. Because of this, we cannot use this metric to claim we improved the persistence of shortages.

<!-- LANDSCAPE -->

## Figure 2 Paired reference policy comparisons

![Figure 2](figures/02_reference_policy_comparison.png)

Figure 2. A comparison showing how the history rule performs against other baselines in terms of total water attainment for the worst-off unit. Positive values mean the history rule is better; negative values mean the baseline is better. The dots represent average outcomes. The history rule beats the simple current-shortfall baseline, but loses heavily to the cumulative minimum-service rule.

<!-- LANDSCAPE -->

## Figure 3 Resource dependent policy rankings

![Figure 3](figures/03_resource_rankings.png)

Figure 3. Counts of how often the history rule wins, loses, or ties against the cumulative minimum rule across different resource settings. In standard and tight-budget scenarios, the cumulative rule always wins. When the budget is higher, the results are more mixed.

<!-- LANDSCAPE -->

## Figure 4 Input stress sensitivity

![Figure 4](figures/04_input_sensitivity_ranges.png)

Figure 4. This shows how the outcomes change when we artificially alter the water supply inputs (like dropping supply by 20%). These lines show the range of results based on our assumed stress tests, not real-world confidence intervals.

<!-- PORTRAIT -->

## 6 Discussion and limitations

### 6.1 What the experiment contributes

This study highlights a useful difference between simply compensating for past deficits and actively protecting the worst-off area's overall service level. The history rule adjusts today's water delivery based on past deprivation, but it doesn't directly try to maximize the absolute minimum outcome. The cumulative minimum-service rule does exactly that, which explains why it performs so much better when resources are tight. This is our interpretation of the simulation, not a mathematical proof that the cumulative rule is perfect for the future.

Having more emergency resources doesn't automatically mean one rule is always best. When budgets are large, limits on delivery capacity and choices about how to share leftover water can change outcomes. Both strong rules can protect a baseline level of service while taking different paths to get there. Our comparisons show that outcomes are very sensitive to assumptions about resources, meaning we haven't identified one "optimal" budget for Delhi.

Therefore, our claim of innovation is modest. We provide a transparent, reproducible way to test emergency allocation rules fairly, using strong benchmarks and acknowledging when our hypothesis fails. We do not claim to have invented fairness optimization or history-based tracking.

### 6.2 What efficiency means here

In the real world, "efficiency" could mean less wasted water, shorter travel times for tankers, fewer delayed deliveries, or better use of reservoirs. None of those improvements are guaranteed just because a rule improves the minimum served-to-target ratio in a simulation. Our model doesn't include distances, vehicles, or financial costs, and the total amount of water shortage remains exactly the same regardless of the rule used. Our findings only show *who* bears the burden of the scarcity.

This limited scope is still useful. Before spending money on routing software or network upgrades, a city can test different fairness goals in a controlled setting. But for practical use, a city must look at both fairness and actual operational costs together. We do not claim this model will improve Delhi's overall water availability or fleet efficiency by any specific percentage.

### 6.3 Measurement and external validity

The historical targets we used are published goals, not actual measurements of what households demand, and the data is older than our current experiments. Missing meter data and incomplete coverage cannot be fixed just by running more simulations. We assumed that annual availability translates to a constant daily baseline, and our failure timings and emergency resources are entirely hypothetical. 

Our success metric treats all reporting units equally, regardless of their population size. It does not measure true population welfare or specifically protect the most vulnerable households within a district. We didn't try to guess income levels or neighborhood-specific exposures because doing so requires strong, defensible data that we do not have.

The limited schedules, selected news stories, and sparse production data we have cannot prove that our policies will work in the real world. Just because there isn't an official notice doesn't mean there isn't an outage. Seeing a water tanker doesn't mean a neighborhood has zero piped water, and an intended schedule doesn't guarantee water was actually delivered.

The 2018 JICA pilot is a valuable historical example, but it isn't a substitute for current event data. Because the pilot areas weren't isolated, we can't treat the water flowing into them as exactly what households received. Unclear dates in the reports prevent us from building a clean timeline. We cannot simply take a historical map and pretend it accurately represents 2024 service areas.


## 7 Future work toward practical outage management

### 7.1 Establish a measured operating case

The next practical step is to test this on one specific, well-documented neighborhood during a known outage, rather than trying to optimize the whole city at once. We would need exact measurements of water flowing in before and during the disruption, actual reservoir levels, a verified map of the area, and precise logs of when emergency water was sent and received. Water quality needs to be tracked separately from water quantity. Most importantly, we need observations at the household or local delivery point to prove people actually got the water, rather than just assuming they did based on production numbers.

Any original documents used must be carefully saved and verified. We need clear links connecting water plants, reservoirs, administrative zones, and specific neighborhoods, and we must report any missing data honestly. An old project map isn't enough to prove current coverage. If data is missing for a certain period, it should stay missing unless we have real evidence to fill the gap.

### 7.2 Test decision support before deployment

Once we have a solid case study, these allocation rules should be tested in "shadow mode." This means the computer suggests how to allocate water, but humans still make the actual decisions. We can then compare the computer's suggestions against what real operators did under the same constraints. The computer must only use information that would have actually been available at the time, without peeking into the future.

We should judge the trial based on actual delivered quantities, delays, resource usage, and fairness metrics agreed upon beforehand. This requires a strict plan, proper authorization, and independent reviews for safety and equity. Human operators must always retain responsibility; unproven algorithms should never automatically dispatch emergency water.

Moving slowly reduces the risk of over-promising. The first test is simply seeing if the computer's inputs match the real world, not whether it can beat a baseline in a simulation. A policy should never be deployed if it mistakes poor data collection in a neighborhood for a lack of need.

### 7.3 Add realistic constraints as separate extensions

Real-world allocation requires knowing travel times, tanker truck sizes, filling station locations, delivery limits, and service time windows. Adding these to the model requires verified fleet records and should be treated as a separate, future project. Once we have that data, we can test fairness and routing efficiency at the same time.

How we store water also needs its own experiment. If emergency water is allowed to refill reservoirs instead of just being used immediately, it changes the entire math of the simulation and introduces new tradeoffs between fairness and efficiency. New tests and constraints should be planned out before running them, and we shouldn't overwrite our old baseline models just to accommodate these new changes.

Dealing with delayed or incorrect information is another priority. Real operators rarely know exactly how much water is missing right now. Future tests should see how these rules perform when the data is noisy, using real-world error rates instead of arbitrary guesses. Finally, setting fairness targets and prioritizing certain households will require clear justification and human oversight.

### 7.4 Criteria for a usable tool

A tool that is actually usable in the real world needs trackable data, clear acknowledgment of uncertainty, understandable recommendations, the ability for humans to override it easily, and constant monitoring to ensure no group is being unfairly ignored. It must be proven to be both operationally feasible and fair. A fancy dashboard or predictive model is useless without these basic checks. The code we have built so far is for comparing research ideas, not for running a real-world dispatch system.


## 8 Conclusion

To help Delhi manage water shortages, we must separate the physical reality of limited water from the decisions about how to distribute it. This study provides a transparent, reproducible way to compare different distribution rules under clear assumptions. We found that a rule that remembers past shortages is better than one that only looks at today's shortfall. However, a stronger rule that explicitly tries to maximize the absolute minimum level of cumulative service performed the best when budgets and storage were tight. When budgets were larger or when using purely synthetic data, the results were mixed.

The main takeaway is not that we have found a magic formula to solve the city's water problem. Rather, it is that fairness over time needs to be tested against strong benchmarks, keeping all assumptions and unfavorable results clearly visible. In our model, the total amount of water shortage stayed exactly the same, and long streaks of water deficits still occurred. Before we can claim any real improvements in efficiency or household benefits, we need measured real-world inputs, accurate geographic mapping, and testing alongside actual operational decisions.

This current work serves as a careful computational study grounded in Delhi's administrative context. The best path forward is to apply this to a tightly measured real-world case, evaluate decisions in a safe "shadow mode," and only then slowly add real-world operational constraints.

## 9 Acknowledgements and contributions

Public administrative sources were made available by DUSIB, Delhi Jal Board, the Comptroller and Auditor General of India, and JICA. The availability of this public data does not imply that these organizations endorse our model or conclusions.

AI assistance supported the investigation of sources, coding, experimental design discussions, data analysis, documentation, and the preparation of this draft. The final submission materials must accurately describe the actual contributions of the student and guide; this document does not assign individual credit or authorship. No mentor names, funding sources, institutional affiliations, or operational approvals have been invented or implied.

## 10 References

[1] Delhi Urban Shelter Improvement Board. JSC / CTC infrastructure report. Undated administrative snapshot; retrieved October 10, 2026. https://delhishelterboard.in/jsc/report/

[2] Delhi Jal Board. Timings of water supply under ACE M-1. Reference date November 23, 2021. Selected pilot excerpts; original layout unverified. https://delhijalboard.delhi.gov.in/sites/default/files/ace_m_1_0.pdf

[3] Delhi Jal Board. Timing of water supply under ACE M-4. Reference date unverified. Selected pilot excerpts; original layout unverified. https://delhijalboard.delhi.gov.in/sites/default/files/ace_m_4.pdf

[4] Comptroller and Auditor General of India. Functioning of Delhi Jal Board. Report 3 of 2025, Annexure 3.1, printed pages 83-84 (PDF pages 113-114). Official Delhi audit listing: https://cag.gov.in/ag/new-delhi/en/audit-report

[5] Japan International Cooperation Agency with Delhi Jal Board. Final Report on the Assistance Related to Delhi Water Supply Improvement Project. March 2018. Relevant mapping and trial: printed pages 3-1, 3-42 to 3-44, 5-4. https://openjicareport.jica.go.jp/pdf/12305603_01.pdf

[6] SciPy. scipy.optimize.linprog documentation. Linear programming interface used for the HiGHS-based benchmarks. https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.linprog.html

## Reproducibility note

The repository retains source provenance, dictionaries, experiment configurations, hash locks and complete paired results. The submission folder includes the four displayed figures and all five consolidated tables, not only favorable cells. Exact source URLs, SHA256 records, consumed seeds and reconstruction commands remain in the research workspace. Figure consolidation is post-experiment presentation; the underlying protocols were frozen before their evaluation seeds were opened.
