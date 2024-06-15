
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
    last change: 15.06.2024, 00:56 by Tanja

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
    last change: ??.06.2024, 00:56 by XY

Link to the Text files with ideas:

https://unigraz-my.sharepoint.com/:w:/g/personal/niklas_schramm_edu_uni-graz_at/EeBLnCoUompDg08Mth5LWpEBGOLcMMrNEzTTY4S28GEdJg?e=p9YLEc

Further report information:

    - Motivation: research question including some references to argue about previous work 

    - Model description: precise explanation of all dynamics and assumptions and what other models it is based on. If any data is involved, please explain it here. 

    - Model analysis: Visualization of the model dynamics to provide insights and results of simulations 

    - Conclusions: reflect on your results and how they inform the question that motivated the project 

------------------------------------------------------

# 1. Morality
    last change: 15.06.2024, 00:45 by Tanja

### TODO 1:
Reference and explain what we want to do here 
### TODO 2:
Implement and mention changes

### Example Scenario:
### TODO 3:
Mention an example

------------------------------------------------------

# 2. Homicide Rate
    last change: 15.06.2024, 00:45 by Tanja

Similar to Slides #1, page 6-7?

### TODO 1:
Reference and explain what we want to do here 

### TODO 2:
Implement and mention changes

### Example Scenario:
### TODO 3:
Mention an example


------------------------------------------------------

# 3. Attractiveness 
    (last change: 15.06.2024, 00:45)

Incorporation of the "attractiveness matching" date choice model (by Kalick and Hamilton, 1986) from the lecture slides #1, pages 24-28. For this, we modified the mate procedure:

### Acceptance Mechanism: 
Attractiveness = assigned to each agent at birth. 

It uses a random value between 1 and a specified maximum 
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
------------------------------------------------------

# References

- The Kalick and Hamilton dating model (1986)
- (...)
