# Acute Stress Reduction Analysis: Music vs. Podcast Intervention

R syntax for analyzing cortisol data from an experiment comparing stress reduction between music (experimental group) and podcast (control group) interventions over 25 minutes.

## Key Analysis Features

- **Cortisol Unit Conversion**: Values converted from nmol/L to µg/dL using factor 27.7 for clinical interpretation[1][3]
- **Nonparametric Methods**:
  - `nparLD` package for factorial repeated measures ANOVA
  - Wilcoxon signed-rank tests for within-group effects
  - Effect size calculation with BCa bootstrap CIs
- **Visualization**:
  - Combined boxplot + scatter plots for pre/post comparisons
  - Group-specific cortisol level trajectories
- **Reproducibility**:
  - Session info tracking
  - Automated data transformation pipelines

## How to Use

### Requirements
```{r}
install.packages(c("readxl", "haven", "ggplot2", "nparLD", "rstatix"))
```

### Basic Workflow
```{r}
#Load data
data <- read_excel("~/path.xlsx")

Convert cortisol units
data$Cortpre <- data$Cortpre * 27.7 # nmol/L → µg/dL
data$Cortpos <- data$Cortpos * 27.7

Run nonparametric ANOVA
mod_1x1 <- nparLD(Cort ~ Time*Group, data = long_data, subject = ID)

Calculate within-group effects
wilcox_effsize(values ~ ind, data = data_wilcox_exp, paired = TRUE)
```

### Data preparation: Long Format Requirement

For non parametric repeated measures analyses in R (such as with the nparLD package), the data must be in long format. In this format, each row represents a single observation for each participant at each time point, rather than having separate columns for each measurement.

Example:

| ID | Group        | Time   | Cort |
|----|--------------|--------|------|
| 1  | Experimental | Before | 5.2  |
| 1  | Experimental | After  | 4.1  |
| 2  | Control      | Before | 6.0  |
| 2  | Control      | After  | 5.8  |

## Key Findings
- **No significant group × time interaction** in factorial ANOVA (`p = 0.82`)
- **Large within-group effect** in experimental group (r = 0.62)
- **Moderate control group effect** (r = 0.41)

## Repository Structure


** dissertation_acute.Rmd # Main analysis script
** Dados_Agudo.xlsx # Raw cortisol measurements
** Figures/ # Generated boxplot visuals
** dados_longo.xlsx # Transformed long-format data

Sales, I. & Pedrosa, F.G. (2024). Acute stress reduction through auditory interventions:
A psychobiological analysis using salivary cortisol. University of Minas Gerais. https://github.com/FredPedrosa/salivary_cortisol/

## Author of experiment
**Isabela Sales**  
Ph.D., Neuroscience ,
isabela_nutricionista@yahoo.com
Neroscience Program, UFMG  

**Advisor and data analyst**: 
Ph.D., Prof. Frederico G. Pedrosa  
fredericopedrosa@ufmg.br

[CC-BY-NC 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/)


