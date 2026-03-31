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

![Screenshot 1](screenshots/screenshot1.png)

After opening Splunk, I uploaded a .json file of SSH logs to Splunk. I did this by clicking settings, and then choosing 'add data'.  

### 2. Set the Source Type and Index

![Screenshot 2](screenshots/screenshot2.png)

After uploading ssh_logs.json, I followed the prompts and set the source type to '_json' and the index to 'main'. 

### 3. Search the Logs

![Screenshot 3](screenshots/screenshot3.png)

To verify that the data was indexed correctly, I performed a quick search in Splunk using the following query:

    index=main sourcetype="_json"

### 4. Review Failed Login Attempts

![Screenshot 4](screenshots/screenshot4.png)

The first task I wanted to complete was to list the top 10 endpoints with failed login attempts.
To do this, I used the query below:

    index=main sourcetype="_json" auth_success=false
    | stats count by "id.orig_h"
    | sort -count
    | head 10

### 5. Count Total SSH Connections

![Screenshot 5](screenshots/screenshot5.png)

The following query would show me the total number of SSH connections in the dataset.  

    index=main sourcetype="json"
    | stats count as total_ssh_connections

### 6. Review Event Types

![Screenshot 6](screenshots/screenshot6.png)

I used the following query to count all event types seen in the logs. This includes successful, failed, no-auth, and multiple-failed attempts. 

    index=main sourcetype="json"
    | stats count by event_type

### Conclusion
(Add a conclusion and what hiccups I may have ran into during this project)
