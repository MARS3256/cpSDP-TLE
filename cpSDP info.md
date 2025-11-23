## Grouped by Domain/Purpose

### 📦 Build & Dependency Management
- **Ant** - Build automation tool
- **Ivy** - Dependency management
- **Maven** - Dependency management support

### 🔌 Enterprise Integration & Messaging
- **Camel** - Integration framework and routing engine
- **Synapse** - ESB and service mediation

### 📝 Development Tools
- **JEdit** - Programmer's text editor
- **Log4j** - Logging framework

### 🗄️ Data Processing & Storage
- **Lucene** - Search engine library
- **POI** - Office document processing

### 🔄 Content Transformation & Templating
- **Velocity** - Template engine
- **Xalan** - XSLT processor

### 📄 XML Processing
- **Xerces** - XML parser

### 🏢 Enterprise/Backend Infrastructure
- **Lucene** - Search functionality

## Dimensionality Reduction Strategy

Available Features in Dataset:  
name, wmc, dit, noc, cbo, rfc, lcom, ca, ce, npm, lcom3, loc, dam, moa, mfa, cam, ic, cbm, amc, max_cc, avg_cc, bug

1. **Direct Removal:**  
    name - Class identifier, irrelevant for training

2. **Remove Redundant Features:**  
    lcom → Keep lcom3 (improved cohesion metric)  
    wmc → Keep max_cc (both measure complexity, highly correlated)

4. **Target:**  
    bug → Convert to binary (1 if > 0, else 0)

**Why NO PCA?**  
- Interpretability: OO metrics have established meanings in software engineering  
- Research Standard: CK metrics are standard in defect prediction research  
- Small Dataset: Only 19 features is already manageable  
- Information Loss: Each metric captures unique aspects  

AGGREGATED FEATURE ANALYSIS ACROSS ALL DATASETS
=
Consistently Redundant Feature Pairs (across multiple datasets):
--------------------------------------------------------------------------------
  ca           <-> cbo          : Found in 12/13 datasets
  rfc          <-> wmc          : Found in 12/13 datasets
  npm          <-> wmc          : Found in 10/13 datasets
  loc          <-> rfc          : Found in 6/13 datasets
  dit          <-> mfa          : Found in 6/13 datasets
  dam          <-> lcom3        : Found in 4/13 datasets
  lcom         <-> wmc          : Found in 3/13 datasets
  avg_cc       <-> max_cc       : Found in 3/13 datasets
  loc          <-> wmc          : Found in 2/13 datasets
  cbm          <-> ic           : Found in 2/13 datasets

Average Feature-Bug Correlation Across All 13 Datasets:
--------------------------------------------------------------------------------

Top 10 Predictive Features:
  rfc          :  0.487
  loc          :  0.439
  wmc          :  0.418
  ce           :  0.396
  npm          :  0.364
  lcom         :  0.353
  cbo          :  0.316
  moa          :  0.271
  max_cc       :  0.247
  ca           :  0.181

Bottom 10 Features (Weakest Predictors):
  dam          :  0.133
  avg_cc       :  0.132
  amc          :  0.126
  cbm          :  0.112
  ic           :  0.085
  noc          :  0.064
  dit          :  0.000
  mfa          : -0.013
  lcom3        : -0.090
  cam          : -0.233

RECOMMENDED FEATURES TO REMOVE
=========================================
Based on Redundancy (>0.85 correlation):
  Remove ca           (keep cbo         ) - Found redundant in 12 datasets
  Remove wmc          (keep rfc         ) - Found redundant in 12 datasets
  Remove npm          (keep wmc         ) - Found redundant in 10 datasets
  Remove loc          (keep rfc         ) - Found redundant in 6 datasets
  Remove dit          (keep mfa         ) - Found redundant in 6 datasets

Based on Weak Prediction Power (avg |correlation| with bug):
  Remove noc          : avg correlation =  0.064
  Remove mfa          : avg correlation = -0.013
  Remove lcom3        : avg correlation = -0.090
  Remove cam          : avg correlation = -0.233

  Remove name (identifier, not predictive)

--------------------------------------------------------------------------------
Total Features to Remove: 10
Original Features: 22 (21 + target)
After Removal: 12 features

Features to remove: ['name', 'ca', 'wmc', 'npm', 'loc', 'dit', 'noc', 'mfa', 'lcom3', 'cam']