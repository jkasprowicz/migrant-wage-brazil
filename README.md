# Migrant Workers' Admission Wages in Brazil, 2011–2025

This repository contains the reproducible data-processing and statistical-analysis workflow for a study of the distribution of admission wages among migrant workers in Brazil's formal labor market between 2011 and 2025.

The analysis uses administrative records from the Brazilian General Register of Employed and Unemployed Workers (CAGED) and Novo CAGED, made available through the Observatório das Migrações Internacionais (OBMigra).

## Study design

The study is an observational retrospective analysis in which the unit of analysis is a registered admission. Because the available records do not contain a persistent worker identifier, observations represent employment admissions and not necessarily unique individuals.

The original consolidated database contained 4,037,733 records covering 2011–2025.

The analytical workflow accounts for changes in data structure and coding associated with the transition from CAGED to Novo CAGED in 2020.

## Analytical population

The construction of the analytical population followed a predefined sequence:

| Processing step | Records |
|---|---:|
| Original consolidated database | 4,037,733 |
| Admissions identified | 2,194,465 |
| After excluding records classified as “NATURALIDADE BRASILEIRA” | 1,997,801 |
| After excluding identifiable transfers | 1,957,531 |
| After excluding intermittent employment | 1,939,085 |
| Wage-eligible admissions | 1,936,104 |
| Complete cases used in the final GAMLSS models | 1,914,957 |

The outcome used in the GAMLSS models was the nominal monthly admission wage expressed in Brazilian reais (BRL). Wages were not deflated by a consumer-price index, and wages expressed as multiples of the minimum wage were not used as the model outcome.

The Brazilian minimum wage applicable to each monthly reference period was used exclusively to define wage eligibility. Records were considered eligible when the admission wage ranged from 0.3 to 150 times the minimum wage applicable to the corresponding reference month. These limits were used to exclude extremely low or exceptionally high values potentially reflecting implausible or inconsistent records while preserving the heterogeneity of the wage distribution.

## Variables

The outcome variable was the nominal monthly admission wage expressed in Brazilian reais.

The multivariable distributional models included:

- harmonized sex;
- educational attainment;
- harmonized age group;
- major occupational group derived from the Brazilian Classification of Occupations (CBO).

Sex and age-group categories were harmonized across the historical CAGED and Novo CAGED coding systems.

Detailed CBO codes were aggregated to major occupational groups using the first digit of the occupational code.

## Statistical analysis

Because admission wages are continuous, positive, right-skewed, and characterized by a long upper tail, distributional regression was performed using Generalized Additive Models for Location, Scale and Shape (GAMLSS).

Candidate distributions evaluated during model development included:

- Lognormal;
- Gamma;
- Generalized Gamma;
- Weibull;
- Box-Cox Cole-Green (BCCG);
- Box-Cox t (BCT);
- Box-Cox Power Exponential (BCPE).

Distribution-family screening was performed on a reproducible random sample of 100,000 observations using a fixed random seed. Candidate families were compared according to numerical convergence, global deviance, Generalized Akaike Information Criterion (GAIC), Schwarz Bayesian Criterion (SBC/BIC), and normalized quantile residual diagnostics.

The BCT distribution was selected for subsequent multivariable modeling because it provided the best overall combination of convergence, information criteria, and residual adequacy among the evaluated candidate families.

The hierarchical GAMLSS models were then fitted to the same analytical population of 1,914,957 complete observations. The fitted specifications were:

- M0: μ ~ 1 and σ ~ 1;
- M1: μ ~ sex + education + age group and σ ~ 1;
- M2: μ ~ sex + education + age group and σ ~ sex + education;
- M3: μ ~ sex + education + age group + major CBO occupational group and σ ~ sex + education.

The BCT shape parameters ν and τ were estimated as intercept-only parameters in all four models.

Models were compared using global deviance, GAIC, SBC/BIC, likelihood-ratio tests for nested models, and normalized quantile residual diagnostics. Statistical significance was not used as the sole criterion for model selection because of the large analytical sample.

