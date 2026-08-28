# FDA AI/ML Medical Device Activity: Identifying Emerging Signals Beyond Radiology

> Analyzing FDA regulatory activity to identify recent and emerging product-code signals across clinical specialties
> 

<aside>

#### 💡 Project Overview

**Scope:** FDA AI/ML Medical Device Activity & Emerging Signal Analysis

**Data Sources:** FDA AI-Enabled Medical Devices Database & FOICLASS Product Code Master

**Dataset:** 1,524 FDA AI/ML device records

**Tools:** Python · Pandas · RegEx · Plotly

**KEY FINDING**

Radiology accounts for **76.7%** of records with an assigned medical specialty. Product Code-level analysis identified **QNP, OLZ, QYE, and SBF** as recent-emerging activity signals, which were subsequently validated through 2021–2025 activity trajectories.

**APPROACH**

Validated Product Code integrity and dataset joins, excluded incomplete 2026 data, compared recent activity across defined time windows, and validated emerging signals through multi-year trajectories.

**QUESTION** 

Beyond the dominant Radiology category, which Product Codes show meaningful recent FDA activity, and do those signals persist across multiple completed years?

**Links:** [[Google Colab Notebook](https://colab.research.google.com/drive/1cMLTit-60v9UJbse-iJIyx0EUE_iVF-1#scrollTo=-4OQN1u5aYQ1)] | [[[Notion](https://app.notion.com/p/FDA-AI-ML-Medical-Device-Activity-Identifying-Emerging-Signals-Beyond-Radiology-3c8f89a0cfc4800b9139d27630b0944f?source=copy_link)]

</aside>

## 1. Problem Context

AI/ML-enabled medical devices have expanded across clinical specialties, but regulatory activity is not evenly distributed across the dataset. Understanding where recent activity is concentrated can help identify Product Codes that warrant closer investigation beyond the largest specialty groups.

For this analysis, I focused on **observed FDA regulatory activity rather than market demand or commercial performance**. The goal was to determine whether Product Codes outside the dominant specialty showed meaningful and persistent recent activity, rather than simply appearing in a single high-activity year.

### **Analytical Question:**

Beyond the dominant specialty, which FDA Product Codes show meaningful recent activity, and do those signals persist across multiple completed years?

### **Scope & Analytical Boundary:**

This analysis measures **FDA record activity** associated with AI/ML-enabled medical devices. It does not measure:

- physician or hospital adoption
- market size or revenue
- clinical effectiveness or patient outcomes
- procurement or utilization
- competitive intensity
- commercial opportunity

Therefore, an increase in FDA records is interpreted as a **regulatory activity signal**, not as evidence of market growth or product success.

### Time Boundary:

The dataset contains records through 2026, but 2026 is an incomplete year. To avoid comparing a partial year with completed annual periods, all year-based comparisons in the analysis use **completed years through 2025**.

## 2. Data Preparation & Analytical Pipeline

The analysis was structured as a validation-first pipeline to ensure that downstream activity metrics were calculated from a consistent and traceable dataset.

### **Step 1: Data Ingestion & Relational Integration**

Combined the FDA AI/ML device records with the FOICLASS Product Code reference data. Before joining, I validated Product Code uniqueness in the classification table to ensure that the lookup could be used as a many-to-one relationship.

**Validation result**

- AI/ML device records: **1,524**
- Classification records: **7,088**
- Unique Product Codes: **7,088**
- Duplicate Product Codes: **0**
- Product Code join coverage: **1,524 / 1,524 (100%)**

The resulting dataset retained the FDA device records while adding standardized Product Code attributes such as medical specialty and device class.

### **Step 2: Data Quality & Time Boundary**

Validated completeness of the key classification fields and isolated incomplete-year records from completed-year analysis.

**Classification field completeness**

- Medical specialty: **10 missing (0.66%)**
- Device class: **0 missing**
- Generic device name: **0 missing**

For annual activity analysis, records through **2025** were treated as completed-year observations. 2026 was excluded from year-over-year comparisons because it represents an incomplete year.

### **Step 3: Recent Activity Signal**

Compared Product Code activity across completed-year windows, focusing on the most recent three completed years (**2023–2025**).

A minimum recent-activity threshold was used to reduce the influence of extremely small counts. This threshold is an **analytical rule defined for this project**, not an FDA classification.

### Step 4: Robustness Check

Compared recent activity using both:

- **3-year window:** 2023–2025
- **5-year window:** 2021–2025

The purpose was to determine whether candidates identified from recent activity also showed evidence of activity across a broader completed-year window.

### Step 5: Product Code Activity Profile

Product Codes were profiled using:

- total records
- first and last observed year
- annual activity from 2021–2025
- recent 3-year activity
- number of active years
- recent activity profile

Product Codes meeting the predefined recent-activity and persistence criteria were classified as **Recent-emerging signals**.

### Step 6: Functional Pattern Analysis

For the selected Product Codes, functional terms were explored from the wording of `generic_device_name`.

These terms were treated as **keyword-based exploratory flags**, not FDA-defined categories.

A device name could contain multiple flags, so the categories were not mutually exclusive.

### Step 7: Emerging Signal Trajectory Validation

The selected Product Codes were then examined year by year from **2021–2025**.

This provided a final descriptive check of whether the observed activity was isolated to a single year or appeared across multiple completed years.

### Step 8: Dashboard

The final dashboard summarizes:

1. Annual FDA record activity
2. Activity concentration by medical specialty
3. Recent-emerging Product Code activity
4. Year-by-year trajectories of the selected signals

## 3. Key Finding

### Radiology dominates observed FDA activity

Among the **1,423 records with an assigned medical specialty**, Radiology (RA) accounts for **1,091 records (76.7%)**. Rather than interpreting this as market share or saturation, it establishes the dominant baseline against which other Product Codes were examined.

### Recent-emerging Product Code signals

Four Product Codes met the predefined criteria for recent-emerging activity:

| Product Code | Medical Specialty | 2021–2025 Records | Active Years | Peak Activity |
| --- | --- | --- | --- | --- |
| **QNP** | GU | 18 | 5 | 5 |
| **OLZ** | NE | 7 | 5 | 2 |
| **QYE** | CV | 5 | 3 | 3 |
| **SBF** | NE | 5 | 2 | 4 |

#### ⭕ What this shows

> 
> 
> - **QNP** shows sustained activity across all five years.
> - **OLZ** shows persistent but low-volume activity across all five years.
> - **QYE** shows more recent activity, reaching 3 records in 2025.
> - **SBF** shows the strongest latest-year increase, reaching 4 records in 2025.
> 
> These Product Codes therefore represent **different recent activity trajectories**, rather than one uniform growth pattern.
> 

#### ❌ What this does not show

> 
> 
> 
> This analysis does **not** establish:
> 
> - market growth or market share
> - physician or hospital adoption
> - commercial opportunity
> - clinical effectiveness
> - competitive intensity
> - product success

![FDA AI Device Acceleration Chart](FDA%20AI%20ML%20Medical%20Device%20Activity%20Identifying%20Emer/Screenshot_2026-08-28_at_10.04.48_PM.png)

The dashboard summarizes the observed activity trend, specialty concentration, recent-emerging signals, and their 2021–2025 trajectories.

## 4. Product & Business Opportunities

### 1. Prioritize Non-Radiology Signals for Further Investigation

The observed concentration in Radiology suggests that Product Codes outside the dominant specialty may warrant separate investigation rather than being overlooked in aggregate-level analysis.

QNP, OLZ, QYE, and SBF provide four distinct activity profiles that could be investigated further using additional clinical, market, adoption, and competitive data.

**Product implication:**

For healthcare product teams, these signals could inform where to conduct deeper discovery before making product investment decisions.

---

### 2. Match Product Strategy to Activity Trajectory

The four signals show different activity patterns:

- **QNP:** sustained activity
- **OLZ:** persistent low-volume activity
- **QYE:** recent increase
- **SBF:** latest-year increase

This suggests that Product Code-level activity should be evaluated together with trajectory and absolute volume rather than relying on a single growth metric.

**Next analytical step:**

Combine FDA regulatory activity with external market, adoption, clinical workflow, and competitive data to evaluate whether these signals translate into meaningful product opportunities.

#### Click to view Pipeline Source Code (Python)

- STEP 1: Data Ingestion, Cleaning & Relational Integration
    
    ```python
    # ==============================================================================
    # STEP 1: Data Ingestion, Cleaning & Relational Integration
    # ==============================================================================
    
    import os
    import urllib.request
    import pandas as pd
    import numpy as np
    
    # ------------------------------------------------------------------------------
    # 1. Data configuration
    # ------------------------------------------------------------------------------
    
    DATA_CONFIG = {
        "aiml": {
            "url": "https://raw.githubusercontent.com/ej86/fda-ai-analytics/main/aiml-devices-csv.csv",
            "file_name": "aiml-devices-csv.csv",
            "sep": ","
        },
        "foiclass": {
            "url": "https://raw.githubusercontent.com/ej86/fda-ai-analytics/main/foiclass.csv",
            "file_name": "foiclass.csv",
            "sep": "|"
        }
    }
    
    # ------------------------------------------------------------------------------
    # 2. Load source datasets
    # ------------------------------------------------------------------------------
    
    def load_dataset(config):
        file_path = config["file_name"]
    
        if not os.path.exists(file_path):
            print(f"[FETCH] Downloading '{file_path}'...")
            urllib.request.urlretrieve(
                config["url"],
                file_path
            )
            print(f"[SUCCESS] Downloaded '{file_path}'.")
    
        df = pd.read_csv(
            file_path,
            sep=config["sep"],
            low_memory=False
        )
    
        if df.empty:
            raise ValueError(f"Dataset is empty: {file_path}")
    
        return df
    
    raw_aiml_df = load_dataset(DATA_CONFIG["aiml"])
    raw_foi_df = load_dataset(DATA_CONFIG["foiclass"])
    
    # ------------------------------------------------------------------------------
    # 3. Clean FDA AI-enabled device dataset
    # ------------------------------------------------------------------------------
    
    aiml_clean = raw_aiml_df.copy()
    
    aiml_clean["submission_number"] = (
        aiml_clean["Submission Number"]
        .astype("string")
        .str.strip()
    )
    
    aiml_clean["decision_date"] = pd.to_datetime(
        aiml_clean["Date of Final Decision"],
        format="%m/%d/%Y",
        errors="coerce"
    )
    
    aiml_clean["decision_year"] = (
        aiml_clean["decision_date"].dt.year
    )
    
    aiml_clean["device_name"] = (
        aiml_clean["Device"]
        .astype("string")
        .str.strip()
    )
    
    aiml_clean["company_name"] = (
        aiml_clean["Company"]
        .astype("string")
        .str.strip()
    )
    
    aiml_clean["lead_panel"] = (
        aiml_clean["Panel (Lead)"]
        .astype("string")
        .str.strip()
    )
    
    aiml_clean["product_code"] = (
        aiml_clean["Primary Product Code"]
        .astype("string")
        .str.strip()
        .str.upper()
    )
    
    # ------------------------------------------------------------------------------
    # 4. Derive high-level regulatory pathway
    # ------------------------------------------------------------------------------
    
    def assign_regulatory_pathway(submission_id):
        value = str(submission_id).strip().upper()
    
        if value.startswith("K"):
            return "510(k)"
        elif value.startswith("DEN"):
            return "De Novo"
        elif value.startswith("P"):
            return "PMA"
        else:
            return "Unclassified"
    
    aiml_clean["regulatory_pathway"] = (
        aiml_clean["submission_number"]
        .apply(assign_regulatory_pathway)
    )
    
    # ------------------------------------------------------------------------------
    # 5. Clean FDA Product Classification lookup
    # ------------------------------------------------------------------------------
    
    foi_clean = raw_foi_df[
        [
            "PRODUCTCODE",
            "MEDICALSPECIALTY",
            "DEVICENAME",
            "DEVICECLASS",
            "REGULATIONNUMBER",
            "DEFINITION",
            "TECHNICALMETHOD",
            "TARGETAREA"
        ]
    ].copy()
    
    foi_clean.columns = [
        "product_code",
        "medical_specialty",
        "generic_device_name",
        "device_class",
        "regulation_number",
        "definition",
        "technical_method",
        "target_area"
    ]
    
    foi_clean["product_code"] = (
        foi_clean["product_code"]
        .astype("string")
        .str.strip()
        .str.upper()
    )
    
    # ------------------------------------------------------------------------------
    # 6. Validate Product Code uniqueness
    # ------------------------------------------------------------------------------
    
    product_code_counts = (
        foi_clean["product_code"]
        .value_counts()
    )
    
    duplicate_product_codes = (
        product_code_counts[
            product_code_counts > 1
        ]
    )
    
    print("=== PRODUCT CODE UNIQUENESS AUDIT ===")
    print(f"Classification records: {len(foi_clean):,}")
    print(
        f"Unique product codes: "
        f"{foi_clean['product_code'].nunique():,}"
    )
    print(
        f"Duplicate product codes: "
        f"{len(duplicate_product_codes):,}"
    )
    
    if len(duplicate_product_codes) > 0:
        raise ValueError(
            "Duplicate product codes detected. "
            "Resolve the lookup relationship before joining."
        )
    
    # ------------------------------------------------------------------------------
    # 7. Relational integration
    # ------------------------------------------------------------------------------
    
    df_integrated = pd.merge(
        aiml_clean,
        foi_clean,
        on="product_code",
        how="left",
        validate="many_to_one",
        indicator=True
    )
    
    # ------------------------------------------------------------------------------
    # 8. Join coverage validation
    # ------------------------------------------------------------------------------
    
    matched_records = (
        df_integrated["_merge"] == "both"
    ).sum()
    
    total_records = len(aiml_clean)
    
    join_rate = (
        matched_records /
        total_records *
        100
    )
    
    print("\n=== DATA INTEGRATION AUDIT ===")
    print(f"AI device records: {total_records:,}")
    print(
        f"Product Code join coverage: "
        f"{matched_records:,} ({join_rate:.2f}%)"
    )
    
    if matched_records < total_records:
    
        unmatched_codes = (
            df_integrated.loc[
                df_integrated["_merge"] == "left_only",
                "product_code"
            ]
            .drop_duplicates()
            .tolist()
        )
    
        print("\nUnmatched product codes:")
        print(unmatched_codes)
    
    # ------------------------------------------------------------------------------
    # 9. Classification field completeness
    # ------------------------------------------------------------------------------
    
    print("\n=== CLASSIFICATION FIELD COMPLETENESS ===")
    
    for column in [
        "medical_specialty",
        "device_class",
        "generic_device_name"
    ]:
    
        missing_count = (
            df_integrated[column]
            .isna()
            .sum()
        )
    
        missing_pct = (
            missing_count /
            len(df_integrated) *
            100
        )
    
        print(
            f"{column}: "
            f"{missing_count:,} missing "
            f"({missing_pct:.2f}%)"
        )
    ```
    
- STEP 2: Core Exploratory & Regulatory Analysis
    
    ```python
    # ==============================================================================
    # STEP 2: Core Exploratory & Regulatory Analysis
    # ==============================================================================
    
    # ------------------------------------------------------------------------------
    # 1. Investigate missing medical specialty
    # ------------------------------------------------------------------------------
    
    missing_specialty = df_integrated[
        df_integrated["medical_specialty"].isna()
    ].copy()
    
    print("=== MISSING MEDICAL SPECIALTY AUDIT ===")
    print(f"Missing records: {len(missing_specialty):,}")
    
    if len(missing_specialty) > 0:
        print("\nRelevant fields for review:")
        print(
            missing_specialty[
                [
                    "submission_number",
                    "device_name",
                    "product_code",
                    "generic_device_name",
                    "device_class",
                    "lead_panel"
                ]
            ].to_string(index=False)
        )
    
    # ------------------------------------------------------------------------------
    # 2. Medical specialty distribution
    # ------------------------------------------------------------------------------
    
    specialty_summary = (
        df_integrated[
            df_integrated["medical_specialty"].notna()
        ]
        .groupby("medical_specialty")
        .agg(
            record_count=("submission_number", "count")
        )
        .reset_index()
        .sort_values(
            "record_count",
            ascending=False
        )
    )
    
    specialty_summary["record_share_pct"] = (
        specialty_summary["record_count"]
        / specialty_summary["record_count"].sum()
        * 100
    ).round(2)
    
    print("\n=== AI-ENABLED DEVICE RECORDS BY MEDICAL SPECIALTY ===")
    print(
        specialty_summary.head(10).to_string(index=False)
    )
    
    # ------------------------------------------------------------------------------
    # 3. Product Code concentration
    # ------------------------------------------------------------------------------
    
    product_summary = (
        df_integrated
        .groupby(
            [
                "product_code",
                "generic_device_name",
                "medical_specialty",
                "device_class"
            ],
            dropna=False
        )
        .agg(
            record_count=("submission_number", "count")
        )
        .reset_index()
        .sort_values(
            "record_count",
            ascending=False
        )
    )
    
    product_summary["record_share_pct"] = (
        product_summary["record_count"]
        / len(df_integrated)
        * 100
    ).round(2)
    
    print("\n=== TOP PRODUCT CODES BY AI-ENABLED DEVICE RECORDS ===")
    print(
        product_summary.head(15).to_string(index=False)
    )
    
    # ------------------------------------------------------------------------------
    # 4. Device class distribution
    # ------------------------------------------------------------------------------
    
    device_class_summary = (
        df_integrated["device_class"]
        .value_counts(dropna=False)
        .rename_axis("device_class")
        .reset_index(name="record_count")
    )
    
    device_class_summary["record_share_pct"] = (
        device_class_summary["record_count"]
        / len(df_integrated)
        * 100
    ).round(2)
    
    print("\n=== DEVICE CLASS DISTRIBUTION ===")
    print(
        device_class_summary.to_string(index=False)
    )
    
    # ------------------------------------------------------------------------------
    # 5. Regulatory pathway distribution
    # ------------------------------------------------------------------------------
    
    pathway_summary = (
        df_integrated["regulatory_pathway"]
        .value_counts(dropna=False)
        .rename_axis("regulatory_pathway")
        .reset_index(name="record_count")
    )
    
    pathway_summary["record_share_pct"] = (
        pathway_summary["record_count"]
        / len(df_integrated)
        * 100
    ).round(2)
    
    print("\n=== REGULATORY PATHWAY DISTRIBUTION ===")
    print(
        pathway_summary.to_string(index=False)
    )
    
    # ------------------------------------------------------------------------------
    # 6. Medical specialty × regulatory pathway
    # ------------------------------------------------------------------------------
    
    specialty_pathway = pd.crosstab(
        df_integrated["medical_specialty"],
        df_integrated["regulatory_pathway"],
        dropna=False
    )
    
    specialty_pathway["Total"] = (
        specialty_pathway.sum(axis=1)
    )
    
    specialty_pathway = (
        specialty_pathway
        .sort_values(
            "Total",
            ascending=False
        )
    )
    
    print("\n=== MEDICAL SPECIALTY × REGULATORY PATHWAY ===")
    print(
        specialty_pathway.head(10)
    )
    
    # ------------------------------------------------------------------------------
    # 7. Annual activity trend
    # ------------------------------------------------------------------------------
    
    yearly_trend = (
        df_integrated[
            df_integrated["decision_year"].notna()
        ]
        .groupby("decision_year")
        .size()
        .rename("record_count")
        .reset_index()
        .sort_values("decision_year")
    )
    
    yearly_trend["record_share_pct"] = (
        yearly_trend["record_count"]
        / yearly_trend["record_count"].sum()
        * 100
    ).round(2)
    
    print("\n=== ANNUAL AI-ENABLED DEVICE RECORDS ===")
    print(
        yearly_trend.to_string(index=False)
    )
    ```
    
- STEP 3: Recent Activity and Descriptive Acceleration Analysis
👉🏻 Purpose: Examine recent FDA activity by Product Code and quantify acceleration only where a valid historical baseline exists.
    
    ```python
    # ==============================================================================
    # STEP 3: Recent Activity and Descriptive Acceleration Analysis
    # ==============================================================================
    # Purpose:
    # Examine recent FDA activity by Product Code and quantify acceleration
    # only where a valid historical baseline exists.
    #
    # Analytical principles:
    # - 2026 is excluded because it is an incomplete year.
    # - Recent activity is measured across the three latest completed years:
    #   2023–2025.
    # - Historical annual activity is calculated using each Product Code's
    #   own observed pre-2023 period, rather than the dataset-wide time span.
    # - Acceleration is a descriptive activity ratio, not a measure of market
    #   growth, adoption, clinical effectiveness, or commercial opportunity.
    # - The final "Recent-emerging signal" classification is defined in STEP 5.
    # ==============================================================================
    
    # ------------------------------------------------------------------------------
    # 1. Prepare completed-year data
    # ------------------------------------------------------------------------------
    
    required_columns = [
        "product_code",
        "submission_number",
        "decision_year",
        "generic_device_name",
        "medical_specialty",
        "device_class"
    ]
    
    missing_columns = [
        column for column in required_columns
        if column not in df_integrated.columns
    ]
    
    if missing_columns:
        raise KeyError(
            f"Required columns are missing from df_integrated: {missing_columns}"
        )
    
    analysis_df = df_integrated.copy()
    analysis_df["decision_year"] = pd.to_numeric(
        analysis_df["decision_year"],
        errors="coerce"
    )
    
    analysis_df = analysis_df[
        analysis_df["decision_year"].notna()
        & (analysis_df["decision_year"] <= 2025)
    ].copy()
    
    analysis_df["decision_year"] = (
        analysis_df["decision_year"]
        .astype(int)
    )
    
    recent_years = [2023, 2024, 2025]
    historical_cutoff = 2023
    
    # ------------------------------------------------------------------------------
    # 2. Build Product Code activity profile
    # ------------------------------------------------------------------------------
    
    product_activity = (
        analysis_df
        .groupby("product_code")
        .agg(
            total_records=("submission_number", "count"),
            first_year=("decision_year", "min"),
            last_year=("decision_year", "max"),
            generic_device_name=("generic_device_name", "first"),
            medical_specialty=("medical_specialty", "first"),
            device_class=("device_class", "first")
        )
        .reset_index()
    )
    
    # ------------------------------------------------------------------------------
    # 3. Calculate recent activity
    # ------------------------------------------------------------------------------
    
    recent_activity = (
        analysis_df[
            analysis_df["decision_year"].isin(recent_years)
        ]
        .groupby("product_code")
        .size()
        .rename("recent_records")
    )
    
    product_activity = product_activity.merge(
        recent_activity,
        on="product_code",
        how="left",
        validate="one_to_one"
    )
    
    product_activity["recent_records"] = (
        product_activity["recent_records"]
        .fillna(0)
        .astype(int)
    )
    
    product_activity["recent_record_share_pct"] = (
        product_activity["recent_records"]
        / product_activity["total_records"]
        * 100
    ).round(2)
    
    # ------------------------------------------------------------------------------
    # 4. Calculate Product Code-specific historical activity
    # ------------------------------------------------------------------------------
    
    historical_activity = (
        analysis_df[
            analysis_df["decision_year"] < historical_cutoff
        ]
        .groupby("product_code")
        .size()
        .rename("historical_records")
    )
    
    product_activity = product_activity.merge(
        historical_activity,
        on="product_code",
        how="left",
        validate="one_to_one"
    )
    
    product_activity["historical_records"] = (
        product_activity["historical_records"]
        .fillna(0)
        .astype(int)
    )
    
    # Number of completed calendar years from the Product Code's first
    # observed year through the end of 2022.
    product_activity["historical_years_observed"] = (
        historical_cutoff
        - product_activity["first_year"]
    ).clip(lower=1)
    
    product_activity["historical_annual_rate"] = np.where(
        product_activity["historical_records"] > 0,
        product_activity["historical_records"]
        / product_activity["historical_years_observed"],
        np.nan
    )
    
    product_activity["recent_annual_rate"] = (
        product_activity["recent_records"]
        / len(recent_years)
    )
    
    # ------------------------------------------------------------------------------
    # 5. Calculate descriptive acceleration
    # ------------------------------------------------------------------------------
    
    product_activity["activity_acceleration"] = np.where(
        product_activity["historical_annual_rate"] > 0,
        product_activity["recent_annual_rate"]
        / product_activity["historical_annual_rate"],
        np.nan
    )
    
    product_activity["activity_acceleration"] = (
        product_activity["activity_acceleration"]
        .round(2)
    )
    
    # ------------------------------------------------------------------------------
    # 6. Identify descriptive acceleration candidates
    # ------------------------------------------------------------------------------
    
    emerging_candidates = product_activity[
        product_activity["recent_records"] >= 3
        & product_activity["activity_acceleration"].ge(1.5)
    ].copy()
    
    emerging_candidates = (
        emerging_candidates
        .sort_values(
            ["activity_acceleration", "recent_records"],
            ascending=[False, False]
        )
    )
    
    # ------------------------------------------------------------------------------
    # 7. Display results
    # ------------------------------------------------------------------------------
    
    print("=== STEP 3: RECENT ACTIVITY AND DESCRIPTIVE ACCELERATION ===")
    print(f"Recent completed years: {recent_years}")
    print("Historical baseline: Product Code-specific activity before 2023")
    
    display_columns = [
        "product_code",
        "generic_device_name",
        "medical_specialty",
        "device_class",
        "total_records",
        "historical_records",
        "recent_records",
        "recent_record_share_pct",
        "historical_annual_rate",
        "recent_annual_rate",
        "activity_acceleration",
        "first_year",
        "last_year"
    ]
    
    if emerging_candidates.empty:
        print("\nNo Product Codes met the descriptive acceleration criteria.")
    else:
        print("\n=== DESCRIPTIVE ACCELERATION CANDIDATES ===")
        print(
            emerging_candidates[display_columns]
            .head(20)
            .to_string(index=False)
        )
    
    # ------------------------------------------------------------------------------
    # 8. Analytical boundary
    # ------------------------------------------------------------------------------
    
    print("\n=== ANALYTICAL BOUNDARY ===")
    print(
        "Activity acceleration is a descriptive comparison of observed "
        "FDA record rates. It does not establish market growth, adoption, "
        "clinical effectiveness, or commercial opportunity."
    )
    ```
    
- STEP 4: Recent Activity Window Sensitivity Check
👉🏻 Purpose: Compare recent Product Code activity across 3-year and 5-year windows to assess how sensitive the observed signal is to the selected time window.
    
    ```python
    # ==============================================================================
    # STEP 4: Recent Activity Window Sensitivity Check
    # ==============================================================================
    # Purpose:
    # Compare recent Product Code activity across 3-year and 5-year windows
    # to assess how sensitive the observed signal is to the selected time window.
    #
    # Analytical principles:
    # - 2026 is excluded because it is an incomplete year.
    # - The comparison uses fixed completed-year windows: 2023–2025 and 2021–2025.
    # - This is a sensitivity analysis, not a statistical robustness test.
    # - The minimum activity threshold is an analytical filter, not an FDA standard.
    # ==============================================================================
    
    # ------------------------------------------------------------------------------
    # 1. Prepare completed-year data
    # ------------------------------------------------------------------------------
    
    completed_df = df_integrated.copy()
    completed_df["decision_year"] = pd.to_numeric(
        completed_df["decision_year"],
        errors="coerce"
    )
    
    completed_df = completed_df[
        completed_df["decision_year"].notna()
        & (completed_df["decision_year"] <= 2025)
    ].copy()
    
    completed_df["decision_year"] = (
        completed_df["decision_year"]
        .astype(int)
    )
    
    recent_3y = [2023, 2024, 2025]
    recent_5y = [2021, 2022, 2023, 2024, 2025]
    
    # ------------------------------------------------------------------------------
    # 2. Build Product Code activity profile
    # ------------------------------------------------------------------------------
    
    product_signal = (
        completed_df
        .groupby("product_code")
        .agg(
            total_records=("submission_number", "count"),
            first_year=("decision_year", "min"),
            last_year=("decision_year", "max"),
            generic_device_name=("generic_device_name", "first"),
            medical_specialty=("medical_specialty", "first"),
            device_class=("device_class", "first")
        )
        .reset_index()
    )
    
    # ------------------------------------------------------------------------------
    # 3. Calculate activity in each comparison window
    # ------------------------------------------------------------------------------
    
    recent_3y_counts = (
        completed_df[
            completed_df["decision_year"].isin(recent_3y)
        ]
        .groupby("product_code")
        .size()
        .rename("recent_3y_records")
    )
    
    recent_5y_counts = (
        completed_df[
            completed_df["decision_year"].isin(recent_5y)
        ]
        .groupby("product_code")
        .size()
        .rename("recent_5y_records")
    )
    
    product_signal = (
        product_signal
        .merge(
            recent_3y_counts,
            on="product_code",
            how="left",
            validate="one_to_one"
        )
        .merge(
            recent_5y_counts,
            on="product_code",
            how="left",
            validate="one_to_one"
        )
    )
    
    product_signal[
        ["recent_3y_records", "recent_5y_records"]
    ] = (
        product_signal[
            ["recent_3y_records", "recent_5y_records"]
        ]
        .fillna(0)
        .astype(int)
    )
    
    # ------------------------------------------------------------------------------
    # 4. Calculate recent activity shares
    # ------------------------------------------------------------------------------
    
    product_signal["recent_3y_share_pct"] = (
        product_signal["recent_3y_records"]
        / product_signal["total_records"]
        * 100
    ).round(2)
    
    product_signal["recent_5y_share_pct"] = (
        product_signal["recent_5y_records"]
        / product_signal["total_records"]
        * 100
    ).round(2)
    
    product_signal["window_share_difference_pct"] = (
        product_signal["recent_3y_share_pct"]
        - product_signal["recent_5y_share_pct"]
    ).round(2)
    
    # ------------------------------------------------------------------------------
    # 5. Apply minimum recent-activity threshold
    # ------------------------------------------------------------------------------
    
    signal_candidates = product_signal[
        product_signal["recent_3y_records"] >= 5
    ].copy()
    
    signal_candidates = (
        signal_candidates
        .sort_values(
            ["recent_3y_records", "window_share_difference_pct"],
            ascending=[False, False]
        )
    )
    
    # ------------------------------------------------------------------------------
    # 6. Display results
    # ------------------------------------------------------------------------------
    
    print("=== STEP 4: RECENT ACTIVITY WINDOW SENSITIVITY CHECK ===")
    print(f"3-year window: {recent_3y}")
    print(f"5-year window: {recent_5y}")
    
    display_columns = [
        "product_code",
        "generic_device_name",
        "medical_specialty",
        "device_class",
        "total_records",
        "recent_3y_records",
        "recent_3y_share_pct",
        "recent_5y_records",
        "recent_5y_share_pct",
        "window_share_difference_pct",
        "first_year",
        "last_year"
    ]
    
    print("\n=== PRODUCT CODES WITH MEANINGFUL RECENT ACTIVITY ===")
    
    if signal_candidates.empty:
        print("No Product Codes met the minimum recent-activity threshold.")
    else:
        print(
            signal_candidates[display_columns]
            .head(20)
            .to_string(index=False)
        )
    
    # ------------------------------------------------------------------------------
    # 7. Analytical boundary
    # ------------------------------------------------------------------------------
    
    print("\n=== ANALYTICAL BOUNDARY ===")
    print(
        "The window comparison evaluates sensitivity to the selected time "
        "window. It does not establish statistical significance or guarantee "
        "that an observed signal will persist."
    )
    ```
    
- STEP 5: Product Code Activity Profile
    
    ```python
    # ==============================================================================
    # STEP 5: Product Code Activity Profile
    # ==============================================================================
    
    completed_df = df_integrated[
        df_integrated["decision_year"].notna()
        & (df_integrated["decision_year"] <= 2025)
    ].copy()
    
    # ------------------------------------------------------------------------------
    # 1. Annual activity by Product Code
    # ------------------------------------------------------------------------------
    
    annual_product_activity = (
        completed_df
        .groupby(
            ["product_code", "decision_year"]
        )
        .size()
        .unstack(fill_value=0)
    )
    
    for year in range(2021, 2026):
        if year not in annual_product_activity.columns:
            annual_product_activity[year] = 0
    
    annual_product_activity = (
        annual_product_activity[
            [2021, 2022, 2023, 2024, 2025]
        ]
        .reset_index()
    )
    
    # ------------------------------------------------------------------------------
    # 2. Product-level descriptive profile
    # ------------------------------------------------------------------------------
    
    product_profile = (
        completed_df
        .groupby("product_code")
        .agg(
            total_records=("submission_number", "count"),
            first_year=("decision_year", "min"),
            last_year=("decision_year", "max"),
            generic_device_name=("generic_device_name", "first"),
            medical_specialty=("medical_specialty", "first"),
            device_class=("device_class", "first")
        )
        .reset_index()
    )
    
    product_profile = product_profile.merge(
        annual_product_activity,
        on="product_code",
        how="left",
        validate="one_to_one"
    )
    
    # ------------------------------------------------------------------------------
    # 3. Recent activity metrics
    # ------------------------------------------------------------------------------
    
    product_profile["recent_3y_records"] = (
        product_profile[[2023, 2024, 2025]]
        .sum(axis=1)
    )
    
    product_profile["recent_3y_share_pct"] = (
        product_profile["recent_3y_records"]
        / product_profile["total_records"]
        * 100
    ).round(2)
    
    # ------------------------------------------------------------------------------
    # 4. Persistence
    # ------------------------------------------------------------------------------
    
    product_profile["active_years_2021_2025"] = (
        product_profile[
            [2021, 2022, 2023, 2024, 2025]
        ]
        .gt(0)
        .sum(axis=1)
    )
    
    # ------------------------------------------------------------------------------
    # 5. Identify recent activity profiles
    # ------------------------------------------------------------------------------
    
    product_profile["activity_profile"] = np.select(
        [
            (
                (product_profile["recent_3y_records"] >= 5)
                &
                (product_profile["first_year"] >= 2021)
                &
                (product_profile["active_years_2021_2025"] >= 2)
            ),
    
            (
                (product_profile["recent_3y_records"] >= 5)
                &
                (product_profile["first_year"] < 2021)
                &
                (product_profile["active_years_2021_2025"] >= 3)
            )
        ],
        [
            "Recent-emerging signal",
            "Established recent activity"
        ],
        default="Lower-volume / insufficient signal"
    )
    
    # ------------------------------------------------------------------------------
    # 6. Review candidate profiles
    # ------------------------------------------------------------------------------
    
    profile_order = {
        "Recent-emerging signal": 1,
        "Established recent activity": 2,
        "Lower-volume / insufficient signal": 3
    }
    
    product_profile["profile_order"] = (
        product_profile["activity_profile"]
        .map(profile_order)
    )
    
    product_profile = (
        product_profile
        .sort_values(
            [
                "profile_order",
                "recent_3y_records"
            ],
            ascending=[True, False]
        )
    )
    
    # ------------------------------------------------------------------------------
    # 7. Display
    # ------------------------------------------------------------------------------
    
    print("=== PRODUCT CODE ACTIVITY PROFILES ===")
    
    display_columns = [
        "product_code",
        "generic_device_name",
        "medical_specialty",
        "device_class",
        "total_records",
        "first_year",
        "last_year",
        2021,
        2022,
        2023,
        2024,
        2025,
        "recent_3y_records",
        "recent_3y_share_pct",
        "active_years_2021_2025",
        "activity_profile"
    ]
    
    print(
        product_profile[
            display_columns
        ]
        .head(30)
        .to_string(index=False)
    )
    ```
    
- STEP 6: Functional Pattern Analysis
👉🏻 Purpose: Examine whether Product Codes identified as recent-emerging signals show different functional characteristics from established recent activity.
    
    ```python
    # ==============================================================================
    # STEP 6: Functional Pattern Analysis
    # ==============================================================================
    # Purpose:
    # Examine whether Product Codes identified as recent-emerging signals
    # show different functional characteristics from established recent activity.
    #
    # IMPORTANT:
    # - Functional terms are NOT FDA-provided categories.
    # - They are transparent keyword flags derived from generic_device_name.
    # - A Product Code can have multiple functional flags.
    # - No claim is made about actual physician behavior, clinical workflow,
    #   clinical effectiveness, market demand, or business opportunity.
    # - 2026 is excluded because it is an incomplete year.
    # ==============================================================================
    
    # ------------------------------------------------------------------------------
    # 1. Prepare completed-year data
    # ------------------------------------------------------------------------------
    
    functional_df = df_integrated[
        df_integrated["decision_year"].notna()
        & (df_integrated["decision_year"] <= 2025)
    ].copy()
    
    # ------------------------------------------------------------------------------
    # 2. Recreate Product Code activity profile
    # ------------------------------------------------------------------------------
    # This avoids depending on variables created in previous cells.
    
    profile_6 = (
        functional_df
        .groupby("product_code")
        .agg(
            total_records=("submission_number", "count"),
            first_year=("decision_year", "min"),
            last_year=("decision_year", "max"),
            generic_device_name=("generic_device_name", "first"),
            medical_specialty=("medical_specialty", "first"),
            device_class=("device_class", "first")
        )
        .reset_index()
    )
    
    # ------------------------------------------------------------------------------
    # 3. Calculate annual activity
    # ------------------------------------------------------------------------------
    
    annual_6 = (
        functional_df
        .groupby(
            ["product_code", "decision_year"]
        )
        .size()
        .unstack(fill_value=0)
    )
    
    for year in range(2021, 2026):
        if year not in annual_6.columns:
            annual_6[year] = 0
    
    annual_6 = annual_6[
        [2021, 2022, 2023, 2024, 2025]
    ].reset_index()
    
    profile_6 = profile_6.merge(
        annual_6,
        on="product_code",
        how="left",
        validate="one_to_one"
    )
    
    # ------------------------------------------------------------------------------
    # 4. Recreate the same activity profile logic used in STEP 5
    # ------------------------------------------------------------------------------
    
    profile_6["recent_3y_records"] = (
        profile_6[[2023, 2024, 2025]]
        .sum(axis=1)
    )
    
    profile_6["active_years_2021_2025"] = (
        profile_6[
            [2021, 2022, 2023, 2024, 2025]
        ]
        .gt(0)
        .sum(axis=1)
    )
    
    profile_6["activity_profile"] = np.select(
        [
            (
                (profile_6["recent_3y_records"] >= 5)
                &
                (profile_6["first_year"] >= 2021)
                &
                (profile_6["active_years_2021_2025"] >= 2)
            ),
    
            (
                (profile_6["recent_3y_records"] >= 5)
                &
                (profile_6["first_year"] < 2021)
                &
                (profile_6["active_years_2021_2025"] >= 3)
            )
        ],
        [
            "Recent-emerging signal",
            "Established recent activity"
        ],
        default="Lower-volume / insufficient signal"
    )
    
    # ------------------------------------------------------------------------------
    # 5. Create comparison groups
    # ------------------------------------------------------------------------------
    
    profile_6["comparison_group"] = np.select(
        [
            profile_6["activity_profile"] == "Recent-emerging signal",
            profile_6["activity_profile"] == "Established recent activity"
        ],
        [
            "Emerging signal",
            "Established activity"
        ],
        default="Other"
    )
    
    analysis_profile_6 = profile_6[
        profile_6["comparison_group"].isin(
            ["Emerging signal", "Established activity"]
        )
    ].copy()
    
    # ------------------------------------------------------------------------------
    # 6. Extract explicit functional terms from generic_device_name
    # ------------------------------------------------------------------------------
    # These are FLAGS, not mutually exclusive categories.
    # A device can therefore have more than one "Yes".
    #
    # The terms are deliberately limited to wording that appears explicitly
    # in the generic device name.
    
    name_text = (
        analysis_profile_6["generic_device_name"]
        .fillna("")
        .astype(str)
        .str.lower()
    )
    
    analysis_profile_6["mentions_detection"] = (
        name_text.str.contains(
            r"\bdetection\b|\bdetect\b",
            regex=True,
            na=False
        )
    )
    
    analysis_profile_6["mentions_notification"] = (
        name_text.str.contains(
            r"\bnotification\b|\balert\b",
            regex=True,
            na=False
        )
    )
    
    analysis_profile_6["mentions_triage_priority"] = (
        name_text.str.contains(
            r"\btriage\b|\bprioritization\b|\bprioritisation\b",
            regex=True,
            na=False
        )
    )
    
    analysis_profile_6["mentions_diagnostic"] = (
        name_text.str.contains(
            r"\bdiagnostic\b|\bdiagnosis\b",
            regex=True,
            na=False
        )
    )
    
    analysis_profile_6["mentions_processing"] = (
        name_text.str.contains(
            r"\bprocessing\b",
            regex=True,
            na=False
        )
    )
    
    analysis_profile_6["mentions_planning"] = (
        name_text.str.contains(
            r"\bplanning\b",
            regex=True,
            na=False
        )
    )
    
    analysis_profile_6["mentions_augmented_reality"] = (
        name_text.str.contains(
            r"\baugmented reality\b",
            regex=True,
            na=False
        )
    )
    
    # ------------------------------------------------------------------------------
    # 7. AUDIT THE ACTUAL PRODUCT CODE NAMES FIRST
    # ------------------------------------------------------------------------------
    
    print("=== STEP 6: PRODUCT CODE FUNCTIONAL TERM AUDIT ===")
    
    audit_columns_6 = [
        "product_code",
        "generic_device_name",
        "medical_specialty",
        "total_records",
        "recent_3y_records",
        "activity_profile",
        "comparison_group",
        "mentions_detection",
        "mentions_notification",
        "mentions_triage_priority",
        "mentions_diagnostic",
        "mentions_processing",
        "mentions_planning",
        "mentions_augmented_reality"
    ]
    
    print(
        analysis_profile_6[
            audit_columns_6
        ]
        .sort_values(
            ["comparison_group", "recent_3y_records"],
            ascending=[True, False]
        )
        .to_string(index=False)
    )
    
    # ------------------------------------------------------------------------------
    # 8. Calculate functional-term prevalence by comparison group
    # ------------------------------------------------------------------------------
    # Because flags are non-exclusive, percentages do NOT need to sum to 100%.
    
    flag_columns = {
        "mentions_detection": "Detection",
        "mentions_notification": "Notification / alert",
        "mentions_triage_priority": "Triage / prioritization",
        "mentions_diagnostic": "Diagnostic / diagnosis",
        "mentions_processing": "Processing",
        "mentions_planning": "Planning",
        "mentions_augmented_reality": "Augmented reality"
    }
    
    summary_rows = []
    
    for group_name, group_df in analysis_profile_6.groupby(
        "comparison_group"
    ):
    
        group_product_codes = len(group_df)
    
        for flag_column, label in flag_columns.items():
    
            matching_product_codes = int(
                group_df[flag_column].sum()
            )
    
            share_pct = round(
                matching_product_codes
                / group_product_codes
                * 100,
                2
            )
    
            summary_rows.append(
                {
                    "comparison_group": group_name,
                    "functional_term": label,
                    "product_code_count": matching_product_codes,
                    "product_code_share_pct": share_pct
                }
            )
    
    functional_summary_6 = pd.DataFrame(summary_rows)
    
    # ------------------------------------------------------------------------------
    # 9. Display comparison
    # ------------------------------------------------------------------------------
    
    print("\n=== STEP 6: FUNCTIONAL TERM COMPARISON ===")
    
    print(
        functional_summary_6
        .sort_values(
            [
                "comparison_group",
                "product_code_share_pct"
            ],
            ascending=[True, False]
        )
        .to_string(index=False)
    )
    
    # ------------------------------------------------------------------------------
    # 10. Compare the emerging signal directly
    # ------------------------------------------------------------------------------
    
    emerging_only_6 = analysis_profile_6[
        analysis_profile_6["comparison_group"] == "Emerging signal"
    ].copy()
    
    print("\n=== STEP 6: EMERGING SIGNAL PRODUCTS ===")
    
    if emerging_only_6.empty:
    
        print("No Product Codes are currently classified as emerging signals.")
    
    else:
    
        print(
            emerging_only_6[
                [
                    "product_code",
                    "generic_device_name",
                    "medical_specialty",
                    "total_records",
                    "recent_3y_records"
                ]
            ]
            .sort_values(
                "recent_3y_records",
                ascending=False
            )
            .to_string(index=False)
        )
    
    # ------------------------------------------------------------------------------
    # 11. Analytical boundary
    # ------------------------------------------------------------------------------
    
    print("\n=== ANALYTICAL BOUNDARY ===")
    
    print(
        "Functional terms are exploratory keyword flags based on the wording "
        "of generic_device_name. They are not FDA-defined functional categories "
        "and do not establish clinical workflow, physician decision-making, "
        "clinical effectiveness, market demand, or business opportunity."
    )
    ```
    
- STEP 7: Emerging Signal Trajectory Validation
👉🏻 Purpose: Validate Product Codes classified as "Recent-emerging signal" in STEP 5 by examining their year-by-year FDA activity from 2021 through 2025.
    
    ```python
    # ==============================================================================
    # STEP 7: Emerging Signal Trajectory Validation
    # ==============================================================================
    # Purpose:
    # Validate Product Codes classified as "Recent-emerging signal" in STEP 5
    # by examining their year-by-year FDA activity from 2021 through 2025.
    #
    # Analytical principles:
    # - Product Codes are derived directly from the STEP 5 classification.
    # - 2026 is excluded because it is an incomplete year.
    # - Activity counts represent observed FDA records, not market demand,
    #   adoption, clinical effectiveness, or commercial success.
    # - Low-volume signals are interpreted together with their absolute record counts.
    # ==============================================================================
    
    # ------------------------------------------------------------------------------
    # 1. Prepare completed-year data
    # ------------------------------------------------------------------------------
    
    step7_df = df_integrated.copy()
    step7_df["decision_year"] = pd.to_numeric(
        step7_df["decision_year"],
        errors="coerce"
    )
    
    step7_df = step7_df[
        step7_df["decision_year"].notna()
        & (step7_df["decision_year"] <= 2025)
    ].copy()
    
    step7_df["decision_year"] = (
        step7_df["decision_year"]
        .astype(int)
    )
    
    # ------------------------------------------------------------------------------
    # 2. Derive emerging Product Codes directly from STEP 5
    # ------------------------------------------------------------------------------
    
    emerging_codes_step7 = (
        product_profile.loc[
            product_profile["activity_profile"] == "Recent-emerging signal",
            "product_code"
        ]
        .dropna()
        .astype(str)
        .drop_duplicates()
        .tolist()
    )
    
    # ------------------------------------------------------------------------------
    # 3. Calculate annual activity
    # ------------------------------------------------------------------------------
    
    trajectory_years = [2021, 2022, 2023, 2024, 2025]
    
    emerging_trajectory = (
        step7_df[
            step7_df["product_code"].astype(str).isin(emerging_codes_step7)
            & step7_df["decision_year"].isin(trajectory_years)
        ]
        .groupby(
            ["product_code", "decision_year"]
        )
        .size()
        .unstack(fill_value=0)
    )
    
    for year in trajectory_years:
        if year not in emerging_trajectory.columns:
            emerging_trajectory[year] = 0
    
    if emerging_trajectory.empty:
        emerging_trajectory = pd.DataFrame(
            columns=trajectory_years
        )
        emerging_trajectory.index.name = "product_code"
    else:
        emerging_trajectory = emerging_trajectory[
            trajectory_years
        ]
    
    # ------------------------------------------------------------------------------
    # 4. Calculate descriptive trajectory metrics
    # ------------------------------------------------------------------------------
    
    if not emerging_trajectory.empty:
    
        emerging_trajectory["total_records_2021_2025"] = (
            emerging_trajectory[trajectory_years]
            .sum(axis=1)
            .astype(int)
        )
    
        emerging_trajectory["active_years"] = (
            emerging_trajectory[trajectory_years]
            .gt(0)
            .sum(axis=1)
            .astype(int)
        )
    
        emerging_trajectory["peak_year"] = (
            emerging_trajectory[trajectory_years]
            .idxmax(axis=1)
            .astype(int)
        )
    
        emerging_trajectory["peak_activity"] = (
            emerging_trajectory[trajectory_years]
            .max(axis=1)
            .astype(int)
        )
    
        emerging_trajectory["change_2024_to_2025"] = (
            emerging_trajectory[2025]
            - emerging_trajectory[2024]
        ).astype(int)
    
    # ------------------------------------------------------------------------------
    # 5. Add device context
    # ------------------------------------------------------------------------------
    
    device_context_step7 = (
        step7_df[
            step7_df["product_code"].astype(str).isin(emerging_codes_step7)
        ][
            [
                "product_code",
                "generic_device_name",
                "medical_specialty",
                "device_class"
            ]
        ]
        .drop_duplicates("product_code")
    )
    
    if emerging_trajectory.empty:
        emerging_trajectory_output = pd.DataFrame(
            columns=[
                "product_code",
                "generic_device_name",
                "medical_specialty",
                "device_class",
                *trajectory_years,
                "total_records_2021_2025",
                "active_years",
                "peak_year",
                "peak_activity",
                "change_2024_to_2025"
            ]
        )
    else:
        emerging_trajectory_output = (
            emerging_trajectory
            .reset_index()
            .merge(
                device_context_step7,
                on="product_code",
                how="left",
                validate="one_to_one"
            )
        )
    
        output_columns_step7 = [
            "product_code",
            "generic_device_name",
            "medical_specialty",
            "device_class",
            *trajectory_years,
            "total_records_2021_2025",
            "active_years",
            "peak_year",
            "peak_activity",
            "change_2024_to_2025"
        ]
    
        emerging_trajectory_output = (
            emerging_trajectory_output[output_columns_step7]
            .sort_values(
                "total_records_2021_2025",
                ascending=False
            )
            .reset_index(drop=True)
        )
    
    # ------------------------------------------------------------------------------
    # 6. Display results
    # ------------------------------------------------------------------------------
    
    print("=== STEP 7: EMERGING SIGNAL TRAJECTORY ===")
    
    if emerging_trajectory_output.empty:
        print("No Product Codes are currently classified as recent-emerging signals.")
    else:
        print(
            emerging_trajectory_output
            .to_string(index=False)
        )
    
    # ------------------------------------------------------------------------------
    # 7. Analytical checks
    # ------------------------------------------------------------------------------
    
    print("\n=== STEP 7: ANALYTICAL CHECKS ===")
    
    if emerging_trajectory_output.empty:
        print("No emerging signal trajectories are available.")
    else:
        for _, row in emerging_trajectory_output.iterrows():
            print(
                f"{row['product_code']}: "
                f"{int(row[2021])} → {int(row[2022])} → "
                f"{int(row[2023])} → {int(row[2024])} → "
                f"{int(row[2025])} | "
                f"Total={int(row['total_records_2021_2025'])} | "
                f"Active years={int(row['active_years'])}"
            )
    
    # ------------------------------------------------------------------------------
    # 8. Analytical boundary
    # ------------------------------------------------------------------------------
    
    print("\n=== ANALYTICAL BOUNDARY ===")
    print(
        "These trajectories describe observed FDA record activity only. "
        "Increasing or sustained activity does not by itself establish "
        "market growth, physician adoption, clinical effectiveness, "
        "or commercial opportunity."
    )
    ```
    
- STEP 8: Dashboard — FDA AI/ML Medical Device Activity
    
    ```python
    # ==============================================================================
    # STEP 8: Executive Dashboard — FDA AI/ML Medical Device Activity
    # ==============================================================================
    # Purpose:
    # Present the validated analytical findings in a dashboard focused on
    # observed FDA regulatory activity.
    #
    # Analytical principles:
    # - Uses observed FDA records only.
    # - 2026 is excluded because it is an incomplete year.
    # - All dashboard metrics are calculated directly from df_integrated.
    # - Recent-emerging signals follow the predefined activity-profile criteria.
    # - Medical specialty values are displayed using FDA source codes.
    # - Regulatory activity does not represent market demand, adoption,
    #   clinical effectiveness, or commercial success.
    # ==============================================================================
    
    import plotly.graph_objects as go
    from plotly.subplots import make_subplots
    
    # ==============================================================================
    # 1. Validate source structure
    # ==============================================================================
    
    required_columns = [
        "product_code",
        "submission_number",
        "decision_year",
        "medical_specialty",
        "generic_device_name",
        "device_class"
    ]
    
    missing_columns = [
        column
        for column in required_columns
        if column not in df_integrated.columns
    ]
    
    if missing_columns:
        raise KeyError(
            f"Required columns are missing from df_integrated: {missing_columns}"
        )
    
    # ==============================================================================
    # 2. Prepare completed-year analysis data
    # ==============================================================================
    
    dashboard_df = df_integrated.copy()
    
    dashboard_df["decision_year"] = pd.to_numeric(
        dashboard_df["decision_year"],
        errors="coerce"
    )
    
    dashboard_df = dashboard_df[
        dashboard_df["decision_year"].notna()
        & (dashboard_df["decision_year"] <= 2025)
    ].copy()
    
    dashboard_df["decision_year"] = (
        dashboard_df["decision_year"].astype(int)
    )
    
    total_completed_records = int(len(dashboard_df))
    
    analysis_start_year = int(
        dashboard_df["decision_year"].min()
    )
    
    analysis_end_year = int(
        dashboard_df["decision_year"].max()
    )
    
    # ==============================================================================
    # 3. Annual activity
    # ==============================================================================
    
    annual_activity = (
        dashboard_df
        .groupby("decision_year")
        .size()
        .reset_index(name="record_count")
        .sort_values("decision_year")
    )
    
    # ==============================================================================
    # 4. Medical specialty concentration
    # ==============================================================================
    
    specialty_dashboard = (
        dashboard_df[
            dashboard_df["medical_specialty"].notna()
            & dashboard_df["medical_specialty"]
            .astype(str)
            .str.strip()
            .ne("")
        ]
        .groupby("medical_specialty")
        .size()
        .reset_index(name="record_count")
        .sort_values(
            "record_count",
            ascending=False
        )
    )
    
    specialty_assigned_total = int(
        specialty_dashboard["record_count"].sum()
    )
    
    if specialty_assigned_total > 0:
        specialty_dashboard["record_share_pct"] = (
            specialty_dashboard["record_count"]
            / specialty_assigned_total
            * 100
        ).round(1)
    else:
        specialty_dashboard["record_share_pct"] = 0.0
    
    specialty_display = (
        specialty_dashboard
        .head(6)
        .sort_values(
            "record_count",
            ascending=True
        )
        .copy()
    )
    
    if specialty_dashboard.empty:
        top_specialty = "N/A"
        top_specialty_count = 0
        top_specialty_share = 0.0
    else:
        top_specialty_row = specialty_dashboard.iloc[0]
        top_specialty = str(top_specialty_row["medical_specialty"])
        top_specialty_count = int(top_specialty_row["record_count"])
        top_specialty_share = float(top_specialty_row["record_share_pct"])
    
    # ==============================================================================
    # 5. Build product-level activity profile
    # ==============================================================================
    
    trajectory_years = [2021, 2022, 2023, 2024, 2025]
    
    annual_product_activity = (
        dashboard_df
        .groupby(["product_code", "decision_year"])
        .size()
        .unstack(fill_value=0)
    )
    
    for year in trajectory_years:
        if year not in annual_product_activity.columns:
            annual_product_activity[year] = 0
    
    annual_product_activity = (
        annual_product_activity[trajectory_years]
        .copy()
    )
    
    annual_product_activity.index.name = "product_code"
    annual_product_activity = annual_product_activity.reset_index()
    
    product_profile = (
        dashboard_df
        .groupby("product_code")
        .agg(
            total_records=("submission_number", "count"),
            first_year=("decision_year", "min"),
            last_year=("decision_year", "max"),
            generic_device_name=("generic_device_name", "first"),
            medical_specialty=("medical_specialty", "first"),
            device_class=("device_class", "first")
        )
        .reset_index()
    )
    
    product_profile = (
        product_profile
        .merge(
            annual_product_activity,
            on="product_code",
            how="left",
            validate="one_to_one"
        )
    )
    
    product_profile["recent_3y_records"] = (
        product_profile[[2023, 2024, 2025]].sum(axis=1)
    )
    
    product_profile["active_years_2021_2025"] = (
        product_profile[trajectory_years].gt(0).sum(axis=1)
    )
    
    # ==============================================================================
    # 6. Apply predefined recent-emerging activity criteria
    # ==============================================================================
    
    product_profile["activity_profile"] = np.select(
        [
            (
                (product_profile["recent_3y_records"] >= 5)
                & (product_profile["first_year"] >= 2021)
                & (product_profile["active_years_2021_2025"] >= 2)
            ),
            (
                (product_profile["recent_3y_records"] >= 5)
                & (product_profile["first_year"] < 2021)
                & (product_profile["active_years_2021_2025"] >= 3)
            )
        ],
        [
            "Recent-emerging signal",
            "Established recent activity"
        ],
        default="Lower-volume / insufficient signal"
    )
    
    emerging_codes = (
        product_profile.loc[
            product_profile["activity_profile"] == "Recent-emerging signal",
            "product_code"
        ]
        .dropna()
        .astype(str)
        .drop_duplicates()
        .tolist()
    )
    
    # ==============================================================================
    # 7. Prepare emerging-signal trajectory data
    # ==============================================================================
    
    trajectory_dashboard = (
        product_profile[
            product_profile["product_code"]
            .astype(str)
            .isin(emerging_codes)
        ]
        .copy()
    )
    
    if not trajectory_dashboard.empty:
        trajectory_dashboard["total_records"] = (
            trajectory_dashboard[trajectory_years]
            .sum(axis=1)
            .astype(int)
        )
    
        trajectory_dashboard["active_years"] = (
            trajectory_dashboard[trajectory_years]
            .gt(0)
            .sum(axis=1)
            .astype(int)
        )
    
        trajectory_dashboard["peak_year"] = (
            trajectory_dashboard[trajectory_years]
            .idxmax(axis=1)
            .astype(int)
        )
    
        trajectory_dashboard["peak_activity"] = (
            trajectory_dashboard[trajectory_years]
            .max(axis=1)
            .astype(int)
        )
    
        trajectory_dashboard = (
            trajectory_dashboard
            .sort_values("total_records", ascending=False)
        )
    
    # ==============================================================================
    # 8. Design system
    # ==============================================================================
    
    COLOR_NAVY = "#17324D"
    COLOR_BLUE = "#176B87"
    COLOR_TEAL = "#218C8D"
    COLOR_BLUE_LIGHT = "#5B84A4"
    COLOR_AMBER = "#C98200"
    
    COLOR_TEXT = "#243B53"
    COLOR_MUTED = "#627D98"
    COLOR_GRID = "#DCE5EC"
    COLOR_BACKGROUND = "#F5F8FB"
    COLOR_WHITE = "#FFFFFF"
    COLOR_BORDER = "#D7E1E8"
    
    SIGNAL_COLORS = [
        COLOR_BLUE,
        COLOR_BLUE_LIGHT,
        COLOR_TEAL,
        COLOR_AMBER
    ]
    
    # ==============================================================================
    # 9. Dashboard layout
    # ==============================================================================
    
    fig = make_subplots(
        rows=2,
        cols=2,
        specs=[
            [{"type": "xy"}, {"type": "xy"}],
            [{"type": "xy"}, {"type": "table"}]
        ],
        row_heights=[0.44, 0.44],
        column_widths=[0.48, 0.48],
        vertical_spacing=0.22,
        horizontal_spacing=0.08,
        subplot_titles=(
            "<b>Annual Activity Trend</b><br><span style='font-size:10px;color:#627D98;font-weight:normal;'>FDA records by decision year</span>",
            "<b>Specialty Concentration</b><br><span style='font-size:10px;color:#627D98;font-weight:normal;'>Share of records with an assigned specialty</span>",
            "<b>Recent-Emerging Signal Trajectories</b><br><span style='font-size:10px;color:#627D98;font-weight:normal;'>Annual FDA records for identified Product Codes</span>",
            "<b>Emerging Signal Summary</b><br><span style='font-size:10px;color:#627D98;font-weight:normal;'>Activity profile of identified Product Codes</span>"
        )
    )
    
    # ==============================================================================
    # 10. Chart 1 — Annual activity trend
    # ==============================================================================
    
    fig.add_trace(
        go.Scatter(
            x=annual_activity["decision_year"],
            y=annual_activity["record_count"],
            mode="lines+markers",
            line=dict(color=COLOR_BLUE, width=3),
            marker=dict(color=COLOR_BLUE, size=6),
            hovertemplate="<b>%{x}</b><br>FDA records: %{y:,}<extra></extra>",
            showlegend=False
        ),
        row=1,
        col=1
    )
    
    # ==============================================================================
    # 11. Chart 2 — Specialty concentration
    # ==============================================================================
    
    specialty_text = [
        f"<b>{int(count):,}</b> ({share:.1f}%)"
        for count, share in zip(
            specialty_display["record_count"],
            specialty_display["record_share_pct"]
        )
    ]
    
    fig.add_trace(
        go.Bar(
            x=specialty_display["record_count"],
            y=specialty_display["medical_specialty"].astype(str),
            orientation="h",
            marker=dict(color=COLOR_TEAL),
            text=specialty_text,
            textposition="outside",
            cliponaxis=False,
            showlegend=False,
            hovertemplate="<b>%{y}</b><br>FDA records: %{x:,}<extra></extra>"
        ),
        row=1,
        col=2
    )
    
    # ==============================================================================
    # 12. Chart 3 — Recent-emerging signal trajectories
    # ==============================================================================
    
    for index, code in enumerate(
        trajectory_dashboard["product_code"].astype(str).tolist()
    ):
        signal_row = trajectory_dashboard[
            trajectory_dashboard["product_code"].astype(str) == code
        ].iloc[0]
    
        line_color = SIGNAL_COLORS[index % len(SIGNAL_COLORS)]
        y_values = [int(signal_row[year]) for year in trajectory_years]
    
        fig.add_trace(
            go.Scatter(
                x=trajectory_years,
                y=y_values,
                mode="lines+markers+text",
                line=dict(color=line_color, width=2.5),
                marker=dict(color=line_color, size=6),
                text=["" if year != 2025 else f"<b>{code}</b>" for year in trajectory_years],
                textposition="middle right",
                cliponaxis=False,
                showlegend=False,
                hovertemplate=(
                    f"<b>{code}</b><br>"
                    "Year: %{x}<br>"
                    "FDA records: %{y:,}"
                    "<extra></extra>"
                )
            ),
            row=2,
            col=1
        )
    
    # ==============================================================================
    # 13. Chart 4 — Emerging signal summary
    # ==============================================================================
    
    if trajectory_dashboard.empty:
        table_values = [[], [], [], [], []]
    else:
        table_values = [
            trajectory_dashboard["product_code"].astype(str).tolist(),
            trajectory_dashboard["total_records"].astype(int).tolist(),
            trajectory_dashboard["active_years"].astype(int).tolist(),
            trajectory_dashboard["peak_year"].astype(int).tolist(),
            trajectory_dashboard["peak_activity"].astype(int).tolist()
        ]
    
    fig.add_trace(
        go.Table(
            columnwidth=[100, 100, 100, 100, 100],
            header=dict(
                values=[
                    "<b>Product Code</b>",
                    "<b>Total Records</b>",
                    "<b>Active Years</b>",
                    "<b>Peak Year</b>",
                    "<b>Peak Activity</b>"
                ],
                align="center",
                fill_color=COLOR_NAVY,
                font=dict(color=COLOR_WHITE, size=11),
                height=32,
                line=dict(color=COLOR_WHITE, width=1)
            ),
            cells=dict(
                values=table_values,
                align="center",
                fill_color=COLOR_WHITE,
                font=dict(color=COLOR_TEXT, size=11),
                height=32,
                line=dict(color=COLOR_BORDER, width=1)
            )
        ),
        row=2,
        col=2
    )
    
    # ==============================================================================
    # 14. Dashboard title & Subtitle
    # ==============================================================================
    
    fig.add_annotation(
        x=0.5,
        y=1.30,
        xref="paper",
        yref="paper",
        text="<b>FDA AI/ML Medical Device Activity</b>",
        showarrow=False,
        xanchor="center",
        yanchor="middle",
        font=dict(family="Arial", size=24, color=COLOR_NAVY)
    )
    
    fig.add_annotation(
        x=0.5,
        y=1.26,
        xref="paper",
        yref="paper",
        text=(
            f"Observed FDA regulatory activity through {analysis_end_year}  •  "
            f"{analysis_start_year}–{analysis_end_year} analysis window  •  "
            f"2026 excluded as incomplete"
        ),
        showarrow=False,
        xanchor="center",
        yanchor="middle",
        font=dict(family="Arial", size=11, color=COLOR_MUTED)
    )
    
    # ==============================================================================
    # 15. KPI cards
    # ==============================================================================
    
    kpi_cards = [
        (
            0.16,
            "COMPLETED-YEAR RECORDS",
            f"{total_completed_records:,}",
            "Observed FDA records"
        ),
        (
            0.50,
            "TOP SPECIALTY SHARE",
            f"{top_specialty_share:.1f}%",
            f"{top_specialty} • assigned specialty records"
        ),
        (
            0.84,
            "RECENT-EMERGING SIGNALS",
            f"{len(emerging_codes)}",
            "Product Codes meeting predefined criteria"
        )
    ]
    
    for x_position, label, value, context in kpi_cards:
        fig.add_annotation(
            x=x_position,
            y=1.13,
            xref="paper",
            yref="paper",
            text=(
                f"<span style='font-size:22px;font-weight:800;color:{COLOR_NAVY};'>{value}</span><br>"
                f"<span style='font-size:10px;font-weight:700;color:{COLOR_TEXT};letter-spacing:0.5px;'>{label}</span><br>"
                f"<span style='font-size:9px;color:{COLOR_MUTED};'>{context}</span>"
            ),
            showarrow=False,
            xanchor="center",
            yanchor="bottom",
            align="center",
            width=230,
            height=58,
            bgcolor=COLOR_WHITE,
            bordercolor=COLOR_BORDER,
            borderwidth=1,
            borderpad=6,
            font=dict(family="Arial", color=COLOR_TEXT)
        )
    
    # ==============================================================================
    # 16. Axis formatting
    # ==============================================================================
    
    fig.update_xaxes(
        title_text="Decision Year",
        showgrid=True,
        gridcolor=COLOR_GRID,
        zeroline=False,
        tickfont=dict(size=9, color=COLOR_MUTED),
        title_font=dict(size=10, color=COLOR_MUTED),
        row=1,
        col=1
    )
    
    fig.update_yaxes(
        title_text="FDA Records",
        showgrid=True,
        gridcolor=COLOR_GRID,
        zeroline=False,
        tickfont=dict(size=9, color=COLOR_MUTED),
        title_font=dict(size=10, color=COLOR_MUTED),
        row=1,
        col=1
    )
    
    fig.update_xaxes(
        title_text="FDA Records",
        showgrid=True,
        gridcolor=COLOR_GRID,
        zeroline=False,
        tickfont=dict(size=9, color=COLOR_MUTED),
        title_font=dict(size=10, color=COLOR_MUTED),
        row=1,
        col=2
    )
    
    fig.update_yaxes(
        title_text="Medical Specialty Code",
        showgrid=False,
        tickfont=dict(size=10, color=COLOR_TEXT),
        title_font=dict(size=10, color=COLOR_MUTED),
        row=1,
        col=2
    )
    
    fig.update_xaxes(
        title_text="Decision Year",
        dtick=1,
        range=[2020.7, 2025.8],
        showgrid=True,
        gridcolor=COLOR_GRID,
        zeroline=False,
        tickfont=dict(size=9, color=COLOR_MUTED),
        title_font=dict(size=10, color=COLOR_MUTED),
        row=2,
        col=1
    )
    
    fig.update_yaxes(
        title_text="FDA Records",
        dtick=1,
        showgrid=True,
        gridcolor=COLOR_GRID,
        zeroline=False,
        tickfont=dict(size=9, color=COLOR_MUTED),
        title_font=dict(size=10, color=COLOR_MUTED),
        row=2,
        col=1
    )
    
    # Subplot title positioning & spacing adjustment
    for annotation in fig["layout"]["annotations"]:
        if annotation["text"].startswith("<b>") and "style=" in annotation["text"]:
            annotation["font"] = dict(family="Arial", size=13, color=COLOR_NAVY)
            annotation["y"] = annotation["y"] + 0.02
    
    # ==============================================================================
    # 17. Final visualization
    # ==============================================================================
    
    fig.update_layout(
        height=980,
        paper_bgcolor=COLOR_BACKGROUND,
        plot_bgcolor=COLOR_WHITE,
        font=dict(family="Arial", size=10, color=COLOR_TEXT),
        margin=dict(l=65, r=65, t=240, b=50),
        showlegend=False
    )
    
    # ==============================================================================
    # 18. Render dashboard
    # ==============================================================================
    
    fig.show()
    
    # ==============================================================================
    # 19. Dashboard validation summary
    # ==============================================================================
    
    print("=" * 68)
    print("STEP 8 — DASHBOARD DATA VALIDATION")
    print("=" * 68)
    print(f"Completed-year FDA records: {total_completed_records:,}")
    print(f"Records with assigned medical specialty: {specialty_assigned_total:,}")
    print(f"Top medical specialty: {top_specialty} ({top_specialty_count:,} records, {top_specialty_share:.1f}%)")
    print(f"Recent-emerging Product Codes: {len(emerging_codes)}")
    print("Trajectory analysis window: 2021–2025")
    print("2026 excluded: incomplete year")
    print("=" * 68)
    ```
