# 🏨 Exploratory Data Analysis (EDA) on Atliq Grands: Hospitality Domain

## 📌 Project Overview
This project presents an end-to-end Exploratory Data Analysis (EDA) on the historical booking and property data of **Atliq Grands**, a prominent hotel chain in India. The goal of this analysis is to uncover the underlying patterns driving recent business struggles, derive key performance metrics, and provide actionable insights to optimize pricing, improve occupancy rates, and regain market share.

## 📉 Business Problem
**The Situation:** Atliq Grands operates a diverse portfolio of luxury and business properties across major Indian cities (Delhi, Mumbai, Bangalore, Hyderabad).

**The Complication:** The company is currently facing a significant decline in revenue and market share, heavily impacted by low occupancy rates and discrepancies between expected and realized revenue (revenue leakage).

**The Objective:** Analyze the data to identify the root causes of the decline and deliver data-driven recommendations.

## 📊 Dataset Structure
The project utilizes a star schema data structure, merging multiple dimension and fact tables:
* **`df_hotels` (Dimension):** Property-specific details (name, city, category - Business/Luxury).
* **`df_rooms` (Dimension):** Defines specific room classes (Standard, Elite, Premium, Presidential).
* **`df_date` (Dimension):** Maps check-in dates to months, week numbers, and day types (weekday/weekend).
* **`df_bookings` (Fact):** Granular transaction data including booking platforms, revenue realized, and status (Checked Out, Cancelled, No Show).
* **`df_aggregated_bookings` (Fact):** Daily aggregated metrics showing total successful bookings against total room capacity.
* **`new_data_august`:** Newly arrived records appended to historical transactions.

## ⚙️ Methodology & Workflow

### 1. Data Profiling & Cleaning
Conducted a thorough "health check" on the dataset to ensure data integrity:
* **Boolean Masking:** Removed invalid negative guest counts.
* **Outlier Treatment:** Applied the **3-Sigma Rule** to remove unrealistic revenue anomalies.
* **Datetime Standardization:** Converted all mixed-format text dates into standard pandas `datetime` objects.
* **Imputation:** Handled missing values strategically (e.g., median imputation for missing guest counts, retaining `NaN` for canceled booking ratings to avoid statistical bias).
* **Anomaly Removal:** Filtered out physically impossible occupancy rates (e.g., > 100% capacity where errors exceeded standard overbooking thresholds).

### 2. Data Transformation & Integration
Merged the star schema tables into a unified, rich dataset using pandas `merge` to enable deep slice-and-dice analysis. 

### 3. Feature Engineering
Derived critical hospitality KPIs to measure behavioral and financial performance:
* **Revenue Leakage:** `revenue_generated` - `revenue_realized` (Measures the exact financial cost of cancellations and no-shows).
* **Booking Lead Time:** Days between booking and check-in (Assesses reliance on last-minute bookings).
* **Length of Stay (LOS):** Total nights stayed per guest.
* **Occupancy Percentage:** `(Successful Bookings / Total Capacity) * 100` (The heartbeat of hotel performance).

## 💡 Phase 1: Macro-Level Insights
The EDA funnels from macro-level metrics down to specific operational behaviors. 
* Analyzed **Occupancy Percentage** and **Total Revenue (in Millions)** across different cities and hotel categories.
* Visualized performance using `Seaborn` and `Matplotlib` to instantly highlight top-performing regions and areas bleeding money due to high cancellation rates (~25%).

## 💡 Phase 2: Revenue Leakage & Cancellation Analysis
After understanding the macro-level performance, this phase drills down into the financial impact of customer behavior. Specifically, we focused on cancellations and no-shows, which the initial data profiling identified as the primary drivers of Atliq's recent revenue drop.

### Key Analysis Areas:
* **Status Distribution:** Evaluated the overall proportion of `Checked Out`, `Cancelled`, and `No Show` bookings across the 134,000+ historical records.
* **Quantifying Leakage:** Utilized the custom `revenue_leakage` metric (Expected Revenue - Realized Revenue) to pinpoint exactly which cities and property categories are bleeding the most money.
* **Platform Reliability:** Analyzed cancellation rates across different `booking_platform`s (e.g., *makeyourtrip*, *logtrip*, *direct online*) to see if specific third-party channels have disproportionately high drop-off rates.

### Core Insights Extracted:
* **The Cancellation Burden:** The analysis confirms a severe chain-wide cancellation rate of roughly 25%. Meaning 1 in every 4 reservations fails to materialize into realized cash.
* **Channel Quality vs. Quantity:** While third-party platforms bring in high volumes of bookings, they were evaluated to see if they generate speculative, low-conversion reservations compared to Atliq's 'direct' channels.
* **Strategic Adjustments:** By quantifying the exact financial loss tied to cancellations, this phase provides the mathematical baseline needed to justify implementing stricter non-refundable policies or calculating a safe "overbooking" threshold (e.g., 105% capacity) to offset expected no-shows.

