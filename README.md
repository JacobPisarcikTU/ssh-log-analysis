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

I started by uploading the `ssh_logs.json` file into Splunk. To do this, I opened Splunk, went to Settings, selected Add Data, and chose the SSH log file for upload.

### 2. Set the Source Type and Index

![Screenshot 2](screenshots/screenshot2.png)

After selecting the file, I followed the upload prompts and confirmed the source type and index being used for the data. In this case, the source type was `_json` and the index was `main`.

### 3. Search the Logs

![Screenshot 3](screenshots/screenshot3.png)

To verify that the data was indexed correctly, I performed a basic search in Splunk using the following query:

    index=main sourcetype="_json"

What this query does:  
This query searches the `main` index for events labeled with the `_json` sourcetype. It is a simple way to confirm that the uploaded SSH log file was indexed properly and that the events are searchable.

What I found:  
The search returned the uploaded SSH log events, which confirmed that the file was successfully added to Splunk and that the data was ready for analysis.

### 4. Review Failed Login Attempts

![Screenshot 4](screenshots/screenshot4.png)

The first analysis task was to identify the top 10 endpoints with failed SSH login attempts. I used the following query:

    index=main sourcetype="_json" auth_success=false
    | stats count by "id.orig_h"
    | sort -count
    | head 10

What this query does:  
This query filters the dataset to failed SSH authentication attempts, counts how many failed logins came from each source IP address, sorts the results from highest to lowest, and shows the top 10 endpoints.

What I found:  
The results showed that several IP addresses generated repeated failed SSH login attempts. The highest count came from `10.0.0.25`, followed by `10.0.0.46` and `10.0.0.48`. This kind of pattern can help identify repeated unauthorized access attempts or possible brute force behavior.

### 5. Count Total SSH Connections

![Screenshot 5](screenshots/screenshot5.png)

The next task was to determine the total number of SSH connections in the dataset. I used the following query:

    index=main sourcetype="_json"
    | stats count as total_ssh_connections

What this query does:  
This query counts all SSH log events in the dataset and returns the total as `total_ssh_connections`. It provides a quick baseline view of how much SSH activity is present in the uploaded log file.

What I found:  
The results showed a total of `1200` SSH connections in the dataset. This gave me a better sense of the overall volume of SSH activity before looking more closely at specific event types.

### 6. Review Event Types

![Screenshot 6](screenshots/screenshot6.png)

For the final task, I counted all event types present in the logs using the following query:

    index=main sourcetype="_json"
    | stats count by event_type

What this query does:  
This query groups the events by `event_type` and counts how many times each category appears. It helps break down the overall dataset into major SSH activity types.

What I found:  
The results showed four categories of SSH activity:
- Successful SSH Login: `306`
- Failed SSH Login: `305`
- Multiple Failed Authentication Attempts: `303`
- Connection Without Authentication: `286`

## Conclusion

This project helped me get more comfortable with using Splunk to upload, search, and analyze log data. By working through the SSH log file, I was able to verify the uploaded data, identify failed login activity, count total SSH connections, and review the different event types present in the dataset.

One of the main hiccups during this project was troubleshooting the correct source type and query format after uploading the file. At first, getting the search results to appear took some trial and error, but once I confirmed the correct sourcetype and index values, the searches returned the expected results. Overall, this lab was a good introduction to using Splunk for log analysis and basic security-focused investigation.
