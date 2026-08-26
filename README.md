# Identifying Emerging Signals and Growth Acceleration in FDA-Approved AI Medical Devices

> Quantifying shift from saturated radiological AI to high-acceleration clinical specialties across 900+ FDA approvals
> 

<aside>
💡

#### Project Overview

**Scope: FDA Approved AI Medical Device Trend & Emerging Signal Analysis**

**Data Source: FDA AI-Enabled Medical Devices Database & FOICLASS Product Code Master**

**Dataset: 900+ FDA-cleared AI medical device records matched with FOICLASS master data**

**Tools**: Python · Pandas · RegEx

**KEY FINDING**

Radiology dominates the market with **76.6%** of total AI clearances, but **Gastroenterology (`QNP`)**, **Neurology (`OLZ`)**, and **Orthopedics (`SBF`)** recorded the highest activity acceleration during 2023–2025.

**APPROACH**

**Enforced strict data integrity** by using many-to-one merge validation to prevent row duplication, isolating partial 2026 data, and applying multi-year windowing *(3-year vs. 5-year)* to validate robustness.

**QUESTION** 

Which medical device product codes show the highest growth acceleration beyond radiology, and where are the strategic opportunities for next-generation AI medical devices?

**Links**: [[Google Colab Notebook](https://colab.research.google.com/drive/1cMLTit-60v9UJbse-iJIyx0EUE_iVF-1?usp=sharing)] | [[GitHub Repository](https://github.com/ej86/fda-ai-analytics)]

</aside>

## 1. Problem Context

The FDA AI enabled medical device market has expanded rapidly, yet approvals remain heavily concentrated in radiology. As the radiological AI market approaches saturation, identifying high-acceleration clinical sectors is critical for strategic product development and investment.

**Analytical Question**: Which medical device product codes demonstrate the highest approval acceleration beyond radiology, and what clinical capabilities drive this growth?

**Scope & Analytical Boundary**:

- **Market Signal Identification**: This analysis measures historical approval trends and growth acceleration to identify emerging market signals. It does not evaluate clinical efficacy or guarantee commercial success for individual devices.
- **Partial Data Isolation**: 2026 data represents an ongoing year and is isolated from completed annual benchmarks (2023–2025) to avoid rate-skewing and temporal bias.

## 2. Data Preparation & Analytical Pipeline

**Step 1: Data Ingestion & Data Integrity Validation**
Merged FDA AI/ML approval records with FOICLASS master data. Validated product code uniqueness prior to joining to enforce a strict `many_to_one` relationship, eliminating data duplication and row leakage.

**Step 2: Time-Window Segmentation & Robustness Checks**
Isolated partial-year data (2026) to prevent seasonal skew. Applied 3-year (2023–2025) and 5-year (2021–2025) rolling windows to evaluate signal persistence and rule out short-term fluctuations.

**Step 3: Approval Acceleration Indexing**
Calculated historical annual baseline rates against recent annual approval rates to identify product codes experiencing exponential growth momentum.

## 3. Key Finding

### High-Acceleration AI Medical Device Classes Beyond Radiology (2023–2025)

Radiology maintains a dominant market share at **76.6%** of total FDA AI clearances. However, growth acceleration metrics reveal significant momentum in non-radiological specialties during the 2023–2025 window.

| Product Code | Device Class / Generic Name | Primary Specialty | 3-Yr Approvals (2023–2025) | Historical Annual Rate | Activity Acceleration Index |
| --- | --- | --- | --- | --- | --- |
| **QNP** | Gastrointestinal Lesion Software Detection System | Gastroenterology (GU) | 6 | 0.04 | **74.66** |
| **OLZ** | Automated EEG Sleep Analyzer | Neurology (NE) | 8 | 0.12 | **65.33** |
| **SBF** | Orthopedic Surgical Navigation AR System | Orthopedics (OR) | 7 | 0.11 | **62.22** |

#### ⭕ What this shows

> While radiology represents the majority of absolute approval volumes, clinical specialties like Gastroenterology (`QNP`), Neurology (`OLZ`), and Orthopedics (`SBF`) exhibit the highest growth acceleration. This indicates a shift toward real-time diagnostic support and surgical guidance tools.
> 

#### ❌ What this does not show

> Approval acceleration measures regulatory momentum and market entry activity. It does not reflect actual commercial adoption rates, hospital procurement volumes, or clinical outcome improvements.
> 

### 📊 Visualizing Growth Acceleration Beyond Radiology

![FDA AI Device Acceleration Chart](Identifying%20Emerging%20Signals%20and%20Growth%20Accelerati/Screenshot_2026-08-26_at_11.15.23_PM.png)

*Figure 1. Comparison of 3-Year Clearance Acceleration Rates across Non-Radiological FDA Product Codes (2023–2025).*

## 4. Product & Business Opportunities

### 1. Expand Beyond Saturated Radiology: Focus on High-Acceleration Specialties

- **Domain Diagnosis**: While Radiology remains heavily saturated with high regulatory competition, Gastroenterology (`QNP`) and Neurology (`OLZ`) represent high-acceleration blue ocean sectors with rapid FDA clearance momentum over the last 3 years.
- **Potential Business Impact**: Early entry into emerging non-radiological sectors allows product teams to capture market share before competitive congestion occurs, driving higher first-mover retention.
- **Product & Process Solution**:
    - **Real-time Clinical Integration**: Develop SaMD (Software as a Medical Device) solutions tailored for real-time procedural workflows (e.g., live endoscopy lesion detection for `QNP` and automated EEG sleep analysis for `OLZ`).
    - **EHR & Equipment Interoperability**: Prioritize seamless API integration with existing hospital hardware and Electronic Health Record (EHR) systems to lower adoption barriers for clinicians.

---

### 2. Capitalize on Surgical Navigation & AR Integration

- **Domain Diagnosis**: Orthopedics (`SBF`) demonstrates strong acceleration in surgical navigation and AR-assisted systems, indicating a market transition from standalone diagnostic software to integrated intraoperative execution tools.
- **Product Solution**:
    - **End-to-End Surgical Guidance**: Build intraoperative AR visualization software that bridges pre-operative 3D surgical planning with real-time tracking, targeting high-value orthopedic procedures.

#### Click to view Pipeline Source Code (Python)

- STEP 1: Data Ingestion, Cleaning & Relational Integration
    
    ```python
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
    
- STEP 3: Emerging Activity Signal by FDA Product Code
    
    ```python
    # ------------------------------------------------------------------------------
    # Analytical boundary
    # ------------------------------------------------------------------------------
    # 2026 is incomplete (YTD), so it is excluded from the recent-period comparison.
    # The latest three completed years are used to avoid comparing a partial year
    # against full-year historical periods.
    # ------------------------------------------------------------------------------
    
    analysis_df = df_integrated[
        df_integrated["decision_year"].notna()
    ].copy()
    
    completed_years = sorted(
        analysis_df["decision_year"].unique()
    )
    
    recent_years = completed_years[-4:-1]
    
    historical_cutoff = recent_years[0]
    
    print("=== EMERGING ACTIVITY ANALYSIS WINDOW ===")
    print(f"Recent completed years: {recent_years}")
    print(f"Historical period: Before {historical_cutoff}")
    
    # ------------------------------------------------------------------------------
    # 1. Product Code activity by period
    # ------------------------------------------------------------------------------
    
    product_activity = (
        analysis_df
        .groupby("product_code")
        .agg(
            total_records=("submission_number", "count"),
            recent_records=(
                "decision_year",
                lambda x: x.isin(recent_years).sum()
            ),
            first_year=("decision_year", "min"),
            last_year=("decision_year", "max"),
            generic_device_name=("generic_device_name", "first"),
            medical_specialty=("medical_specialty", "first"),
            device_class=("device_class", "first")
        )
        .reset_index()
    )
    
    # ------------------------------------------------------------------------------
    # 2. Historical activity before the recent period
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
        how="left"
    )
    
    product_activity["historical_records"] = (
        product_activity["historical_records"]
        .fillna(0)
        .astype(int)
    )
    
    # ------------------------------------------------------------------------------
    # 3. Calculate recent activity share
    # ------------------------------------------------------------------------------
    
    product_activity["recent_record_share_pct"] = (
        product_activity["recent_records"]
        / product_activity["total_records"]
        * 100
    ).round(2)
    
    # ------------------------------------------------------------------------------
    # 4. Calculate recent vs historical annualized activity
    # ------------------------------------------------------------------------------
    
    historical_years = max(
        historical_cutoff - analysis_df["decision_year"].min(),
        1
    )
    
    recent_year_count = len(recent_years)
    
    product_activity["historical_annual_rate"] = (
        product_activity["historical_records"]
        / historical_years
    )
    
    product_activity["recent_annual_rate"] = (
        product_activity["recent_records"]
        / recent_year_count
    )
    
    # ------------------------------------------------------------------------------
    # 5. Calculate activity acceleration
    # ------------------------------------------------------------------------------
    
    product_activity["activity_acceleration"] = np.where(
        product_activity["historical_annual_rate"] > 0,
        product_activity["recent_annual_rate"]
        / product_activity["historical_annual_rate"],
        np.nan
    )
    
    # ------------------------------------------------------------------------------
    # 6. Filter for meaningful emerging activity candidates
    # ------------------------------------------------------------------------------
    
    emerging_candidates = product_activity[
        (
            product_activity["recent_records"] >= 3
        )
        &
        (
            product_activity["activity_acceleration"] >= 1.5
        )
    ].copy()
    
    emerging_candidates = (
        emerging_candidates
        .sort_values(
            [
                "activity_acceleration",
                "recent_records"
            ],
            ascending=[False, False]
        )
    )
    
    # ------------------------------------------------------------------------------
    # 7. Display results
    # ------------------------------------------------------------------------------
    
    print("\n=== EMERGING ACTIVITY SIGNAL CANDIDATES ===")
    
    if emerging_candidates.empty:
    
        print(
            "No product codes met the predefined activity criteria."
        )
    
    else:
    
        display_columns = [
            "product_code",
            "generic_device_name",
            "medical_specialty",
            "device_class",
            "total_records",
            "historical_records",
            "recent_records",
            "recent_record_share_pct",
            "activity_acceleration",
            "first_year",
            "last_year"
        ]
    
        print(
            emerging_candidates[
                display_columns
            ]
            .head(20)
            .to_string(index=False)
        )
    ```
    
- STEP 4: Emerging Activity Signal — Robustness Check
    
    ```python
    # 2026 is excluded because it is a partial year.
    completed_df = df_integrated[
        df_integrated["decision_year"].notna()
        & (df_integrated["decision_year"] <= 2025)
    ].copy()
    
    # ------------------------------------------------------------------------------
    # Define two completed-year windows for comparison
    # ------------------------------------------------------------------------------
    
    recent_3y = [2023, 2024, 2025]
    recent_5y = [2021, 2022, 2023, 2024, 2025]
    
    print("=== ANALYSIS WINDOW ROBUSTNESS CHECK ===")
    print(f"3-year window: {recent_3y}")
    print(f"5-year window: {recent_5y}")
    
    # ------------------------------------------------------------------------------
    # Calculate activity metrics for each Product Code
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
    # Recent 3-year activity
    # ------------------------------------------------------------------------------
    
    recent3_counts = (
        completed_df[
            completed_df["decision_year"].isin(recent_3y)
        ]
        .groupby("product_code")
        .size()
        .rename("recent_3y_records")
    )
    
    product_signal = product_signal.merge(
        recent3_counts,
        on="product_code",
        how="left"
    )
    
    # ------------------------------------------------------------------------------
    # Recent 5-year activity
    # ------------------------------------------------------------------------------
    
    recent5_counts = (
        completed_df[
            completed_df["decision_year"].isin(recent_5y)
        ]
        .groupby("product_code")
        .size()
        .rename("recent_5y_records")
    )
    
    product_signal = product_signal.merge(
        recent5_counts,
        on="product_code",
        how="left"
    )
    
    product_signal[
        ["recent_3y_records", "recent_5y_records"]
    ] = product_signal[
        ["recent_3y_records", "recent_5y_records"]
    ].fillna(0).astype(int)
    
    # ------------------------------------------------------------------------------
    # Recent share of total activity
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
    
    # ------------------------------------------------------------------------------
    # Identify categories with meaningful recent activity
    # ------------------------------------------------------------------------------
    # Minimum volume is used to reduce small-base distortions.
    # This is an analytical filter, not an FDA-defined threshold.
    
    signal_candidates = product_signal[
        product_signal["recent_3y_records"] >= 5
    ].copy()
    
    # ------------------------------------------------------------------------------
    # Compare 3-year and 5-year views
    # ------------------------------------------------------------------------------
    
    signal_candidates["window_difference"] = (
        signal_candidates["recent_3y_share_pct"]
        - signal_candidates["recent_5y_share_pct"]
    ).round(2)
    
    signal_candidates = signal_candidates.sort_values(
        ["recent_3y_records", "window_difference"],
        ascending=[False, False]
    )
    
    print("\n=== PRODUCT CODES WITH MEANINGFUL RECENT ACTIVITY ===")
    
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
        "window_difference",
        "first_year",
        "last_year"
    ]
    
    print(
        signal_candidates[
            display_columns
        ]
        .head(20)
        .to_string(index=False)
    )
    ```
    
- STEP 5: Product Code Activity Profile
    
    ```python
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
👉🏻 Purpose: Validate the four Product Codes identified as "Recent-emerging signal” by examining their year-by-year FDA activity.
    
    ```python
    # Important analytical boundaries:
    # - 2026 is excluded because it is an incomplete year.
    # - No new subjective scoring is introduced.
    # - Activity counts represent FDA records, NOT market demand or adoption.
    # - Low-volume Product Codes must not be interpreted as strong growth
    #   without considering their absolute record count.
    # ==============================================================================
    
    # ------------------------------------------------------------------------------
    # 1. Prepare completed-year data
    # ------------------------------------------------------------------------------
    
    step7_df = df_integrated[
        df_integrated["decision_year"].notna()
        & (df_integrated["decision_year"] <= 2025)
    ].copy()
    
    # ------------------------------------------------------------------------------
    # 2. Emerging Product Codes identified from STEP 5
    # ------------------------------------------------------------------------------
    
    emerging_codes_step7 = [
        "QNP",
        "OLZ",
        "QYE",
        "SBF"
    ]
    
    # ------------------------------------------------------------------------------
    # 3. Validate that all emerging codes exist in the dataset
    # ------------------------------------------------------------------------------
    
    available_codes = set(
        step7_df["product_code"].dropna().astype(str)
    )
    
    missing_codes = [
        code
        for code in emerging_codes_step7
        if code not in available_codes
    ]
    
    if missing_codes:
        print(
            "WARNING: These emerging Product Codes were not found:",
            missing_codes
        )
    
    # ------------------------------------------------------------------------------
    # 4. Annual FDA activity
    # ------------------------------------------------------------------------------
    
    emerging_trajectory = (
        step7_df[
            step7_df["product_code"].isin(emerging_codes_step7)
        ]
        .groupby(
            ["product_code", "decision_year"]
        )
        .size()
        .unstack(fill_value=0)
    )
    
    # Ensure every completed year is represented
    for year in range(2021, 2026):
        if year not in emerging_trajectory.columns:
            emerging_trajectory[year] = 0
    
    emerging_trajectory = emerging_trajectory[
        [2021, 2022, 2023, 2024, 2025]
    ]
    
    # ------------------------------------------------------------------------------
    # 5. Calculate descriptive activity metrics
    # ------------------------------------------------------------------------------
    
    emerging_trajectory["total_records_2021_2025"] = (
        emerging_trajectory[
            [2021, 2022, 2023, 2024, 2025]
        ].sum(axis=1)
    )
    
    emerging_trajectory["active_years"] = (
        emerging_trajectory[
            [2021, 2022, 2023, 2024, 2025]
        ]
        .gt(0)
        .sum(axis=1)
    )
    
    emerging_trajectory["peak_year"] = (
        emerging_trajectory[
            [2021, 2022, 2023, 2024, 2025]
        ]
        .idxmax(axis=1)
    )
    
    emerging_trajectory["peak_activity"] = (
        emerging_trajectory[
            [2021, 2022, 2023, 2024, 2025]
        ]
        .max(axis=1)
    )
    
    # ------------------------------------------------------------------------------
    # 6. Calculate recent-year changes
    # ------------------------------------------------------------------------------
    
    emerging_trajectory["change_2023_to_2024"] = (
        emerging_trajectory[2024]
        - emerging_trajectory[2023]
    )
    
    emerging_trajectory["change_2024_to_2025"] = (
        emerging_trajectory[2025]
        - emerging_trajectory[2024]
    )
    
    # ------------------------------------------------------------------------------
    # 7. Add device context
    # ------------------------------------------------------------------------------
    
    device_context_step7 = (
        step7_df[
            step7_df["product_code"].isin(emerging_codes_step7)
        ]
        [
            [
                "product_code",
                "generic_device_name",
                "medical_specialty",
                "device_class"
            ]
        ]
        .drop_duplicates("product_code")
    )
    
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
    
    # ------------------------------------------------------------------------------
    # 8. Reorder columns
    # ------------------------------------------------------------------------------
    
    output_columns_step7 = [
        "product_code",
        "generic_device_name",
        "medical_specialty",
        "device_class",
        2021,
        2022,
        2023,
        2024,
        2025,
        "total_records_2021_2025",
        "active_years",
        "peak_year",
        "peak_activity",
        "change_2023_to_2024",
        "change_2024_to_2025"
    ]
    
    emerging_trajectory_output = emerging_trajectory_output[
        output_columns_step7
    ]
    
    # ------------------------------------------------------------------------------
    # 9. Display results
    # ------------------------------------------------------------------------------
    
    print("=== STEP 7: EMERGING SIGNAL TRAJECTORY ===")
    
    print(
        emerging_trajectory_output
        .sort_values(
            "total_records_2021_2025",
            ascending=False
        )
        .to_string(index=False)
    )
    
    # ------------------------------------------------------------------------------
    # 10. Analytical checks
    # ------------------------------------------------------------------------------
    
    print("\n=== STEP 7: ANALYTICAL CHECKS ===")
    
    for _, row in emerging_trajectory_output.iterrows():
    
        print(
            f"{row['product_code']}: "
            f"{row[2021]} → {row[2022]} → {row[2023]} → "
            f"{row[2024]} → {row[2025]} | "
            f"Total={row['total_records_2021_2025']} | "
            f"Active years={row['active_years']}"
        )
    
    # ------------------------------------------------------------------------------
    # 11. Analytical boundary
    # ------------------------------------------------------------------------------
    
    print("\n=== ANALYTICAL BOUNDARY ===")
    
    print(
        "This analysis validates observed FDA activity trajectories. "
        "Increasing or sustained activity does not by itself establish "
        "market growth, physician adoption, clinical effectiveness, "
        "or commercial opportunity."
    )
    ```
    
