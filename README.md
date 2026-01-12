# Group11_Final-Project Explore the Geospatial relationship between and Child Stunting in Rwanda

This project develops a secure, internal Small Area Estimation (SAE) framework for analyzing the relationship between geographic elevation and child stunting prevalence across Rwanda's administrative sectors. The system integrates confidential Demographic and Health Survey (DHS) data with Census information within NISR's secure environment to produce sector-level estimates that inform nutrition and poverty reduction policies. The workflow includes data harmonization, elevation extraction, SAE modeling, and visualization—all designed to operate without sharing sensitive individual-level data externally.

# All raw data files (DHS, Census) remain exclusively within NISR's secure servers. 
## The repository contains only:
•	Code for processing
•	Synthetic/simulated data for development/testing
•	Aggregated results (sector-level only, no individual records)
•	Documentation and configuration files

# Repository Structure Overview
Path	Type	Description	Security Level
🔒 SECURE_ZONE	Directory	NISR Internal Only (Not in repository)	🔴 Confidential
/nisr_server/dhs_data/	Data	Actual DHS .DTA files	🔴 Confidential
├── RWKR81FL.DTA	File	Children's Recode (stunting data)	🔴 Confidential
├── RWIR81FL.DTA	File	Individual Recode	🔴 Confidential
└── RWHR81FL.DTA	File	Household Recode	🔴 Confidential
/nisr_server/census_data/	Data	Rwanda Census 2012	🔴 Confidential
└── PHC5_2012/	Directory	Census with coordinates	🔴 Confidential
/nisr_server/results/	Output	Final outputs for internal use	🔴 Confidential
code/	Directory	✅ PUBLIC: All code (no sensitive data)	🟢 Public
1_data_processing/	Module	Data preparation modules	🟢 Public
├── dhs_processor.py	Script	Loads & processes DHS data	🟢 Public
│ └── process_dhs_children_stunting()	Function	Reads DHS .DTA files, calculates HAZ scores	🟢 Public
├── census_harmonizer.py	Script	Harmonizes DHS-Census variables	🟢 Public
│ └── harmonize_datasets()	Function	Matches sector IDs, creates common variables	🟢 Public
├── elevation_extractor.py	Script	Geographic elevation processing	🟢 Public
│ └── extract_elevation_data()	Function	Extracts elevation from coordinates	🟢 Public
└── sector_aggregator.py	Script	Creates analysis datasets	🟢 Public
└── create_sector_level_analysis()	Function	Aggregates to sector-level, adds covariates	🟢 Public
2_sae_models/	Module	Statistical modeling	🟢 Public
├── sae_fay_herriot.py	Script	Fay-Herriot SAE implementation	🟢 Public
│ └── fit_fay_herriot_model()	Function	Area-level SAE with elevation covariate	🟢 Public
├── model_validation.py	Script	Cross-validation & diagnostics	🟢 Public
│ └── cross_validate_sae()	Function	Leave-one-out CV, RMSE/bias metrics	🟢 Public
└── uncertainty_estimation.py	Script	Error propagation	🟢 Public
3_visualization/	Module	Mapping and charts	🟢 Public
├── sector_maps.py	Script	Spatial visualization	🟢 Public
│ └── create_stunting_elevation_map()	Function	Choropleth maps with stunting/elevation	🟢 Public
├── results_dashboard.py	Script	Interactive dashboard	🟢 Public
│ └── launch_dashboard()	Function	Streamlit app for exploring results	🟢 Public
└── report_generator.py	Script	Automated reporting	🟢 Public
4_utilities/	Module	Helper functions	🟢 Public
├── config_manager.py	Script	Manages paths and parameters	🟢 Public
├── data_validation.py	Script	Quality checks	🟢 Public
└── logging_setup.py	Script	Audit logging	🟢 Public
run_pipeline.py	Script	⭐ MAIN EXECUTION SCRIPT	🟢 Public
config/	Directory	Configuration files	🟡 Restricted
├── paths.json	Config	File path definitions (JSON)	🟡 Restricted
├── model_parameters.yaml	Config	SAE model settings (YAML)	🟢 Public
└── variables_mapping.csv	Config	DHS-Census variable matching	🟡 Restricted
data/	Directory	✅ PUBLIC: Synthetic/test data only	🟢 Public
synthetic/	Data	Simulated data for development	🟢 Public
├── sample_dhs_children.csv	File	Fake child records	🟢 Public
├── sample_sector_data.csv	File	Fake sector aggregates	🟢 Public
└── README_SYNTHETIC.md	Doc	Clearly marks as fake data	🟢 Public
external/	Data	Publicly available data	🟢 Public
├── rwanda_admin_boundaries/	Data	Shapefiles from GADM	🟢 Public
├── srtm_elevation/	Data	Public elevation tiles	🟢 Public
└── metadata/	Doc	Public data documentation	🟢 Public
processed/	Data	Will contain processed outputs	🟡 Restricted
outputs/	Directory	Generated results	🟡 Restricted
models/	Output	Saved model objects	🟡 Restricted
predictions/	Output	Sector-level estimates	🟡 Restricted
Format	Schema	sector_id, stunting_rate, se, lower_ci, upper_ci	🟡 Restricted
reports/	Output	Analysis reports	🟡 Restricted
├── technical_report.pdf	Report	Full methodology	🟡 Restricted
├── policy_brief.pdf	Report	Non-technical summary	🟢 Public
└── validation_results/	Output	Cross-validation outputs	🟡 Restricted
visualizations/	Output	Maps and charts	🟢 Public
├── maps/	Output	Interactive HTML maps	🟢 Public
├── charts/	Output	.png charts for reports	🟢 Public
└── dashboard/	Output	Dashboard assets	🟢 Public
docs/	Directory	Documentation	🟢 Public
methodology/	Docs	Technical documentation	🟢 Public
├── sae_methodology.md	Doc	SAE theory & implementation	🟢 Public
├── data_harmonization.md	Doc	DHS-Census matching	🟢 Public
└── elevation_processing.md	Doc	GIS methods	🟢 Public
user_guides/	Docs	How-to guides	🟢 Public
├── setup_guide.md	Doc	Environment setup	🟢 Public
├── running_analysis.md	Doc	Step-by-step execution	🟢 Public
└── interpreting_results.md	Doc	Understanding outputs	🟢 Public
api_reference/	Docs	Code documentation	🟢 Public
Auto-generated	Docs	From docstrings	🟢 Public
data_protocol/	Docs	⚠️ CRITICAL: Data security	🟢 Public
├── data_handling_protocol.md	Protocol	How to handle NISR data	🟢 Public
├── output_disclosure_rules.md	Protocol	What can be shared	🟢 Public
└── audit_logging_requirements.md	Protocol	Logging requirements	🟢 Public
tests/	Directory	Unit and integration tests	🟢 Public
├── test_data_processing.py	Tests	Tests on synthetic data	🟢 Public
├── test_models.py	Tests	Model validation tests	🟢 Public
└── test_integration.py	Tests	End-to-end workflow tests	🟢 Public
environment/	Directory	Development setup	🟢 Public
├── environment.yml	Config	Conda environment	🟢 Public
├── requirements.txt	Config	pip requirements	🟢 Public
└── Dockerfile	Config	Containerization	🟢 Public
deployment/	Directory	Production deployment	🟡 Restricted
├── nisr_server_setup/	Config	NISR server configuration	🔴 Confidential
├── scheduled_jobs/	Scripts	Cron jobs for updates	🟡 Restricted
└── monitoring/	Config	Performance monitoring	🟡 Restricted
 Security Level Legend
