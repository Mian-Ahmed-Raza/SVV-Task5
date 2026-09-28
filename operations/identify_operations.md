# Identify Operations

| opid | operation | purpose |
| :--- | :--- | :--- |
| 1 | PerformSelfCheck | Validates the status of sensors and environmental-control devices upon startup. |
| 2 | LoadArtifactProfile | Records the artifact's ID and its specific environmental limits. |
| 3 | DetectDoorClosure | Registers that the chamber door has been closed. |
| 4 | DetectDoorOpen | Suspends active conservation if the door is opened. |
| 5 | CorrectTemperature | Activates environmental controls when the temperature strays outside the permitted profile. |
| 6 | CorrectHumidity | Activates environmental controls when the humidity strays outside the permitted profile. |
| 7 | VerifyCorrection | Polls sensors to confirm that the environment has successfully returned to the permitted range. |
| 8 | TriggerProtectionAlert | Alerts the operator when environmental conditions cannot be corrected within the recovery period. |
| 9 | ReduceLightExposure | Dims or turns off lighting to protect the artifact during an emergency response. |
| 10 | SuspendForVibration | Halts normal environmental controls when significant vibration is detected. |
| 11 | VerifyVibrationStabilization | Confirms that vibration levels have remained below the threshold for the required stabilization period. |
| 12 | SwitchToEmergencyPower | Transfers the system to the backup power source when main power is lost. |
| 13 | ExecuteSafeShutdown | Logs a failure incident and safely powers down the system when no power is available. |
| 14 | ReleaseArtifact | Clears the system state for the operator to safely remove the artifact. |
