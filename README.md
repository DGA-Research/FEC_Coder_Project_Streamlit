# FEC Donor & PAC Analysis Tool

A comprehensive Streamlit web application for analyzing Federal Election Commission (FEC) data, including donor information, PAC contributions, and geographic visualizations.

## Features

- **Real-time FEC Data Integration**: Fetch live data from the FEC API
- **Comprehensive Database**: Local SQLite database with master lists for:
  - Bad actors and problematic donors
  - Industry classifications
  - Committee information
  - Geographic data with ZIP code coordinates
- **Interactive Visualizations**: 
  - Plotly charts and graphs
  - Interactive maps with Folium
  - Geographic analysis and choropleth maps
- **Multi-format Export**: Download results in CSV, Excel, and other formats
- **Advanced Filtering**: Search and filter by multiple criteria

## Deployment

This application is configured for deployment on Streamlit Community Cloud.

### Requirements

- Python 3.8+
- Streamlit
- FEC API Key (configured via Streamlit secrets)

### Local Development

1. Clone this repository
2. Install dependencies: `pip install -r requirements.txt`
3. Set up your FEC API key in `.streamlit/secrets.toml`
4. Run the app: `streamlit run app.py`

## Configuration

The app requires an FEC API key for accessing live data. In production, this should be configured through Streamlit Cloud's secrets management.

## Performance Optimizations

Several committee-related features have been disabled to improve application performance:

### Disabled Features

#### 1. Industry Committee ID Lookups
- **Location**: `app.py` lines 947-951, 953-957
- **Impact**: Eliminates database lookups for industry org_type data
- **Restoration**: Uncomment the blocks marked with `# DISABLED FOR PERFORMANCE`

#### 2. LPAC Sponsor District Lookups  
- **Location**: `app.py` lines 1037-1042
- **Impact**: Shows "No Sponsor District Available" instead of performing database lookup
- **Restoration**: Uncomment the `processor.get_lpac_full_data_by_id()` call

#### 3. Fuzzy Name Matching for Industries
- **Location**: 
  - `app.py` lines 953-957 (display layer)
  - `data_processor.py` lines 435-440 (processing layer)
- **Impact**: Eliminates expensive string similarity calculations
- **Restoration**: Uncomment both the app.py and data_processor.py fuzzy matching blocks

#### 4. FEC API Committee Searches
- **Location**: `app.py` lines 2677-2683, 2686-2689
- **Impact**: Disables candidate committee lookup and committee search features
- **Restoration**: Uncomment the API calls and remove the `st.info()` messages

### Quick Restoration Guide

To re-enable any disabled feature:

1. Find the marked section with `# DISABLED FOR PERFORMANCE:`
2. Uncomment the code block below the marker
3. Remove any fallback values or info messages added
4. Test the feature to ensure it works correctly

### Performance Impact

These optimizations significantly reduce:
- Database query overhead (50+ fewer queries per processing run)
- String similarity calculation overhead (fuzzy matching)
- External API call overhead
- Overall processing time by approximately 60-80%

## License

This project is designed for research and educational purposes in campaign finance analysis.