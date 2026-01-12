# Group11_Final-Project Explore the Geospatial relationship between and Child Stunting in Rwanda

This project develops a secure, internal Small Area Estimation (SAE) framework for analyzing the relationship between geographic elevation and child stunting prevalence across Rwanda's administrative sectors. The system integrates confidential Demographic and Health Survey (DHS) data with Census information within NISR's secure environment to produce sector-level estimates that inform nutrition and poverty reduction policies. The workflow includes data harmonization, elevation extraction, SAE modeling, and visualization—all designed to operate without sharing sensitive individual-level data externally.

# All raw data files (DHS, Census) remain exclusively within NISR's secure servers. 
## The repository contains only:
•	Code for processing
•	Synthetic/simulated data for development/testing
•	Aggregated results (sector-level only, no individual records)
•	Documentation and configuration files

# Directory Structure
text
rwanda-sae-nisr/                      # MAIN PROJECT DIRECTORY
│
├── 🔒 SECURE_ZONE/                    # ⚠️ NISR INTERNAL ONLY ⚠️
│   │   (Contains actual confidential data - NOT in this repository)
│   ├── /nisr_server/dhs_data/        # Actual DHS .DTA files
│   │   ├── RWKR81FL.DTA              # Children's Recode (stunting)
│   │   ├── RWIR81FL.DTA              # Individual Recode
│   │   └── RWHR81FL.DTA              # Household Recode
│   │
│   ├── /nisr_server/census_data/     # Rwanda Census 2012
│   │   └── PHC5_2012/                # Census with coordinates
│   │
│   └── /nisr_server/results/         # Final outputs for internal use
│
├── code/                             # ✅ PUBLIC: All code (no sensitive data)
│   │
│   ├── 1_data_processing/           # Data preparation modules
│   │   ├── dhs_processor.py         # Loads & processes DHS data
│   │   │   └── Key Function: process_dhs_children_stunting()
│   │   │       • Reads DHS .DTA files using pyreadstat
│   │   │       • Calculates HAZ scores and stunting classifications
│   │   │       • Outputs: child-level stunting indicators
│   │   │
│   │   ├── census_harmonizer.py     # Harmonizes DHS-Census variables
│   │   │   └── Key Function: harmonize_datasets()
│   │   │       • Matches sector IDs between datasets
│   │   │       • Creates common variable definitions
│   │   │       • Outputs: harmonized sector-level file
│   │   │
│   │   ├── elevation_extractor.py   # Geographic elevation processing
│   │   │   └── Key Function: extract_elevation_data()
│   │   │       • Uses sector coordinates from Census
│   │   │       • Extracts elevation from SRTM/Google API
│   │   │       • Outputs: sector_elevation.csv
│   │   │
│   │   └── sector_aggregator.py     # Creates analysis datasets
│   │       └── Key Function: create_sector_level_analysis()
│   │           • Aggregates child-level to sector-level
│   │           • Adds socioeconomic covariates
│   │           • Outputs: sae_analysis_dataset.csv
│   │
│   ├── 2_sae_models/                # Statistical modeling
│   │   ├── sae_fay_herriot.py       # Fay-Herriot SAE implementation
│   │   │   └── Key Function: fit_fay_herriot_model()
│   │   │       • Implements area-level SAE model
│   │   │       • Incorporates elevation as covariate
│   │   │       • Produces sector-level predictions with SE
│   │   │
│   │   ├── model_validation.py      # Cross-validation & diagnostics
│   │   │   └── Key Function: cross_validate_sae()
│   │   │       • Leave-one-out cross-validation
│   │   │       • Calculates RMSE, bias metrics
│   │   │       • Outputs: validation_report.pdf
│   │   │
│   │   └── uncertainty_estimation.py # Error propagation
│   │
│   ├── 3_visualization/             # Mapping and charts
│   │   ├── sector_maps.py           # Spatial visualization
│   │   │   └── Key Function: create_stunting_elevation_map()
│   │   │       • Choropleth maps of Rwanda sectors
│   │   │       • Overlays stunting rates and elevation
│   │   │       • Outputs: interactive HTML maps
│   │   │
│   │   ├── results_dashboard.py     # Interactive dashboard
│   │   │   └── Key Function: launch_dashboard()
│   │   │       • Streamlit app for exploring results
│   │   │       • Sector comparison tools
│   │   │       • Model diagnostics display
│   │   │
│   │   └── report_generator.py      # Automated reporting
│   │
│   ├── 4_utilities/                 # Helper functions
│   │   ├── config_manager.py        # Manages paths and parameters
│   │   ├── data_validation.py       # Quality checks
│   │   └── logging_setup.py         # Audit logging
│   │
│   └── run_pipeline.py              # ⭐ MAIN EXECUTION SCRIPT ⭐
│       • Orchestrates entire workflow
│       • Calls modules in correct sequence
│       • Handles errors and logging
│
├── config/                          # Configuration files
│   ├── paths.json                   # File path definitions
│   │   └── Example structure:
│   │       {
│   │         "raw_dhs": "/nisr_server/dhs_data/RWKR81FL.DTA",
│   │         "output_dir": "/nisr_server/results/2024_sae",
│   │         "shapefiles": "./data/external/rwanda_sectors.shp"
│   │       }
│   │
│   ├── model_parameters.yaml        # SAE model settings
│   │   └── Example:
│   │       fay_herriot:
│   │         covariates: ["elevation", "wealth_index", "urban"]
│   │         variance_method: "reml"
│   │         confidence_level: 0.95
│   │
│   └── variables_mapping.csv        # DHS-Census variable matching
│
├── data/                            # ✅ PUBLIC: Synthetic/test data only
│   ├── synthetic/                   # Simulated data for development
│   │   ├── sample_dhs_children.csv  # Fake child records
│   │   ├── sample_sector_data.csv   # Fake sector aggregates
│   │   └── README_SYNTHETIC.md      # ⚠️ CLEARLY MARKS AS FAKE
│   │
│   ├── external/                    # Publicly available data
│   │   ├── rwanda_admin_boundaries/ # Shapefiles from GADM
│   │   ├── srtm_elevation/          # Public elevation tiles
│   │   └── metadata/                # Public data documentation
│   │
│   └── processed/                   # Will contain processed outputs
│       └── (Initially empty - filled during processing)
│
├── outputs/                         # Generated results
│   ├── models/                      # Saved model objects
│   ├── predictions/                 # Sector-level estimates
│   │   └── Format: sector_id, stunting_rate, se, lower_ci, upper_ci
│   │
│   ├── reports/                     # Analysis reports
│   │   ├── technical_report.pdf     # Full methodology
│   │   ├── policy_brief.pdf         # Non-technical summary
│   │   └── validation_results/      # Cross-validation outputs
│   │
│   └── visualizations/              # Maps and charts
│       ├── maps/                    # Interactive HTML maps
│       ├── charts/                  .png charts for reports
│       └── dashboard/               # Dashboard assets
│
├── docs/                            # Documentation
│   ├── methodology/                 # Technical documentation
│   │   ├── sae_methodology.md       # SAE theory & implementation
│   │   ├── data_harmonization.md    # DHS-Census matching
│   │   └── elevation_processing.md  # GIS methods
│   │
│   ├── user_guides/                 # How-to guides
│   │   ├── setup_guide.md           # Environment setup
│   │   ├── running_analysis.md      # Step-by-step execution
│   │   └── interpreting_results.md  # Understanding outputs
│   │
│   ├── api_reference/               # Code documentation
│   │   └── (Auto-generated from docstrings)
│   │
│   └── data_protocol/               # ⚠️ CRITICAL: Data security
│       ├── data_handling_protocol.md # How to handle NISR data
│       ├── output_disclosure_rules.md # What can be shared
│       └── audit_logging_requirements.md
│
├── tests/                           # Unit and integration tests
│   ├── test_data_processing.py      # Tests on synthetic data
│   ├── test_models.py               # Model validation tests
│   └── test_integration.py          # End-to-end workflow tests
│
├── environment/                     # Development setup
│   ├── environment.yml              # Conda environment
│   ├── requirements.txt             # pip requirements
│   └── Dockerfile                   # Containerization
│
└── deployment/                      # Production deployment
    ├── nisr_server_setup/           # NISR server configuration
    ├── scheduled_jobs/              # Cron jobs for updates
    └── monitoring/                  # Performance monitoring

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





