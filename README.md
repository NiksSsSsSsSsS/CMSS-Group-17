
### CMSS-Group-17

# Agent-based Modelling​
## Probability simulation of a societal ​collapse in an isolated environment​
### (via NetLogo)​

------------------------------------------------------

[706030] Computational Modelling of Social Systems (SS)

Final Project by Group 17 - Team members:
    
    Tanja Boric
    Verena Grall
    Niklas David Schramm
    Paul Jakob Sedlmayr

Important dates:

    22.05: First project feedback exercise session
    12.06: Second project feedback exercise session and general Q&A session
    12.06: Deadline to register your group in Teach Center. Registration was opened earlier today.
    19.06: First presentations session
    26.06: Second presentations session
    10.07: Project report deadline

------------------------------------------------------

## Useful links

#### Markdwon 
- Online Editor for markdown (md) files such as this README.md - https://readme.so/de/editor

- Look-up table for basic markdwon syntax - https://www.markdownguide.org/basic-syntax/

#### NetLogo
- Download: https://ccl.northwestern.edu/netlogo/6.4.0/

#### Flowchart Creator
- draw.io - https://app.diagrams.net/

------------------------------------------------------

## Presentation

Link to the Power Point Presentation:

https://unigraz-my.sharepoint.com/:p:/g/personal/niklas_schramm_edu_uni-graz_at/EaNu7IcmR41OoMHv9IJxpj4BrR4LGyHZIMFZjfl4RZsN6g?e=SfldAz

Further Presentation information:
  - The 10-minute time limit for presentations is strict, points will be deducted if you take longer.

  - Discussants have to pay attention to their corresponding presentation, as the student projects will not be available before the presentations.

  - We have not set a limit on the number of pages of reports but we trust that you will try to be concise. Please do not include redundant output or notebook code, your report has to be readable and well-edited.

  - A good practice is to structure your presentation with at least one slide for each part of the report, so it is easier to follow.

  - You will learn the date when your project presents and when it iss discussant on the week after groups are registered. Prepare as if you would present on the first session.

------------------------------------------------------

## Report

Link to the Text files with ideas:

https://unigraz-my.sharepoint.com/:w:/g/personal/niklas_schramm_edu_uni-graz_at/EeBLnCoUompDg08Mth5LWpEBGOLcMMrNEzTTY4S28GEdJg?e=p9YLEc

Further report information:

    - Motivation: research question including some references to argue about previous work 

    - Model description: precise explanation of all dynamics and assumptions and what other models it is based on. If any data is involved, please explain it here. 

    - Model analysis: Visualization of the model dynamics to provide insights and results of simulations 

    - Conclusions: reflect on your results and how they inform the question that motivated the project 


------------------------------------------------------

# New addition inspired by lecture: Attractiveness

(inspired by Slides #1, page 24 etc.)

Netlogo: marked LOC with 

    <----NEW(3) 


Incorporation of the "attractiveness matching" date choice model (by Kalick and Hamilton, 1986) from the lecture slides #1, pages 24-28. For this, we modified the mate procedure:

### Acceptance Mechanism: 
Attractiveness = assigned to each agent at birth. 

It uses a random value between 1 and a specified maximum (OPTIONAL - subject to change)
- male_attractiveness_scale for males 
- female_attractiveness_scale for females)

### Attractiveness Match: 
    let attractivenessMatch (attractiveness - [attractiveness] of partner). 

When a potential partner is found: 

the attractiveness difference (attractivenessMatch) between the male and female is calculated. 

This difference can be positive or negative, indicating how much more or less attractive the partner is compared to the agent.

### Acceptance Probability: 
    let acceptProbability (1 - abs(attractivenessMatch) / 10)

Acceptance probability is calculated (range between 0 and 1) using the absolute value of the attractiveness difference. 

We use abs to ensure that the difference is always non-negative. 

By subtracting from 1, we invert the probability.

Meaning: a smaller attractiveness difference results in a higher acceptance probability, while a larger difference results in a lower acceptance probability.

### Acceptance Decision: 
    if random-float 1 <= acceptProbability and random-float 1 <= acceptProbability

