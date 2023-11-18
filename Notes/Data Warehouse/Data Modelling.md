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
