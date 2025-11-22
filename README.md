# U.S.-Airport-Fleet-Evolution-ETL


## Overview
This is a production-grade data engineering and analytics pipeline that processes 10+ years of U.S. DOT T-100 Segment data (~150M rows) to answer the #1 question airlines and airport planners pay millions to solve:

> “Which airports are quietly switching from 50-seat regional jets to 190-seat A321s — and will therefore hit gate/runway capacity limits in the next 3–5 years?”

The pipeline calculates, for every large U.S. airport and every year:
- Exact % of departures performed by each aircraft type (e.g., “In 2022, 737-900ER = 41.2% of all departures at PHX”)
- Year-over-year change and linear regression forecasts to 2028
- Full hierarchical passenger/freight/mail flows: Airport → Distance Band → O-D City Pair → Carrier
- Passenger experience benchmarking using scraped airport amenity data (restaurants, lounges, ratings)

Outputs are clean, nested JSON files optimized for instant consumption in Power BI, Tableau, or Streamlit dashboards — no extra transformation needed.

This is the same type of fleet intelligence used by American Airlines Network Planning, Delta TechOps, Boeing Commercial, and airport master-planning consultants.

## Key Features
- **YoY Fleet Mix Evolution Engine** – Tracks precise % share of every aircraft type at every airport (2012–2022)
- **Linear Regression Forecasting (2024–2028)** – Predicts when an aircraft type will exceed 50% share (capacity red flag)
- **Hierarchical Passenger Flow Aggregation** – Airport → Distance Group → O-D Link → Carrier with pre-calculated % shares
- **Amenity & Passenger Experience Benchmarking** – Aggregates restaurant/lounge/shop counts and Google-star ratings across competing airports
- **60–80% faster querying** than raw CSVs thanks to normalized JSON structure

## Technologies
- Python 3.x (pandas, numpy, scikit-learn, plotly)
- U.S. DOT T-100 Domestic & International Segment data
- BTS reference tables (aircraft types, carriers, world area codes)
- GeoJSON / CSV  = normalized nested JSON ETL
- Linear regression 

## Setup & Usage

```bash
# 1. Clone
git clone https://github.com/DeanHak/us-airport-fleet-evolution-etl.git
cd us-airport-fleet-evolution-etl

# 2. Install requirements
pip install pandas numpy scikit-learn plotly openpyxl

# 3. Place your T-100 CSV files in
#    airports/01_data/   (e.g., 2022_T_T100_SEGMENT_ALL_CARRIER.csv)

# 4. Run the core pipelines (in order)

python airportFleetMixYoy.py                    # fleetMix_by_origin_2012-2022.json
python aircraftStats_allAirports_linReg.py      # adds 2024–2028 forecasts + plots
python selectAirportOperationalStats.py         # example: deep dive LAX (change selectAirport = "LAX")
python aggregateSelectAirportAmenities.py       # amenity benchmarking (set selectAirport)

# Output appears in airports/02_output/