•	Confidential: NISR internal only, never shared
•	Restricted: Internal use, limited sharing after review
•	Public: Can be shared in repository


# KEY COMPONENTS LOCATION
## 1. Main Pipeline Script
•	Location: code/run_pipeline.py
•	Purpose: Single script to run entire analysis
•	Usage: python run_pipeline.py --config config/model_parameters.yaml
## 2. Data Processing (Core)
•	DHS Processing: code/1_data_processing/dhs_processor.py
•	Census Harmonization: code/1_data_processing/census_harmonizer.py
•	Elevation Extraction: code/1_data_processing/elevation_extractor.py
## 3. SAE Modeling
•	Fay-Herriot Model: code/2_sae_models/sae_fay_herriot.py
•	Model Validation: code/2_sae_models/model_validation.py
## 4. Visualization & Dashboard
•	Interactive Maps: code/3_visualization/sector_maps.py
•	Web Dashboard: code/3_visualization/results_dashboard.py
##  5. Configuration
•	Paths & Settings: config/paths.json
•	Model Parameters: config/model_parameters.yaml
##  6. Security Protocols
•	Data Handling: docs/data_protocol/data_handling_protocol.md
•	Output Rules: docs/data_protocol/output_disclosure_rules.md

# HOW TO RUN THE CODE
##  IMPORTANT SECURITY NOTE
Before running with real NISR data:
1.	Ensure you're on NISR's secure network
2.	Review docs/data_protocol/data_handling_protocol.md
3.	Use only the synthetic data for initial testing
4.	All outputs must follow NISR disclosure rules
##  Option A: Development Testing (Safe - Synthetic Data)
bash
# 1. Clone repository
git clone https://github.com/nisr-rwanda/sae-elevation-stunting.git
cd sae-elevation-stunting

# 2. Setup environment
conda env create -f environment/environment.yml
conda activate rwanda-sae

# 3. Test with synthetic data
python code/run_pipeline.py --test --synthetic

# Output: Process runs on fake data, generates sample outputs
## Option B: NISR Internal Execution (With Real Data)
bash
# 1. On NISR secure server, navigate to project
cd /nisr_projects/sae_elevation_stunting/

# 2. Update configuration for real data paths
# Edit config/paths.json to point to actual DHS/Census locations

# 3. Run full pipeline
python code/run_pipeline.py --config config/model_parameters.yaml

