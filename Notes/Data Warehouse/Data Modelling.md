# Data Modelling

## Data Is Secondary
- Derived from other systems.
- Comes later in business evolution.
- Often an afterthought.
- Result: Data is often a mess.

## Goals
- Prepare data for analytics.
- Synthesize higher-level properties.
- Merge data from different sources into logical entities.
- Ready for analysis/KPIs/BI.
- Answer questions in various analytic categories.

## Concepts of Dimensional Modeling
- Entities: Unchanging "things" with primary keys.
- Events: Changes happening to entities over time.
- Facts: Measurements of cumulative effects of events at a point in time.
- Dimensions: Descriptive properties of an entity true over time.
- Grain: Temporal resolution of pre-aggregated facts.
- Model: A set of facts or dimensions with a common schema.
- Mart: Namespace for a related collection of models.

## Examples
- Facts: Order, Stock trade, Sensor reading.
- Dimensions: Product size & color, Q4 Earnings, Sensor location.

## NYC Transit Example
- Entities: Bikes, bike stations, bike riders, FHV bases, Taxi company locations, Weather stations.
- Events: Rides, trips, weather.
- Facts: Duration, distance, start/end time, tolls, passenger count, wheelchair flag.
- Dimensions: Temperature, precipitation, base name, taxi company, pick-up location, wheelchair accessible vehicle flag.

## Metrics & KPIs
- Metrics: Numeric aggregations made with data.
- Simple Metric Example: Revenue = sum of order values.
- Complex Metric Example: Customer lifetime value.
- KPIs: Top-level metrics aligning with business goals.

## Facts and Dimensions
- Fact tables: Data points about entities with timestamps.
- Primary keys sometimes present, linked to dimension tables by foreign key.
- Largest tables in a data model.

## Surrogate Keys
- Used when fact tables don’t have a primary key.
- Combination of fields like foreign keys + timestamp for uniqueness.

## Synthesized Dimensions
- Calculated from events about other entities.
- Stored when rarely changing, involving complex calculations, or expensive historical data retrieval.

## Changing Dimensions
- Fixed dimensions like city coordinates vs changing dimensions like a person’s address.
- Three main approaches: SCD Type I, SCD Type II, Snapshots.

## SCD Types
- Type I: Ignores changes over time.
- Type II: Records dimension states over time; most common and optimal but expensive.

## Snapshots
- Regular snapshots of the whole dimension table.
- Efficient but lose details between snapshot points.

## Conforming Dimensions
- Ensure combined dimension columns from different sources have the same meaning.

## Modelling Strategies
- Snowflake: Traditional relational model with 3NF.
- One Big Table (OBT): Fully denormalized wide fact table, easier to query but inefficient on storage.

## Choosing an Approach
- Depends on resources, use case scope, and requirements.
- OBT for simple, narrow-scope cases.
- Snowflake with SCDs for well-resourced, wide-scope cases.
- Star schema: Outdated but still in use.

## Additional Notes
- Hypercubes/OLAP cubes irrelevant for MPP DWH.
- Semantic layers: Store metric and dimension definitions once, offering a single source of truth.

## DBT Operations
- `dbt seed`: Load small in-repo data files.
- Create fact and dimension tables in DBT models.
- Run DBT builds and compile analyses for execution.


# Designing DBT Models from Scratch: A Framework by Taylor Brownlow

## Step 1: Discover
- **Objective**: Understand the business process being modeled.
- **Players**: Data modeler, business stakeholders.
- **Activities**:
  - Map the business process.
  - Identify stakeholders' needs for the final table (metrics, filters, etc.).
  - Understand current solutions and their limitations.
  - Identify secondary stakeholders.
  - Gather relevant business context.

## Step 2: Design (& Iterate!)
- **Objective**: Develop potential model structures.
- **Players**: Data modeler.
- **Activities**:
  - Map out the final table structure (granularity, columns).
  - Explore multiple design options.
  - Seek feedback from stakeholders.

## Step 3: Prototype (& Iterate!)
- **Objective**: Develop a working model structure.
- **Players**: Data modeler, data team members, stakeholders.
- **Activities**:
  - Outline each step of the model with code and results.
  - Peer review by data team members.
  - Validate logic with stakeholders.
  - Ensure understanding and approval from stakeholders and data team.

## Step 4: Deploy
- **Objective**: Implement the model in DBT.
- **Players**: Data modeler.
- **Activities**:
  - Integrate the final prototype code into the DBT codebase.
  - Conduct tests to ensure model accuracy.

## Step 5: Deliver
- **Objective**: Make the model accessible to stakeholders.
- **Players**: Data modeler, business stakeholders.
- **Activities**:
  - Document the model (in DBT or elsewhere).
  - Share links to the model and documentation.
  - Optionally, create sample analyses or introductory sessions.

## Conclusion and Next Steps
- The process is designed to be collaborative and iterative, involving stakeholders at each stage.
- Emphasis on understanding business needs, validating designs, and ensuring stakeholder buy-in.
- Aims to decrease deployment time and improve model uptake.
- Encourages feedback and continuous improvement in data modeling.

# How to Data Model Correctly: Kimball vs One Big Table

## Kimball Data Modeling
- **Overview**: Long-standing gold standard in data engineering.
- **Principles**:
  - Data should be as normalized as possible.
  - Minimize duplication of columns.
  - Maximize integrity constraints (foreign keys, primary keys).
  - Split data into facts and dimensions tables.
  - Facts are high-volume, event-focused (the 'what').
  - Dimensions are lower-volume, entity-focused (the 'who').
- **Strengths**:
  - Powerful for answering complicated questions about relationships.
  - Enforced integrity guarantees data quality.
- **Shortcomings in Data Lake Environments**:
  - Loses primary and foreign key notions.
  - Joins can be inefficient in distributed computing environments.
  - Shuffle process can be a curse, especially without broadcast join options.
  - Can lead to disorganized data and increased cloud costs.

## One Big Table Model
- **Concept**:
  - Sounds unconventional but offers efficiency gains.
  - Dimensions are modeled cumulatively and deduplicated.
  - Fact data added to dimensions as `ARRAY<STRUCT>` data types.
  - Utilizes array functions like REDUCE and TRANSFORM for efficient analysis.
- **Advantages**:
  - Faster query execution.
  - Deeper analysis capabilities.
  - Maintains data sort order, crucial in data lake environments.
- **Case Study**:
  - Used at Airbnb for pricing and availability.
  - Led to significant efficiency improvements.

## Choosing Between Kimball and One Big Table
- **Factors to Consider**:
  - **Data Volume**:
    - Small volume: Kimball is preferable.
    - Large volume: One Big Table can reduce shuffle and increase efficiency.
  - **Technical Skills of Downstream Users**:
    - Basic SQL users: Kimball is more user-friendly.
    - Willingness to learn new techniques: One Big Table can be advantageous.
  - **Data Practices at the Company**:
    - Strong data quality standards: Either model can work.
    - Lacking built-in quality constraints: One Big Table requires proactive quality management.

## Conclusion
- Data modeling is an evolving field.
- Choice depends on specific needs and constraints.
- Important to consider data volume, user skills, and company practices.

