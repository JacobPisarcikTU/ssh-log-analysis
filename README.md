# SSH Log Analysis in Splunk

## Objective

Use Splunk to ingest and analyze SSH logs, detect failed and successful SSH authentication attempts, and identify unusual SSH activity that may indicate brute force or unauthorized access.

### Skills Learned

- SIEM log ingestion and log analysis
- Basic Splunk searching and reporting
- Reviewing failed and successful SSH authentication attempts
- Identifying unusual SSH activity in log data
- Working with JSON-formatted Zeek-style SSH logs

### Tools Used

- Splunk
- SSH logs
- JSON log data
- Zeek-style SSH logs

## Steps

### 1. Upload the SSH Logs into Splunk

[Insert Screenshot 1 Here]

Screenshot 1: Uploading the SSH log file into Splunk through Settings > Add Data.

### 2. Set the Source Type and Index

[Insert Screenshot 2 Here]

Screenshot 2: Configuring the source type and index for the SSH log data.

### 3. Search the Logs

[Insert Screenshot 3 Here]

Screenshot 3: Searching the ingested SSH logs in Splunk to verify that the data was indexed correctly.

### 4. Review Failed Login Attempts

[Insert Screenshot 4 Here]

Screenshot 4: Running the query to identify the top endpoints with failed SSH login attempts.

    index=ssh_lab sourcetype="json" auth_success=false
    | stats count by "id.orig_h"
    | sort -count
    | head 10

### 5. Count Total SSH Connections

[Insert Screenshot 5 Here]

Screenshot 5: Running the query to count the total number of SSH connections in the dataset.

    index=ssh_lab sourcetype="json"
    | stats count as total_ssh_connections

### 6. Review Event Types

[Insert Screenshot 6 Here]

Screenshot 6: Running the query to count the SSH event types observed in the logs.

    index=ssh_lab sourcetype="json"
    | stats count by event_type
