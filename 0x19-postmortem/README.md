# Postmortem project
## 0x19-postmortem

## Issue Summary
Duration: The outage lasted for 2 hours, from 08:00 AM to 10:00 AM UTC on August 15, 2024.
Impact: The primary web application was unavailable during this time, affecting 70% of users globally. Users experienced "500 Internal Server Error" messages when attempting to access the service.
Root Cause: A misconfiguration in the database connection pool led to the exhaustion of available connections, causing the application to fail to retrieve data from the database.

## Timeline

    08:00 AM - Issue detected via automated monitoring alert indicating a sharp increase in "500 Internal Server Error" responses.
    08:05 AM - Engineering team was alerted and began investigating the issue. Initial focus was on the web server configuration.
    08:15 AM - Misleading assumption: Web server resources were suspected to be exhausted due to increased traffic.
    08:25 AM - Web server logs reviewed; no significant issues found.
    08:40 AM - Issue escalated to the Database Administration (DBA) team.
    08:50 AM - DBA team identified that the database connection pool was exhausted.
    09:00 AM - Misleading path: The connection pool was increased, but the issue persisted.
    09:15 AM - Further investigation revealed a misconfiguration in the connection pool settings.
    09:30 AM - Configuration corrected, and the database service was restarted.
    09:45 AM - Monitoring confirmed that the application started recovering.
    10:00 AM - Full service restored.

## Root Cause and Resolution

The root cause of the outage was a misconfiguration in the database connection pool settings. Specifically, the maximum number of allowed connections was set too low to handle the incoming traffic, leading to all available connections being used up. Once the pool was exhausted, the application could no longer establish new connections, resulting in widespread "500 Internal Server Error" responses.

To resolve the issue, the configuration settings for the database connection pool were adjusted to allow for more connections. The database service was then restarted to apply the new configuration. After the restart, the application regained the ability to establish connections to the database, and service was gradually restored.

## Corrective and Preventative Measures

### Improvements Needed:

    Database Connection Monitoring: Implement more granular monitoring on database connection pool usage to detect exhaustion before it leads to an outage.
    Configuration Review: Regularly review and test configuration settings for critical components like the database connection pool.
    Traffic Handling: Improve the application’s ability to handle traffic spikes by optimizing connection management.