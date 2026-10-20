UEBA: Logon Anomaly Detection

Overview

This project explores User and Entity Behaviour Analytics (UEBA) using logon activity from the CERT Insider Threat Dataset. It creates a behavioural profile for each user and applies an unsupervised anomaly-detection algorithm to identify profiles that differ from the wider group.

The notebook also reviews flagged users by role and uses Principal Component Analysis (PCA) to visualise patterns in the selected features. Although device and file-activity data are loaded and explored, they are not combined with the anomaly-detection model in this version of the project.

Objectives

The project aims to:

- Explore logon, device, file-activity, and user data.
- Create time-based and user-level logon features.
- Use Isolation Forest to identify unusual user profiles.
- Review flagged profiles in the context of employee roles.
- Use PCA to visualise variation in the engineered features.
- Consider the limitations of unsupervised anomaly detection for insider-threat monitoring.

Data and Features

The notebook uses four CSV files:

- logon.csv records logon and logoff activity, timestamps, users, and PCs.
- device.csv records device connection activity.
- file.csv records file activity, including removable-media indicators.
- users.csv contains employee role and organisational information.

The anomaly-detection model uses four features aggregated by user. logon_activity_events counts records in the logon dataset, including both Logon and Logoff events. weekend_ratio measures the proportion of a user's recorded events that occurred on weekends. after_hours_ratio measures the proportion occurring before 06:00 or from 21:00 onward. unique_pc_count counts the distinct PCs associated with the user's recorded activity.

The user profiles are calculated across the available observation period. This supports retrospective analysis, but does not by itself simulate real-time detection using only information available up to a particular moment.

Methodology

The notebook first explores the available datasets and checks their structure and missing values. It then derives hour-of-day and day-of-week features from logon timestamps and aggregates the records into user-level profiles.

The four features are standardised before an Isolation Forest model is fitted. The model uses 200 trees, a contamination setting of 0.01, and a fixed random seed of 42. The contamination setting instructs the model to identify approximately 1% of profiles as outliers; it is a model configuration, not an estimate of the actual prevalence of insider threats.

The notebook then inspects the lowest anomaly scores, compares anomaly rates across employee roles, and applies PCA to visualise the selected features in two dimensions.

Results and Interpretation

In the saved notebook results, flagged profiles tend to have high activity-event counts, higher proportions of after-hours activity, and a large number of distinct PCs. Many flagged users are IT Administrators, including users in Electronic Security teams.

This pattern may reflect the access requirements of those roles rather than malicious activity. The model identifies statistical outliers based on the selected features; it does not establish intent or confirm an insider threat.

The first two principal components explain approximately **93.75%** of the variance in the four standardised features. The first component is associated with activity-event count, after-hours activity, and unique PC count, while the second is dominated by weekend activity ratio. This helps visualise variation in the feature data, but explained variance is not a measure of anomaly-detection accuracy.

Limitations

The model uses logon-derived features only. Device and file-activity data are explored but are not integrated into the anomaly-detection model. The user profiles also aggregate activity across the available observation period, so the notebook does not evaluate a real-time detection process.

The analysis is unsupervised and does not validate the anomaly flags against known insider-threat cases. The contamination setting affects how many profiles are flagged, and unusual behaviour may be explained by legitimate job responsibilities. In addition, some fields in the device and user datasets contain missing values that would need to be addressed before those sources could be incorporated into a combined model.

Tools and Technologies

- Python
- pandas
- NumPy
- scikit-learn
- Jupyter Notebook
- Matplotlib

Future Work

Possible next steps include combining logon data with device, file-access, and email activity; creating time-windowed features to examine changes in behaviour; and comparing Isolation Forest with other anomaly-detection methods. If suitable labelled events are available, the anomaly rankings could also be evaluated against known insider-threat scenarios. Role-aware baselines could help provide context for different job responsibilities.

Conclusion

This project demonstrates a basic UEBA workflow using logon-derived features, Isolation Forest, role-based review, and PCA. It shows how unsupervised learning can identify unusual user profiles, while also illustrating why anomalies need to be investigated in context.

The findings are exploratory. The notebook does not confirm that flagged users are malicious, validate the flags against known insider-threat cases, or establish that the model is suitable for production use.

Disclaimer

This project is for educational and portfolio purposes only. It is not intended to determine employee intent or to support production security decisions without further validation and investigation.
