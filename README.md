Innovate MFG IoT Predictive Maintenance

Business Scenario

Innovate MFG is a leader in industrial manufacturing. They have instrumented their factory floor machinery with thousands of IoT sensors that monitor critical operational parameters like temperature, pressure, vibration, and rotation speed. These sensors stream terabytes of data daily.

The primary business objective is to implement a predictive maintenance program. By analyzing historical sensor data leading up to machine failures, the company wants to build models that can predict future failures. This will allow them to schedule maintenance proactively, reducing costly unplanned downtime and improving safety.

Key analytical questions include:

What were the average sensor readings (temp, vibration) in the 24 hours preceding a known failure for a specific machine type?

Are there correlations between sensor reading spikes and maintenance events?

How many times has a machine operated outside its normal tolerance range in the last month?

Identify all machines that have shown anomalous vibration patterns for more than an hour today.

Technical Context

Data Source: An IoT data stream pushing sensor readings every few seconds. Each reading includes a sensor_id, timestamp, machine_id, and a set of key-value readings ({'temperature': 95.4, 'vibration': 0.78}). Maintenance logs are stored in a separate system.

Data Warehouse: AWS Redshift.

Challenge: The sheer volume and velocity of the data make a traditional transactional fact table impractical.
Your Task
Design a robust and highly performant data model in AWS Redshift capable of handling massive-scale time-series data for predictive analytics. Your design must balance storage costs, data ingestion performance, and query latency.
Expected Outcomes

1. Conceptual Data Model:

Create a high-level diagram showing the relationships between Factories, Factory Areas, Machines, Sensors, Sensor Readings, and Maintenance Events.
2. Logical Data Model (Dimensional Model):

Propose a multi-layered data architecture. You should design tables for both raw data and aggregated data.
Fact Tables:
fact_sensor_reading_aggregated: This will be your primary table for analysis. It should not store every raw reading. Define the grain of this table. Will you aggregate by minute, 15 minutes, or hour? Justify your choice based on the business requirements. What are the measures (e.g., avg_temperature, max_vibration, min_pressure)?

fact_maintenance_event: A table to log maintenance activities, including scheduled and unscheduled (failure) events.

fact_out_of_tolerance_alert: A Factless Fact Table to record every instance a machine's sensor reading goes outside its predefined acceptable tolerance. This table captures the co-occurrence of a machine, sensor, and timestamp for an event.

Dimension Tables:

dim_machine: Includes machine type, installation date, operational parameters (e.g., normal temperature range), and location.

dim_sensor: Includes sensor type and specifications.

dim_date

dim_location (Factory, Area).

Diagram your proposed schema.


4. Physical Data Model:
Write the complete Redshift DDL SQL scripts for all proposed fact and dimension tables.

This is the most critical section. Your physical design choices must reflect the massive scale of the data.

Redshift-specific optimizations:

Distribution Style (DISTSTYLE): Justify your choice for the aggregated fact table. Should it be distributed by machine_id or sensor_id? What are the trade-offs?

Sort Keys (SORTKEY): This is crucial for time-series data. Propose a compound sort key for the aggregated fact table and explain how it will dramatically improve performance for date-range queries.

Data Ingestion Strategy: Briefly describe a strategy for populating the fact_sensor_reading_aggregated table. Would you load raw data into a staging area first and then run aggregation queries? Explain the benefits of this approach.

Data Retention: Propose a data lifecycle policy. For example, raw data might be kept for 7 days, while aggregated data is kept for 5 years. How would you implement this in an AWS environment?
