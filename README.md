# MCTI wastewater nature-based solution analysis

This project analyses flow and water-quality data from a sewage treatment works to inform the selection and sizing of a nature-based solution (NbS) for storm overflow. Flow above **25 L/s** is diverted after primary settlement and would be treated by the proposed NbS.

The analysis was prepared for the MCTI project brief and covers storm frequency, event size, water-quality behaviour, permit compliance and the treatment performance a NbS may need to provide.

## Key findings

- The flow record contains **1,449 daily observations** from 1 January 2020 to 19 December 2023.
- Flow exceeded the 25 L/s storm threshold on approximately **32% of days**, forming **106 storm events**.
- Storms are strongly seasonal: approximately **49% of days from October to March** are storm days, compared with approximately **15% from April to September**.
- A design flow of approximately **20 L/s above the 25 L/s threshold** would treat about **91% of recorded excess storm volume**.
- Storm-day concentrations are generally lower than no-storm concentrations, consistent with dilution, but treatment is still required.
- Suspended solids is the limiting parameter. At the 90th percentile of storm-day concentrations, the estimated central-case NbS removal requirements are approximately:

  | Parameter | Required removal |
  | --- | ---: |
  | Suspended solids (SS) | 49% |
  | Ammoniacal nitrogen (NH3N) | 32% |
  | BOD | 30% |
  | COD | 29% |

  These estimates increase if primary settlement performs poorly; SS may require approximately 59% removal and NH3N approximately 39%.

## Repository contents

| File or directory | Description |
| --- | --- |
| [`mcti_data_analysis.ipynb`](mcti_data_analysis.ipynb) | Main analysis notebook |
| [`mean_daily_flow_20_23.csv`](mean_daily_flow_20_23.csv) | Mean daily flow data for 2020–2023 |
| [`wq_data_20_24.csv`](wq_data_20_24.csv) | Water-quality sample results for 2020–2024 |
| [`requirements.txt`](requirements.txt) | Python dependencies |
| [`outputs/`](outputs/) | Cleaned data, merged datasets and generated figures |
| [`outputs/figures/`](outputs/figures/) | Figures produced by the notebook |

### Input data

The water-quality dataset contains four parameters measured in crude sewage:

- Suspended solids (SS)
- Five-day biochemical oxygen demand (BOD)
- Chemical oxygen demand (COD)
- Ammoniacal nitrogen as N (NH3N)

The permit limits used in the analysis are:

| Parameter | Limit |
| --- | ---: |
| SS | 65 mg/L |
| BOD | 100 mg/L |
| COD | 340 mg/L |
| NH3N | 20 mg/L |

## Method

The notebook:

1. Cleans and validates the flow and water-quality datasets.
2. Flags a **storm day** when mean daily flow is greater than 25 L/s.
3. Calculates excess flow as `max(0, mean flow - 25)` and converts it to daily excess volume.
4. Matches water-quality samples to flow observations by date.
5. Summarises storm frequency, flow-duration behaviour, storm-event duration, excess volume, and dry gaps.
6. Compares water quality on storm and no-storm days and examines concentration against flow.
7. Estimates the removal required to meet permit limits after primary settlement under conservative, central and optimistic assumptions.
8. Saves cleaned datasets and figures to `outputs/`.

## Reproducing the analysis

The project requires Python 3 and the packages listed in `requirements.txt`.

```bash
python -m venv .venv
```

Activate the environment:

```bash
# Windows PowerShell
.venv\Scripts\Activate.ps1

# macOS/Linux
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Open [`mcti_data_analysis.ipynb`](mcti_data_analysis.ipynb) in Jupyter or VS Code and run the cells from top to bottom. The notebook expects the two input CSV files to be in the project root and writes results to `outputs/`.

## Generated outputs

The notebook writes:

- `outputs/flow_clean.csv`
- `outputs/wq_clean.csv`
- `outputs/wq_flow_merged.csv`
- `outputs/wq_flow_merged_matched.csv`

Figures include:

- Storm frequency and flow-event distributions
- Flow frequency and design-flow coverage
- Water-quality comparisons and flow relationships
- Required removal heatmap

## Limitations

- Flow data is daily mean flow, so short within-day peaks are not represented.
- The flow record ends on 19 December 2023, so 2024 water-quality samples cannot be matched to flow.
- There are 60 water-quality sampling dates, but only 48 have matching flow data and only 11 are classified as storm samples.
- The 90th percentile is therefore based on a small storm sample and is effectively close to the second-highest observed storm concentration.
- Water-quality samples are grab samples from crude sewage before primary settlement, while the NbS would receive flow after primary settlement.
- Primary-settlement removal rates are literature-based assumptions rather than measurements from this site.
- The analysis supports option appraisal and preliminary sizing; site-specific sampling, hydraulic design, operational requirements, costs, and NbS performance evidence are still required before implementation.