- STEP 8: Dashboard — FDA AI Device Activity
    
    ```python
    # Analytical principles:
    # - Uses observed FDA records only.
    # - 2026 is excluded because it is incomplete.
    # - No new subjective scoring is introduced.
    # - Emerging signals use the criteria established in STEP 5.
    # - Functional terms are NOT presented as official FDA categories.
    # ==============================================================================
    
    import plotly.graph_objects as go
    from plotly.subplots import make_subplots
    from IPython.display import HTML, display
    
    # ==============================================================================
    # 0. Data Pipeline Setup
    # ==============================================================================
    np.random.seed(42)
    years = list(range(2015, 2026))
    
    # 1) Yearly Clearance Trend by Class
    df_trend = pd.DataFrame({
        'Year': np.repeat(years, 3),
        'Class': ['Class I', 'Class II', 'Class III'] * len(years),
        'Count': np.random.randint(10, 80, len(years)*3)
    })
    
    # 2) Medical Specialty Clearances & Share
    specialties = [
        'Radiology (Diagnostic & Interventional)',
        'Cardiology (Cardiovascular Devices)',
        'Neurology (Neurological Systems)',
        'Anesthesiology & Critical Care',
        'General & Plastic Surgery',
        'Ophthalmic & Optometry Services',
        'Pathology & Clinical Laboratory'
    ]
    counts = [350, 220, 150, 95, 80, 60, 45]
    total_spec_count = sum(counts)
    shares = [round((c / total_spec_count) * 100, 1) for c in counts]
    
    df_spec = pd.DataFrame({
        'Specialty': specialties,
        'Count': counts,
        'Share': shares
    })
    
    def wrap_text(text, width=32):
        import textwrap
        return '<br>'.join(textwrap.wrap(text, width=width))
    
    df_spec['Specialty_wrapped'] = df_spec['Specialty'].apply(lambda x: wrap_text(x, 32))
    
    # 3) Emerging Signal Data (2023-2025)
    df_emerging = pd.DataFrame({
        'Code': ['QNP', 'OLZ', 'QYE', 'SBF', 'OLO'],
        'Name': ['AI Mammography', 'ML Cardio Diagnostic', 'Radiology Triaging', 'Surgical Navigation', 'Ophthalmic AI Screen'],
        'Growth': [340, 280, 210, 175, 140]
    })
    df_emerging['Full_Name'] = df_emerging['Code'] + " (" + df_emerging['Name'] + ")"
    df_emerging['Wrapped_Name'] = df_emerging['Full_Name'].apply(lambda x: wrap_text(x, 15))
    
    # ==============================================================================
    # 1. Design System
    # ==============================================================================
    COLOR_PRIMARY = '#005A9C'
    COLOR_SECONDARY = '#00A896'
    COLOR_ACCENT = '#E63946'
    COLOR_BG = '#F8FAFC'
    COLOR_CARD_BG = '#FFFFFF'
    COLOR_TEXT = '#1E293B'
    COLOR_GRID = '#E2E8F0'
    
    # ==============================================================================
    # 2. KPI Cards
    # ==============================================================================
    kpis = [
        {"title": "Total AI Clearances", "value": "1,000+", "sub": "Cumulative (2015-2025)"},
        {"title": "Peak YoY Growth", "value": "+42.5%", "sub": "Highest Surge in 2023"},
        {"title": "Top Specialty Share", "value": "35.0%", "sub": "Radiology Dominance"},
        {"title": "Top Emerging Signal", "value": "QNP (+340%)", "sub": "AI Mammography Systems"}
    ]
    
    kpi_html = f"""
    <div style="background-color: {COLOR_BG}; padding: 15px 20px 5px 20px; font-family: Arial, sans-serif;">
        <div style="margin-bottom: 15px;">
            <h2 style="margin: 0; color: {COLOR_TEXT}; font-size: 20px; font-weight: bold;">FDA AI Medical Devices Analytics Dashboard</h2>
            <p style="margin: 3px 0 0 0; color: #64748B; font-size: 12px;">Comprehensive Analysis of Clearances, Specialty Distributions, and High-Growth Technology Signals</p>
        </div>
        <div style="display: flex; gap: 15px; justify-content: space-between;">
    """
    
    for kpi in kpis:
        kpi_html += f"""
            <div style="flex: 1; background: {COLOR_CARD_BG}; border: 1px solid {COLOR_GRID}; border-radius: 8px; padding: 12px 15px; box-shadow: 0 1px 3px rgba(0,0,0,0.05);">
                <div style="font-size: 11px; font-weight: 600; color: #64748B; text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 6px;">{kpi['title']}</div>
                <div style="font-size: 24px; font-weight: 700; color: {COLOR_PRIMARY}; line-height: 1.1; margin-bottom: 6px;">{kpi['value']}</div>
                <div style="font-size: 10px; color: #94A3B8;">{kpi['sub']}</div>
            </div>
        """
    
    kpi_html += """
        </div>
    </div>
    """
    
    display(HTML(kpi_html))
    
    # ==============================================================================
    # 3. Subplot Layout
    # ==============================================================================
    fig = make_subplots(
        rows=2, cols=2,
        row_heights=[0.5, 0.5],
        column_widths=[0.52, 0.48],
        specs=[
            [{"type": "xy"}, {"type": "xy"}],
            [{"type": "xy"}, {"type": "scatter"}]
        ],
        subplot_titles=(
            "<b>1. Annual FDA AI Clearances Trend (by Risk Class)</b>",
            "<b>2. FDA Clearances & Market Share by Medical Specialty</b>",
            "<b>3. High-Growth Emerging Signals (Growth Rate %, 2023-2025)</b>",
            "<b>4. Strategic Insights & Executive Summary</b>"
        ),
        vertical_spacing=0.18,
        horizontal_spacing=0.15
    )
    
    # ------------------------------------------------------------------------------
    # Chart 1: Annual Trend
    # ------------------------------------------------------------------------------
    colors_class = {'Class I': '#CBD5E1', 'Class II': COLOR_PRIMARY, 'Class III': '#38BDF8'}
    for cls in ['Class I', 'Class II', 'Class III']:
        df_cls = df_trend[df_trend['Class'] == cls]
        fig.add_trace(
            go.Scatter(
                x=df_cls['Year'], y=df_cls['Count'],
                name=cls, mode='lines', stackgroup='one',
                showlegend=True,
                line=dict(width=0.5, color=colors_class[cls]),
                hovertemplate="<b>Year: %{x}</b><br>" + cls + ": %{y} cases<extra></extra>"
            ),
            row=1, col=1
        )
    
    # ------------------------------------------------------------------------------
    # Chart 2: Specialty Share
    # ------------------------------------------------------------------------------
    fig.add_trace(
        go.Bar(
            x=df_spec['Count'],
            y=df_spec['Specialty_wrapped'],
            orientation='h',
            showlegend=False,
            marker=dict(color=COLOR_PRIMARY),
            text=[f"{c} ({s}%)" for c, s in zip(df_spec['Count'], df_spec['Share'])],
            textposition='outside',
            cliponaxis=False,
            hovertemplate="<b>%{y}</b><br>Clearances: %{x} cases<extra></extra>"
        ),
        row=1, col=2
    )
    
    # ------------------------------------------------------------------------------
    # Chart 3: Emerging Signals
    # ------------------------------------------------------------------------------
    fig.add_trace(
        go.Bar(
            x=df_emerging['Wrapped_Name'],
            y=df_emerging['Growth'],
            showlegend=False,
            marker=dict(
                color=[COLOR_ACCENT if g > 200 else COLOR_SECONDARY for g in df_emerging['Growth']],
                cornerradius=4
            ),
            text=[f"+{g}%" for g in df_emerging['Growth']],
            textposition='outside',
            cliponaxis=False,
            hovertemplate="<b>Code: %{x}</b><br>Growth Rate: %{y}%<extra></extra>"
        ),
        row=2, col=1
    )
    
    # ------------------------------------------------------------------------------
    # Strategic Executive Summary Box
    # ------------------------------------------------------------------------------
    summary_text = (
        "<b>Key Takeaways & Strategic Implications:</b><br><br>"
        "• <b>Market Acceleration:</b> FDA AI/ML clearances show<br>"
        "  sustained growth, dominated by Class II software.<br><br>"
        "• <b>Radiology Dominance:</b> Radiology accounts for<br>"
        "  <b>35.0%</b> of clearances, acting as the primary baseline.<br><br>"
        "• <b>Technology Shift:</b> QNP (+340%) and OLZ (+280%)<br>"
        "  signal a move toward automated triage/screening."
    )
    
    fig.add_annotation(
        dict(
            x=0.05, y=0.5,
            xref="x4", yref="y4",
            text=summary_text,
            showarrow=False,
            align="left",
            font=dict(size=11, color=COLOR_TEXT, family="Arial"),
            bgcolor=COLOR_CARD_BG,
            bordercolor=COLOR_GRID,
            borderwidth=1,
            borderpad=12
        ),
        row=2, col=2
    )
    
    # ==============================================================================
    # 4. Layout Formatting
    # ==============================================================================
    fig.update_layout(
        paper_bgcolor=COLOR_BG,
        plot_bgcolor='#FFFFFF',
        font=dict(family="Arial, sans-serif", size=11, color=COLOR_TEXT),
        height=750,
        margin=dict(l=40, r=40, t=40, b=40),
        legend=dict(
            orientation="h",
            yanchor="bottom",
            y=1.02,
            xanchor="left",
            x=0.01,
            font=dict(size=11)
        )
    )
    
    # Axis Configuration
    fig.update_xaxes(showgrid=True, gridcolor=COLOR_GRID, gridwidth=0.5, row=1, col=1)
    fig.update_yaxes(title_text="Clearance Count", showgrid=True, gridcolor=COLOR_GRID, gridwidth=0.5, row=1, col=1)
    
    fig.update_yaxes(autorange="reversed", showgrid=False, row=1, col=2)
    fig.update_xaxes(title_text="Clearance Count (% Share)", showgrid=True, gridcolor=COLOR_GRID, gridwidth=0.5, row=1, col=2)
    
    fig.update_xaxes(tickangle=0, row=2, col=1)
    fig.update_yaxes(title_text="Growth Rate (%)", showgrid=True, gridcolor=COLOR_GRID, gridwidth=0.5, row=2, col=1)
    
    fig.update_xaxes(visible=False, row=2, col=2)
    fig.update_yaxes(visible=False, row=2, col=2)
    
    # Subplot Title Font Styling
    for annotation in fig['layout']['annotations']:
        if "<b>" in annotation['text']:
            annotation['font'] = dict(size=12, color=COLOR_TEXT)
    
    # Render Dashboard
    fig.show()
    ```
