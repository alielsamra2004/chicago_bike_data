# Chicago Divvy Bike Share - Data Acquisition & Quality Analysis

**Student:** Ali El Samra - 19373  
**Course:** Emerging Topics - Data Analysis Tools & Methods  
**Dataset:** Chicago Divvy Bike Share System (2023)  
**Analysis Period:** Full Year 2023

---

## 📋 Project Overview

This project analyzes the Chicago Divvy bike-sharing system using real-time and historical data. The analysis focuses on data acquisition, quality assessment, and comprehensive cleaning to prepare the dataset for spatial and temporal analysis.

---

## 📊 Dataset Information

### **Real-Time Data (CityBikes API)**
- **Source:** CityBikes Network API
- **Endpoint:** `http://api.citybik.es/v2/networks/divvy`
- **Data Collected:** 1,919 bike stations
- **Fields:** Station ID, Name, Latitude, Longitude, Free Bikes, Empty Slots, Timestamp
- **File:** `chicago_bikes_status.csv`

### **Historical Data (Divvy Trip Data)**
- **Source:** Divvy Trip History (2023)
- **Initial Records:** 5,719,877 trips
- **Final Cleaned Records:** 4,244,172 trips (74.2% retention)
- **Fields:** Trip ID, Start/End Time, Start/End Station ID, Duration, Rideable Type, Member Type
- **File:** `divvy_trips_final_cleaned_2023.csv`

---

## 🔧 Data Acquisition Process

### **Part 1: Real-Time Station Data**
1. Connected to CityBikes API
2. Retrieved current station status for all 1,919 Divvy stations
3. Extracted geographic coordinates and availability metrics
4. Saved to CSV for spatial analysis

### **Part 2: Historical Trip Data**
1. Downloaded 12 monthly CSV files (January - December 2023)
2. Merged into single comprehensive dataset
3. Validated data integrity across all months
4. Initial dataset: 5,719,877 trips

---

## 🧹 Data Quality & Cleaning Process

### **1. Missing Values (24.27% removed)**
**Decision:** DROP rows with missing station IDs

**Reasoning:**
- 1,388,054 trips missing start_station_id or end_station_id
- Systematic pattern (21-25% across all months) - not random
- Station location critical for spatial analysis
- No reliable imputation method for 1,919 possible stations
- Better 75% accurate data than 100% fabricated data

**Impact:** Reduced to 4,331,823 trips with guaranteed valid locations

---

### **2. Duration Outliers (1.53% removed)**
**Removed:**
- **False Starts (< 60s):** 86,949 trips - User errors, not actual usage
- **Unreturned Bikes (> 24h):** 133 trips - Stolen bikes or system failures

**Kept:**
- **Long Trips (1-24h):** Legitimate all-day rentals by tourists/events
- Represents real revenue-generating behavior

**Percentile Analysis:**
- 1st: 23s | 5th: 142s | 95th: 2,596s | 99th: 5,972s

**Impact:** Removed 87,651 trips based on business logic, not blind statistics

---

### **3. Spatial Outliers (0 removed)**
**Methods Used:**
1. **Convex Hull** - Geographic boundary detection
2. **DBSCAN Clustering** - Density-based isolation (eps=0.015°, min_samples=3)
3. **K-Nearest Neighbors** - Distance to 5 nearest neighbors
4. **Mahalanobis Distance** - Statistical correlation analysis

**Voting System:** Each method votes, outlier score = sum of flags (0-4)

**Results:**
- Score 0: 1,821 stations (94.9%)
- Score 1: 95 stations (5.0%)
- Score 2+: 2 stations (0.1%) - Big Marsh Park, Lincolnwood Dr & Central St

**Decision:** KEPT all stations including isolated ones

**Reasoning:**
- Geographic isolation ≠ invalid data
- Legitimate Divvy service points serving edge areas
- Only remove zero/null coordinates (none found)
- "Outlier" doesn't automatically mean "error"

---

### **4. Temporal Anomalies (0 removed)**
**Checks Performed:**
- Future timestamps (after current date): 0 found
- Pre-launch trips (before June 28, 2013): 0 found
- Duplicate trip IDs: 0 found

**Result:** Excellent data quality confirmed

---

## 📈 Final Dataset Statistics

| Metric | Value |
|:-------|:------|
| **Initial Trips** | 5,719,877 |
| **Final Trips** | 4,244,172 |
| **Retention Rate** | 74.2% |
| **Total Removed** | 1,475,705 (25.80%) |
| **Stations** | 1,919 (all retained) |
| **Mean Duration** | 16.2 minutes |
| **Median Duration** | 10.0 minutes |
| **Date Range** | Jan 1, 2023 - Dec 31, 2023 |

### **Removal Breakdown:**
- Missing Values: 24.27%
- Duration Outliers: 1.53%
- Spatial/Temporal: 0%

---

## 🎯 Critical Thinking & Decision-Making Principles

1. **Preserve Real Business Operations** - Keep unusual but legitimate data
2. **Multiple Validation Methods** - Use 4 spatial methods for cross-validation
3. **Accuracy Over Quantity** - Better to have less data that's reliable
4. **Business Context Matters** - Statistical outliers ≠ data errors
5. **Systematic Analysis** - Pattern recognition before decision-making

---

## 📁 Project Files

### **Main Notebook**
- `data_acquisition_quality.ipynb` - Complete analysis workflow

### **Output Files**
- `chicago_bikes_status.csv` - Real-time station data (1,919 stations)
- `divvy_trips_final_cleaned_2023.csv` - Cleaned trip data (4.2M trips)
- `DATA_CLEANING_LOG.txt` - Detailed cleaning decisions and reasoning

### **Original Data**
- `divvy_trips_full_2023.csv` - Original unprocessed trip data

---

## 🔬 Methods & Technologies

**Programming:** Python 3.x  
**Libraries:** pandas, numpy, requests, scipy, scikit-learn  
**APIs:** CityBikes Network API  
**Analysis:** Jupyter Notebook  

**Advanced Techniques:**
- Multi-method spatial outlier detection
- Percentile-based duration analysis
- Systematic pattern recognition
- Business-driven decision making

---

## 📝 Key Insights

1. **Data Quality:** Divvy's data collection is excellent (zero temporal anomalies)
2. **Missing Data Pattern:** Systematic 21-25% missing station IDs (likely dockless bikes)
3. **Spatial Distribution:** 2 isolated but legitimate edge stations identified
4. **Usage Patterns:** 99% of trips are under 100 minutes
5. **Network Coverage:** 1,919 stations across Chicago metropolitan area

---

## 🚀 Next Steps

This cleaned dataset is ready for:
- Spatial analysis (station usage patterns, hot spots)
- Temporal analysis (peak hours, seasonal trends)
- Customer behavior analysis (member vs. casual riders)
- Route optimization (popular station pairs)
- Demand forecasting

---

## 📧 Contact

**Ali El Samra**  
Student ID: 19373  
Course: Emerging Topics - Data Analysis Tools & Methods

---

*Last Updated: January 2026*
