# Complete Operation Schema

**Note:** The prime symbol (') denotes the state of a variable after the operation has executed.

| id | operation | precondition | input | post condition |
| :--- | :--- | :--- | :--- | :--- |
| 1 | PerformSelfCheck | `system_power = ON` AND `system_mode = STARTUP` | *None* | IF `sensors_status = OK` -> `system_mode' = MONITORING`<br>IF `sensors_status = FAULT` -> `system_mode' = STARTUP` AND `error_flag' = TRUE` |
| 2 | LoadArtifactProfile | `system_mode = MONITORING` AND `artifact_present = TRUE` AND `profile_loaded = FALSE` | `artifact_id`, `temp_limit`, `humidity_limit` | `current_artifact_id' = artifact_id`<br>`req_temp' = temp_limit`<br>`req_humidity' = humidity_limit`<br>`profile_loaded' = TRUE` |
| 3 | DetectDoorClosure | `door_status = OPEN` | *None* | `door_status' = CLOSED`<br>IF `profile_loaded = TRUE` -> `system_mode' = CONSERVATION_ACTIVE` |
| 4 | DetectDoorOpen | `door_status = CLOSED` | *None* | `door_status' = OPEN`<br>IF `system_mode = CONSERVATION_ACTIVE` -> `system_mode' = MONITORING` |
| 5 | CorrectTemperature | `system_mode = CONSERVATION_ACTIVE` AND (`current_temp < req_temp_min` OR `current_temp > req_temp_max`) | *None* | `temp_control_active' = TRUE`<br>`recovery_timer' = STARTED` |
| 6 | CorrectHumidity | `system_mode = CONSERVATION_ACTIVE` AND (`current_humidity < req_humidity_min` OR `current_humidity > req_humidity_max`) | *None* | `humidity_control_active' = TRUE`<br>`recovery_timer' = STARTED` |
| 7 | VerifyCorrection | (`temp_control_active = TRUE` OR `humidity_control_active = TRUE`) AND `recovery_timer > 0` | *None* | IF conditions restored -> `controls_active' = FALSE` AND `recovery_timer' = STOPPED`<br>IF `recovery_timer = EXPIRED` -> `system_mode' = PROTECTION_MODE` |
| 8 | TriggerProtectionAlert | `system_mode = PROTECTION_MODE` AND `alert_active = FALSE` | *None* | `operator_alert' = GENERATED`<br>`alert_active' = TRUE` |
| 9 | ReduceLightExposure | `system_mode = PROTECTION_MODE` | *None* | `light_level' = MINIMUM` OR `light_level' = OFF` |
| 10 | SuspendForVibration | `artifact_present = TRUE` AND `current_vibration > vibration_threshold` | *None* | `system_mode' = VIBRATION_RESPONSE`<br>`environmental_controls' = SUSPENDED`<br>`stabilization_timer' = STARTED` |
| 11 | VerifyVibrationStabilization | `system_mode = VIBRATION_RESPONSE` | *None* | IF `current_vibration < vibration_threshold` until `stabilization_timer = EXPIRED` -> `system_mode' = CONSERVATION_ACTIVE`<br>ELSE -> `stabilization_timer' = RESET` |
| 12 | SwitchToEmergencyPower | `main_power = LOST` AND `emergency_power = AVAILABLE` | *None* | `current_power_source' = EMERGENCY`<br>`system_mode' = system_mode` |
| 13 | ExecuteSafeShutdown | `main_power = LOST` AND `emergency_power = UNAVAILABLE` | *None* | `incident_log' = UPDATED`<br>`system_mode' = SAFE_SHUTDOWN` |
| 14 | ReleaseArtifact | `door_status = OPEN` AND `system_mode = MONITORING` AND `alert_active = FALSE` | *None* | `artifact_present' = FALSE`<br>`profile_loaded' = FALSE` |