Agents (both male and female) decide whether to accept the partner based on this calculated probability. 

Both agents must independently agree (based on their own acceptance probability) for mating to occur.

### Conditions for Acceptance
The probability of acceptance depends on how close the agents' attractiveness levels are.

Agents are more likely to accept partners whose attractiveness is similar to their own.

For example:
- If the attractiveness difference is 0 (exact match), the acceptance probability is 1 (100% chance of acceptance).
- If the attractiveness difference is 5, the acceptance probability is 0.5 (50% chance of acceptance).
- If the attractiveness difference is 10, the acceptance probability is 0 (0% chance of acceptance).


### Example Scenario:
Let's say we have a male agent with attractiveness 7 and a female agent with attractiveness 5:

Calculate Attractiveness Match:

    let attractivenessMatch (7 - 5) ; which is 2

Determine Acceptance Probability: 

    let acceptProbability (1 - abs(2) / 10) ; which is 0.8

Acceptance Decision: Both agents will generate a random number between 0 and 1.
- If both random numbers are ≤ 0.8, the agents accept each other and mating occurs.
- If either agent's random number is > 0.8, the mating attempt fails.

### Attractiveness at birth

We added a new logic for influencing the attractiveness at birth per gender.

2 new sliders:

    male_attractiveness_scale
    female_attractiveness_scale

The standard is 10. This means that a child can be born as anything between a 1 to 10 (attractiveness-wise). 

If we set it to 3 for males, e.g., male children can only be born as a 1, a 2 or a 3. 

If the difference between the genders is to high, the population goes extinct due to the previously added attractiveness filter.

Example Results for attractiveness: if the scale for attractiveness at birth is set to... 
- M=2, F=2 --> stable population (because on average, the chances are equal)
- M=3, F=7 or inverted --> population goes extinct (difference is too big, too many rejections = pickiness too high in the long run)
- M=10, F=10 (standard values), means that children can be born as 1 or a 10


------------------------------------------------------

#  New addition chosen by us: Polarizing View (for Homicide Rate)

(inspired by Slides #1, page 6-7)

Netlogo: marked LOC with 

    <----NEW (*) 

### Agent-specific Additions:

- added *polarizing_view* parameter to each object struct (males-own & females-own = m/f agents).
- set random value between 1-10 at time of birth (in *actFemales* function)

      set polarizing_view random-float 10

### Global Additions:

- Added a Monitor (for counting up the number of kills caused by different views)
  - in the interface, select "Monitor" (Button) 
  - Reporter = numberKillsByViewDifference
  - Label (optional) = Number of Kills by View_Difference

### Check Polarizing View:

- new function *checkPolarizingView* (in *setup*)
- only asks agents outside of the safety-zone
  - to avoid checking for pregnancy
  - to avoid checking age 
  - won't kill pregnant agents or children by default

When two agents meet outside of the safety-zone:

- check difference:
    
      let view-difference abs (polarizing_view - [polarizing_view] of other-turtle)

- if *view_difference* surpasses a certain treshold:

      let to-die one-of (list self other-turtle) ; self or the other (only 1) randomly dies

- one of the two agents dies (randomly chosen), the other one survives the encounter:

      ask to-die [ die ]

- add +1 to polarizing_view kill counter (only for statistical reasons)

      set numberKillsByViewDifference numberKillsByViewDifference + 1
  
------------------------------------------------------

# References

- [1] Github: Simulation of a 6,300-year intergalactic journey​: https://github.com/SommerEngineering/Simulation-of-long-distance-space-flight?tab=readme-ov-file ​

- [2] Model: Sommer, Thorsten (2019). Simulation of a long-distance space flight. DOI: 10.5281/zenodo.3382912​

- [3] Procedure of the Simulation:  Sommer, Thorsten (2019). Simulation of a 6,300-year intergalactic journey. DOI: 10.17504/protocols.io.676hhre​

- [4]  Experiment of Frédéric Marin and Camille Beluffi from 2018: https://arxiv.org/abs/1806.03856 