Calendar year was not included as a covariate in models M0–M3. Monthly reference period and calendar year were used during data harmonization, admission identification, and assignment of the applicable minimum wage. Accordingly, model estimates represent associations in the pooled set of admissions from 2011 to 2025 and should not be interpreted as real-wage changes, purchasing-power effects, or temporal effects.

Associations are interpreted as statistical associations and not as causal effects.

## Repository structure

`notebooks/01_data_audit_and_harmonization.ipynb`  
Historical inspection of CAGED/Novo CAGED structure, coding changes, and data-quality issues. This notebook documents exploratory and harmonization decisions.

`notebooks/02_exploratory_salary_analysis.ipynb`  
Exploratory analyses of wages and available sociodemographic and occupational variables. This notebook documents analyses performed during study development and is not, by itself, the final analytical pipeline.

`notebooks/03_build_salary_analysis_dataset.ipynb`  
Reproducible construction of the salary-analysis dataset from the consolidated CAGED source file, including the final inclusion, exclusion, harmonization, and wage-eligibility rules.

`notebooks/04_distribution_selection_gamlss.ipynb`  
Reproducible comparison of candidate probability distributions on a fixed random sample of 100,000 admissions, including convergence assessment, deviance, GAIC, SBC, and normalized quantile residual diagnostics.

`notebooks/05_gamlss_distributional_modeling.ipynb`  
Final GAMLSS workflow using the selected BCT distribution, including complete-case construction, hierarchical fitting of models M0–M3, model comparison, and residual diagnostics.

## Reproducibility

The original source dataset is not stored in this GitHub repository.

To reproduce the analysis:

1. Obtain the corresponding CAGED/Novo CAGED data from the original data provider.
2. Reconstruct or place the consolidated source file according to the documentation in `data/README.md`.
3. Run the notebooks in numerical order.
4. Notebook 03 generates the processed salary-analysis dataset.
5. Notebook 04 reproduces the candidate-distribution comparison and BCT selection.
6. Notebook 05 reproduces the final M0–M3 GAMLSS models and residual diagnostics.

Software dependencies and package versions are listed in `requirements.txt`.

Random procedures use a fixed random seed where applicable.

## Data availability

The raw administrative records remain attributable to their original data provider and are not redistributed through this repository.

A processed analytical dataset used in the study is planned to be archived separately in Zenodo, subject to verification of the applicable redistribution conditions and disclosure considerations.

Once available, the dataset DOI will be provided here:

**Processed dataset:** [Zenodo DOI to be added]

The archived dataset will be accompanied by metadata, a data dictionary, processing documentation, and information linking it to the source data and this repository.

## Code availability

The analytical code is publicly available in this repository.

A version of the repository corresponding to the published analysis will also be archived in Zenodo to provide a persistent, citable snapshot.

**Code archive DOI:** [Zenodo DOI to be added]

## Data provenance

Data source:

Observatório das Migrações Internacionais (OBMigra) / CAGED and Novo CAGED administrative records, Brazil, 2011–2025.

Users wishing to reproduce the complete workflow should obtain source data from the original provider rather than treating the processed analytical dataset as a replacement for the original administrative records.

## Limitations relevant to reproducibility

The transition from CAGED to Novo CAGED introduced changes in record structure and variable coding over time. Harmonization decisions are therefore explicitly documented in the preprocessing workflow.

The unit of analysis is a registered admission rather than a unique worker.

Admission wages are modeled in nominal Brazilian reais. Calendar year is not included as a covariate in the final M0–M3 models; therefore, estimates should not be interpreted as temporal effects or changes in real purchasing power across 2011–2025.

## Citation

If using the code or processed data, please cite the corresponding repository release and/or Zenodo record.

A machine-readable citation file (`CITATION.cff`) will be maintained in the repository.

## License

Code licensing and data licensing are documented separately because redistribution rights for source or derived administrative data may differ from the license applied to analytical code.