# 4. Launch interactive dashboard (internal network only)
streamlit run code/3_visualization/results_dashboard.py --server.port 8501

# Access at: http://nisr-internal-server:8501
##  Option C: Step-by-Step Execution
bash
# Process DHS data
python code/1_data_processing/dhs_processor.py \
  --input /nisr_server/dhs_data/RWKR81FL.DTA \
  --output ./data/processed/dhs_stunting.csv

# Extract elevation
python code/1_data_processing/elevation_extractor.py \
  --coordinates ./data/external/sector_coordinates.csv \
  --output ./data/processed/sector_elevation.csv

# Run SAE model
python code/2_sae_models/sae_fay_herriot.py \
  --data ./data/processed/sae_analysis_dataset.csv \
  --output ./outputs/predictions/sector_estimates.csv

# Generate maps
python code/3_visualization/sector_maps.py \
  --estimates ./outputs/predictions/sector_estimates.csv \
  --shapefile ./data/external/rwanda_sectors.shp \
  --output ./outputs/visualizations/stunting_map.html
##  Option D: Using Makefile (Simplified)
bash
make setup          # Install dependencies
make test-synthetic # Run with synthetic data
make process-data   # Process real data (NISR only)
make run-models     # Execute SAE models
make visualize      # Generate visualizations
make report         # Create PDF reports
make clean          # Remove outputs (preserving originals)
##  Option E: Docker Container (Isolated Environment)
bash
# Build Docker image
docker build -t rwanda-sae -f environment/Dockerfile .

# Run with volume mount for data
docker run -v /nisr_server/dhs_data:/data/dhs:ro \
           -v ./outputs:/app/outputs \
           rwanda-sae python code/run_pipeline.py

# Note: Data remains on NISR server, only code runs in container

QUICK START FOR NEW TEAM MEMBERS
1.	Clone repo and setup environment
2.	Review security protocols in docs/data_protocol/
3.	Run synthetic test to understand workflow
4.	Examine sample outputs in outputs/examples/
5.	When access granted: Update paths and run with real data
________________________________________
DATA SECURITY PROTOCOLS
What Stays in NISR Servers:
•	Original DHS .DTA files
•	Census microdata with coordinates
•	Any data with individual identifiers
•	Intermediate files with disaggregated data
What Can Be Shared (After Review):
•	Sector-level aggregated estimates
•	Model code and methodology
•	Synthetic test datasets
•	Visualization templates
•	Documentation
Output Validation Before Sharing:
bash
# Check outputs for disclosure risks
python code/4_utilities/disclosure_check.py \
  --file ./outputs/predictions/sector_estimates.csv \
  --min_cells 5  # Minimum cells per estimate



# TROUBLESHOOTING
##  Common Issues:
1.	"File not found" errors:
o	Check config/paths.json matches NISR server structure
o	Verify file permissions on secure server
2.	Memory issues with large DHS files:
o	Use pyreadstat.read_file_in_chunks()
o	Process in batches
3.	Elevation API failures:
o	Fallback to local SRTM data
o	Use simulated elevation as in demo notebook
4.	Model convergence problems:
o	Adjust parameters in config/model_parameters.yaml
o	Try simpler model specifications first
## SUPPORT
•	Technical issues: Open GitHub issue (code-only discussions)
•	Data access: Contact NISR Data Governance Committee
•	Methodology questions: Review docs/methodology/
•	Security concerns: Review docs/data_protocol/

# PROJECT TEAM MEMBERS
## Core Development Team
1.	Gaga Rukorera Didier - Project Lead 
o	Primary contact for methodology and overall project direction
o	Responsible for SAE model development and validation
o	Coordinates with NISR data governance committees
2.	SCOVIA Muhoza - Data Processing Specialist
o	Manages DHS data extraction and processing
o	Implements data harmonization between DHS and Census datasets
o	Responsible for data quality assurance and validation
3.	IRAGUHA Juvenal - Geospatial & Visualization Specialist
o	Handles elevation data extraction and spatial analysis
o	Develops choropleth maps and spatial visualizations
o	Manages geographic data integration and coordinate systems
4.	SHARANGABO Faustin - Software Development & Deployment
o	Implements code architecture and pipeline automation
o	Manages version control and collaborative development
o	Handles deployment to NISR secure servers
## Collaborating Partners
•	National Institute of Statistics of Rwanda (NISR) 
- Data Provider & Institutional Partner
•	Ministry of Health (MoH) 
- Policy Stakeholder & End User
## Contact Information
For code-related issues: Create an issue in the GitHub repository
For data access requests: Contact NISR Data Governance Committee
For methodological questions: didier.gagarukorera@statistics.gov.rw 

## Team Roles Mapping to Repository Structure:
•	Gaga Rukorera Didier: Oversees code/2_sae_models/ and docs/methodology/
•	SCOVIA Muhoza: Manages code/1_data_processing/ and data validation
•	IRAGUHA Juvenal: Develops code/3_visualization/ and spatial components
•	SHARANGABO Faustin: Maintains code/4_utilities/, environment/, and deployment/





