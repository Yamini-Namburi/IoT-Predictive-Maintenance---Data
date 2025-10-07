Coceptual Data Model :

At a high level, we capture where, what, when, and why of sensor activity:

Factory → Area → Machine → Sensor
(Hierarchical structure of physical equipment)

Sensor Reading records measurements from each sensor every few seconds.

Maintenance Event records scheduled or unscheduled maintenance.

Out-of-Tolerance Alert records instances where readings exceed predefined safe ranges.


Logical Data Model

We use a multi-layered architecture:

Raw Layer - Raw IoT readings (every few seconds, huge volume).

Aggregated Layer - Aggregated readings (e.g., hourly) for analytics.

Analytical Layer - Predictive maintenance models, dashboards.

Fact Tables
a) fact_sensor_reading

Fact Type: Periodic Snapshot (aggregated)

Grain: One row per machine × sensor × hour

Why hourly?

Hourly aggregation balances data volume and trend visibility.

Minute-level data = petabytes; hourly data = manageable with sufficient granularity for predictive trends.

Measures:

avg_temperature, max_temperature, min_temperature

avg_vibration, max_vibration, min_vibration

avg_pressure, avg_rotation_speed

out_of_tolerance_count

b) fact_maintenance_event

Grain: One row per maintenance activity (machine × timestamp)

Measures: event_duration_hours, parts_replaced_count

c) fact_out_of_tolerance_alert

Factless fact table: logs when a sensor exceeds tolerance.

Grain: One row per (machine × sensor × timestamp)

Attributes: alert_type (TEMP_HIGH, VIBRATION_HIGH, etc.)

Dimension Tables


dim_machine: machine_sk, machine_id, machine_type, installation_date, normal range params 

Historical metadata of machines

dim_sensor:sensor_sk, sensor_id, sensor_type, unit, tolerance ranges

Sensor catalog

dim_date:date_sk, full_date, year, month, day

Calendar dimension

dim_time_of_day: time_sk, hour, minute, part_of_day

Time of day for slicing

dim_location: location_sk, factory_id, area_id, factory_name, area_name, country

Hierarchical factory mapping


Why This Logical Design Works?

Uses three fact tables to separate:

Aggregated IoT readings

Maintenance activities

Anomalies/alerts

Dimensions are shared (conformed) → allows joining facts consistently by machine, sensor, location, and time.

Enables analysis like:

“Average temperature vs. maintenance frequency by factory area.”

“Machines with highest out-of-tolerance counts in last 7 days.”

“Vibration trends in the 24 hours before a failure event.”

