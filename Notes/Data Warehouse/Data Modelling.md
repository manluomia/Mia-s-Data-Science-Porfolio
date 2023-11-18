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

