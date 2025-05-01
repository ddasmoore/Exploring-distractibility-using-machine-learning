# Exploring-distractibility-using-machine-learning

## Table of Contents
* [Context and research goals](#Context-and-reseach-goals)
* [Method](#Method)
* [Executive summary of insights](#Executive-summary-of-insights)
* [Technical Details and detailed insights](#Technical-Details-and-detailed-insights)
  *   [Data collection](#Data-collection)
  *   [Data cleaning, transaformations and troubleshooting](#Data-cleaning-transformations-and-troubleshooting)
  *   [Behavioral-data-analysis](#Behavioral-data-analysis)
  *   [EEG-data-analysis-and-Machine-Learning](#EEG-data-analysis-and-Machine-Learning)

The following project is based on my [dissertation](https://escholarship.org/uc/item/9436715m).

## Context-and-reseach-goals
If I tell you the location of a “target” object, you’ll be able to find it faster. Think “Can you find my keys before we leave the house?” versus 
“Can you find my keys that are on the coffee table?” Having context about location helps narrow down where your target is. However our visual environment is also filled with “distractors” that constantly vie for our attention. How does knowledge of a distractor's location impact our ability to find the target? For example: in a video game, before a cluttered game scene appears, a flashing icon briefly highlights a corner where a non-interactive square (the distractor) will be. Your task is to quickly locate a glowing orb (the target) that appears in the same scene. Does previewing the location of the distractor square help you find the target orb faster or does it slow you down in identifying the orb? Or perhaps does it have no impact at all?


**Research goals**

The goal of this study was to examine how advance knowledge of a distractor’s location impacts how quickly we find our target. 
There are several possibilities: 
1) knowledge of a distractor’s location can help find our target faster by narrowing down/reducing where the target can be,
2) knowledge of a distractor location can help us suppress/inhibit that location thereby making finding that target easier or
3) knowledge of a distractor’s location can in fact distract us from finding the target. or
4) knmowledge of a distractor's location can have no impact at all

## Method
I designed an experiment using a visual task to study this question. I created a task where participants were presented with a target and a distractor object on every trial. Participants were instructed to identify the target. On every trial, participants were cued (or given a preview) of the target or distractor’s location or no proivded no cue at all. The cue, when presented, was valid 80% of the time.  I measured behavior (accuracy, reaction time), brain activity patterns using EEG and recorded eye movements using an eye tracker on every trial. I wanted to see how the independent variable- cue manipulation- impacted the dependent variables- behavior and brain activity. 


![image](https://github.com/user-attachments/assets/8ecd68f8-6637-44f6-bf62-9c8d149331bb)

A. This image shows a schematic representation of a trial sequence. Every trial starts with a fixation period where participants are asked to fixate their gaze on the central cross. After this, during the cue
period, either a cue indicating the most likely location of the target (in blue) or distractor (in red) is shown or no cue is presented. Finally, partcipants are presented with a target and distractor. Their task is to identify the target and respond using a mouse click. 

B. This image is a representation of the cue manipulation. Since I used a 80 percent valid cue, we had valid and invalid types of trials. 

C. This image shows the target and distractor presented to the participants. The target was a high frequency gabor, while the distractor was a low frequency gabor. The lines in the gabor could be
pointing in the same direction (congruent) or in opposing direction (incongruent). This manipuation- called a Flanker manipulation- provides an additional attentional challenge and is widely used in psychology experiements
to study attention in human beings. 

## Executive-summary-of-insights
- Based on the both the behavioral and EEG data, there was little evidence of inhibition of a distractor location as a result of distractor cueing. It is more likely that a cue or preview of a distractor location enables a “attend away” or “tag and avoid” mechanism in the brain.
- Compared to no cue or no preview of location, curing the location of a target is  more effective in reducing distraction caused by a distractor present than cueing the location of the distractor.


## Technical-Details-and-detailed-insights

## Data-collection

n=27 participants completed 1320 trials of the task described above. I collected response time and accuracy of the participants as they identified the target on each trial. I recorded eye tracking data
so that I could eliminate trials where participants blinked or looked away from the fixation cross (so attention to target is not conflated with eye movement to the target). I recorded brain activity data using EEG
to examine how distractor's location is processed in the brain as a function of the cue manipulation. Each participant's data was collected over three days of testing, each testing sessions took 90 minutes. The visual task was created using custom scripts in MATLAB (psychtoolbox package)

## Data-cleaning-and-preprocessing
- I re-referenced, re-sampled and epoched the EEG data according to standard methods in the field.
- I filtered each trial so that the EEG data only contained brain activity going from cue to participants' response (and not irrelevant events between trials). I removed electrodes that are known to be noisy, also removed other electrodes that were noisy upon visual inspection. Trials exceeding ± 150μV in remaining electrodes were excluded. Also only trials corresponding to accurate responses (which I got from the behavioral data) were included in the EEG analysis. These procedures also led to exlusion of some participants' data. 
- In the behavioral data, incorrect trials and trials faster than 200ms and slower than 1000ms were removed.
- Final analysis were conducted on n=22 participants' data. 

## Behavioral-data-analysis

A repeated measures ANOVA with congruency (congruent, incongruent), validity (valid, invalid-other, invalid target/distractor) and cue condition (target, distractor) was conducted.
  ![image](https://github.com/user-attachments/assets/fdfa8080-a207-45d3-a365-ec263294df91)

In the image above, response time is plotted. On the X axis, we have the target cue, distractor cue and no cue conditions. On the Y axis, we have response time. The brighter color bars represent congruent trials (target and distractor gabors point in similar direction) whereas the translucent bars represent incongruent trials (gabors pointed in opposing directions). 

Typically, the presence of an incongruent distractor is more distracting than when the distractor has a congruent or similar orientation as the target. This is what we are seeing in the baseline condition. There's signicant distraction in the no cue condition. We see that this distracting effect of the incongruent distractor is significantly reduced when we validly cue the location of the target (in green). However, compared to the no cue condition, validly cueing the location of the distractor is also beneficial in reducing distraction. 


## EEG-data-analysis-and-Machine-Learning

The primary analytical technique for the EEG data was an Inverted Encoding Model (IEM). This multivariate pattern analysis method was applied to specific brain waves that are known to correlate with attention (alpha (8-12 Hz) and theta (4-8 Hz) frequency bands). IEM allows us to understand the extent to which target and distractor locations are processed in the brain. Whatever is better processed or attended will have a sharper and consistent representation in IEM compared to what is not processed or attended. This allows us to understand how our cueing manipulation impacted attention. 

There are two parts to the IEM. First we train the model to learn the mapping between where things appear on the screen and the brain activity that evokes in the participant (training set). Then we “invert” the process with a testing set to estimate which location (distractor or target) was better encoded in the brain, given our cueing manipulation. 

**Machine learning analogy**

The focus on specific EEG features like alpha and theta wave activity is akin to feature engineering in machine learning, where relevant aspects of the raw data are extracted for better model performance.Then a model learns a linear mapping of a feature (location in this case) and sensor-level data. The inversion process then acts as a linear decoder, estimating the underlying location from the observed EEG patterns. The effectiveness of the reconstruction reflects the strength and consistency of the brain’s representation of the spatial information.

![image](https://github.com/user-attachments/assets/dcad8ef2-efdc-4f44-a747-16325825ba6a)

The top panel in this image above shows comparison between cued target location and un-cued target location at alpha .05 level. We can see better reconstruction of the cued target location compared to the un-cued target location (un-cued stimulus in the distractor cue condition) around 200 ms in the cue period. Similarly, the bottom panel shows comparison between cued distractor location and un-cued distractor location at alpha .05 level. We see better reconstruction in the cued distractor location compared to un-cued distractor location (un-cued stimulus location in the target cue condition) around 400ms in the cue period.

![image](https://github.com/user-attachments/assets/f4686f10-b2d0-4631-adf1-67fcc530258d)

In the top panel of this image above, we see that there is greater selectivity in the cued target location compared the un-cued distractor location in the target cue condition, around 200ms and 400 ms during the cue period.  However, in the distractor cue condition, there is very little difference between cued distractor and un-cued stimulus locations, suggesting perhaps a diffused attention state or attend away mechanism (bottom panel).

Overall we see that there is reconstruction of the cued target location, suggesting this information is carried in alpha band activity. This is some evidence that the distractor location is represented in the alpha band activity, but little evidence that the representation is inhibited. Both of the analyses above were conducted in alpha band activity. Analyses of theta band activity showed greated representation of distractors but still no evidence of inhibition of the distractor location as a function of cueing. 

Together the behavioral and EEG data show that compared to no preview or cueing of location, target cueing is most effective in reducing distraction, followed by distractor cueing. However, we don't see any evidence that distractor cueing is a result of inhibition at the cued location. It is more likely that an indication to "ignore left" is converted to a signal to "attend right" which leads to facilitation of likely target locaiton. Another similar possibility is the cueing of a distractor location involves a "tag and avoid" where the distractor location is processed to some extent and avoided during search. 