## 💡 Phase 3: Booking Behavior Analysis (Lead Time & Length of Stay)
Having established the financial impact of cancellations, Phase 3 shifts focus to understanding the guest's booking journey. By analyzing the newly engineered behavioral metrics—**Booking Lead Time** and **Length of Stay (LOS)**—we aimed to uncover patterns that dictate inventory turnover and revenue predictability.

### Key Analysis Areas:
* **Booking Window Distribution:** Examined the variance in `booking_lead_time` (the days between booking and check-in) to determine whether properties are overly reliant on last-minute, short-notice bookings versus stable, long-term advance reservations.
* **Length of Stay (LOS) Patterns:** Analyzed the average `length_of_stay` across different cities, room classes (Standard to Presidential), and day types (weekday vs. weekend).
* **Behavior by Channel:** Investigated whether specific `booking_platform`s attract guests with distinctly different lead times or stay durations compared to Atliq's direct channels.

### Core Insights Extracted:
* **The Last-Minute Challenge:** By mapping out lead times, the analysis highlights the standard booking window. High volumes of short lead-time bookings indicate a reactive consumer base, making dynamic pricing, staffing, and inventory forecasting significantly more difficult.
* **LOS Impact on Occupancy:** A shorter average Length of Stay mathematically requires a higher volume of new, constant reservations just to maintain target occupancy rates. Pinpointing properties with depressed LOS provides a clear target for implementing "extended stay" discounts or weekend packages.
* **Business vs. Leisure Profiling:** Segmenting these behavioral metrics by hotel `category` (Business vs. Luxury) validates the different operational rhythms required to manage transient weekday corporate travelers versus longer-stay weekend luxury guests.

## 💡 Phase 4: Customer Satisfaction & Rating Analysis
The final phase of the EDA focuses on qualitative data—specifically, the `ratings_given` by guests. Correlating customer satisfaction with specific cities, properties, and room types is crucial for understanding the brand's perceived value and identifying operational bottlenecks.

### Key Analysis Areas:
* **City-Wise Rating Distribution:** Calculated the average ratings across the four major operating cities (Bangalore, Delhi, Hyderabad, Mumbai).
* **Impact of Cancellations on Ratings:** Evaluated whether properties with higher cancellation or overbooking rates also suffer from depressed customer ratings.
* **Property-Specific Performance:** Drilled down into individual hotel branches (e.g., AtliQ Exotica vs. AtliQ Bay) to see which physical locations are dragging down the chain's overall reputation.

### Core Insights Extracted:
* **The Bangalore Deficit:** The analysis revealed that **Bangalore has the lowest average customer rating (3.41)**, indicating significant service or property-level issues in this region.
* **The Delhi Benchmark:** Conversely, **Delhi leads with the highest average rating (3.78)**, serving as the operational gold standard for the rest of the chain. 
* **The Feedback Loop:** Properties with lower ratings closely align with properties experiencing lower occupancy and realization rates, proving that service quality directly dictates long-term revenue health.

---

## 🎯 Final Recommendations & Strategic Actions
Based on the end-to-end exploratory data analysis, the following data-driven strategies are recommended for the Atliq Grands management team:

1. **Address the Bangalore Crisis:** Immediate operational audits are required for the Bangalore properties. The low average rating (3.41) suggests maintenance, staffing, or service quality issues that are directly causing market share loss.
2. **Revamp Cancellation Policies:** With a staggering ~25% cancellation rate causing massive revenue leakage, Atliq must implement stricter cancellation penalties (e.g., non-refundable tier discounts) or strategically overbook by 5-10% to offset predictable no-shows.
3. **Optimize Third-Party Channel Strategy:** Re-evaluate partnerships with booking platforms that bring in high volumes of empty, cancelled bookings. Shift marketing budgets to incentivize bookings through Atliq's "Direct Online" channels.
4. **Dynamic Pricing for Short Lead Times:** Since a significant portion of bookings happens close to the check-in date, implement dynamic, demand-based pricing models that automatically adjust rates as available inventory shrinks.
5. **Weekend vs. Weekday Campaigns:** Launch targeted marketing campaigns—such as extended stay discounts for business travelers or luxury weekend packages—to stabilize the Length of Stay (LOS) and maintain baseline occupancy targets.


## 🛠️ Tech Stack & Libraries
* **Language:** Python 3
* **Libraries:** `pandas`, `numpy`, `matplotlib`, `seaborn`
* **Environment:** Jupyter Notebook / Google Colab

## 🚀 How to Run
1. Clone this repository to your local machine.
2. Ensure you have the required libraries installed (`pip install pandas numpy matplotlib seaborn`).
3. Update the `drive_path` or local path to point to the datasets.
4. Run the Jupyter Notebook `EDA on Atliq Grands.ipynb` sequentially to reproduce the analysis and visualizations.

---
*Developed by [Your Name/GitHub Handle]. Specialized in Data Science and the Hospitality Sector.*
