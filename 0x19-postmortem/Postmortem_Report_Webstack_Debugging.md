Incident Report for 504 Error / Site Outage



Summary:

On September 11th, 2023, at midnight (EAT), the server experienced an access issue leading to a 504 error for users attempting to access a specific website. The server in question operates on a LAMP stack.

Timeline:

- 00:00 EAT: Users encounter a 500 error when trying to access the website.
- 00:05 EAT: Apache and MySQL are verified to be up and running.
- 00:10 EAT: Despite server and database functionality, the website fails to load correctly.
- 00:12 EAT: A quick restart of the Apache server results in a status of 200 and OK when curling the website.
- 00:18 EAT: Error logs are reviewed to identify the source of the issue.
- 00:25 EAT: Examination of /var/log reveals premature shutdown of the Apache server, with PHP error logs missing.
- 00:30 EAT: Investigation of php.ini settings reveals that error logging is turned off; it is subsequently enabled.
- 00:32 EAT: Apache server is restarted, and error logs are checked to monitor entries in the PHP error logs.
- 00:36 EAT: Examination of PHP error logs uncovers a mistyped file name causing incorrect loading and premature server closure.
- 00:38 EAT: File name is corrected, and Apache server is restarted.
- 00:40 EAT: Server is now functioning normally, and the website loads properly.

Root Cause and Resolution:

The issue stemmed from an incorrect file name reference in the wp-settings.php file. The error manifested during attempts to curl the server, resulting in a 500 error. Initial investigation revealed the absence of error logs for PHP, and examination of the default Apache error log provided limited information on the premature server closure. Upon discovering that PHP error logs were not being generated, the engineer reviewed the php.ini file and found error logging was disabled. Enabling error logging and restarting the Apache server revealed that a misspelled file extension (.phpp) in the wp-settings.php file was causing the access error. Recognizing the potential replication of this error in other servers, a swift resolution was implemented by deploying Puppet code to correct all misspelled file extensions. This ensured consistent, error-free operation across all servers.

Corrective and Preventive Measures:

1. Enable error logging on all servers and websites to facilitate prompt identification of issues.
2. Test servers and websites locally before deploying in a multi-server setup to address and rectify errors proactively, minimizing downtime in a live environment.

