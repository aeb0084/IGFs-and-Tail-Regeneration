# Effects of Dietary Restriction and Insulin-Like Growth Factors on Brown Anole Tail Regeneration 
 
 Alexis Lindsey<sup>1</sup>, Abby Beatty<sup>1</sup>, Tonia S. Schwartz<sup>1,2</sup>
 
<sup>1</sup> Department of Biological Sciences, Auburn University, Auburn, AL 36849 

<sup>2</sup> Corresponding Author: tschwartz@auburn.edu 

This repository holds all supplemental files for "Effects of Dietary Restriction and Insulin-Like Growth Factors on Brown Anole Tail Regeneration".

DOI: TBD


## Abstract: 
> ""

### Quick Key to File Directory: Detailed descriptions of file use can be found below.
Analysis and File Names| Brief Description | File Download
-------------------------------------|------------------------------------ | -----------------------------------------------------
Weight Loss Analysis           | File used to determine effectiveness of restricted diet | [Weight Loss Analysis](WL.analysis.csv)
Final Longitudinal Data Set    | File used in statistical analysis to determine effect of IGF and diet on regeneration | [Final Longitudinal Data] (R.analysis.currated.csv)
Pre-Diet Body Size Analysis    | File used to ensure no biases were present during treatment allocation | [Pre-Diet Body Size](Pre.Diet.Measures.csv)
Tail Length Loss Analysis      | File used to assess effects of length loss during experimental period  | [Tail Length Loss](LengthLossAnalysis.csv)
R Markdown Statistical Code    | File containing all statistical code and figure production code        | [R Markdown Code](Regeneration.Diet.IGFs_Final_currated_data.Rmd)
R Markdown HTML Output         | Output file produced from R Markdown Statistical Code    | [R Markdown Output](Regeneration.Diet.IGFs_Final_currated_data.html)


## Supplementary Materials: 

<img src="Supplemental Fig. (2).png" width="1000">
Supplemental Figure 1: 


## Statistical Code:

EXAMPLE: The statistical analyses were performed in R (version 4.0.3) using the code file titled [R Markdown Code](Regeneration.Diet.IGFs_currated_data.Rmd). The code output displays all statistical models, results, and figures produced in [HTML](Regeneration.Diet.IGFs_Final_currated_data.html) format. Note, you will have to download the HTML file to visualize the data output. 

Examples of required packages, statistical models, and plots used can be seen below. Note: These are generalized examples produced for ease of adaptation.  

```{ruby}
#load all packages necessary to run statistical and graphic models
library(ggplot2)
library(nlme)
library(multcomp)
library(emmeans)
library(MuMIn)
library(bestNormalize)
library(lmtest)
library(car)
library(Rmisc)
library(dplyr)
library(viridis)
library(hrbrthemes)
library(plotly)
```

### Effect of Diet Restriction Statistical Analysis 
```{ruby}
diet.reg=subset(early_reg, Treatment == "DR" | Treatment == "AdLib")
anova(lme(Perc.Regeneration~ Treatment*Week, random=~1|Animal_ID , data=diet.reg, na.action=na.omit))
anova(lme(Eggs~ Treatment, random=~1|Animal_ID , data=diet.reg, na.action=na.omit))

#get week by week comparisons
w1=subset(diet.reg, Week == "1")
w2=subset(diet.reg, Week == "2")
w3=subset(diet.reg, Week == "3")
w4=subset(diet.reg, Week == "4")

anova(lm(Perc.Regeneration~ Treatment, data=w2, na.action=na.omit))
anova(lm(Perc.Regeneration~ Treatment, data=w3, na.action=na.omit))
anova(lm(Perc.Regeneration~ Treatment, data=w4, na.action=na.omit))

diet.reg.av=Rmisc::summarySE(data=diet.reg, measurevar="Perc.Regeneration", groupvars=c("Week", "Treatment"), na.rm=T, conf.interval=0.95)

diet.reg.pl=ggplot(diet.reg.av, aes(x=Week, y=Perc.Regeneration, color=Treatment, fill=Treatment)) +
    geom_area(position = "identity", alpha=0.02, size=2) +
    geom_jitter(data=diet.reg, alpha=0.9, size=0.5) +
      scale_color_manual(values = c("slategrey", "lemonchiffon2")) +
        scale_fill_manual(values = c("slategrey", "lemonchiffon2")) +
      ggtitle("Effect of Diet Restriction") +
    theme_ipsum() +
        theme(legend.position="top") 


diet.reg.pl


ggsave(diet.reg.pl, file="diet.reg.pl.png", height=6, width=4, dpi = 300)
```
### Effect of IGF Injection Statistical Analysis 
```{ruby}
inj.reg=subset(early_reg, Treatment != "AdLib")
anova(lme(Perc.Regeneration~ Treatment*Week, random=~1|Animal_ID , data=inj.reg, na.action=na.omit))

inj.reg.average=Rmisc::summarySE(data=inj.reg, measurevar="Perc.Regeneration", groupvars=c("Week","Treatment"), na.rm=T, conf.interval=0.95)


inj.reg.pl=ggplot(inj.reg.average, aes(x=Week, y=Perc.Regeneration, color=Treatment, fill=Treatment)) +
       geom_area(position = "identity", alpha=0.02, size=2 ) +
    geom_jitter(data=inj.reg, alpha=0.9, size=0.5) +
      scale_color_manual(values = c("lemonchiffon2", "thistle4", "mistyrose3")) +
        scale_fill_manual(values = c("lemonchiffon2", "thistle4", "mistyrose3")) +
    ggtitle("Effect of Supplemental IGF Injection") +
    theme_ipsum() +
        theme(legend.position="top",
              plot.title = element_text(size=14)) 


inj.reg.pl

ggsave(inj.reg.pl, file="inj.reg.pl.png", height=6, width=4, dpi = 300)
```
