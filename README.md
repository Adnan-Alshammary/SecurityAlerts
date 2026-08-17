
# Security Alerts App
<img width="948" height="439" alt="alerts_management" src="https://github.com/user-attachments/assets/45025442-ce2e-4dab-b8cb-f959ad547c14" />

<br>
<br>


Security Alerts App allows you to manage your alerts from any index ( or multiple indexes) based on SPL query. It could be used to manage alerts for different team within SOC (threat hunting , compromised assessments, or threat intelligence team, …). 

the App define two custom roles:
- secalerts_analyst
- secalerts_admin

"secalerts_analyst" role is the minimum role needed to manage the alerts (by selecting alerts and click "Edit" button):
- add comment
- update alert status or disposition
- assign alert(s) to specific user

<img width="935" height="459" alt="edit_alert_casemgmt" src="https://github.com/user-attachments/assets/50f8f2bf-22f9-4e32-9c68-462f109f21eb" />

<br>
<br>
<br>
<br>

"secalerts_admin" role  (+ regular Splunk admin role) can used to customize the case management ("settings" button) including: 
- change main query (indexes to fetch alerts)
- add or change custom (status, disposition, or severity)
- assign a color for specific values in (status, disposition, or severity)

<img width="557" height="458" alt="settings_1" src="https://github.com/user-attachments/assets/fd5052fc-0d44-48d5-a8b1-45bf866f14cc" />

<img width="576" height="461" alt="settings_2" src="https://github.com/user-attachments/assets/b60c7215-65e4-4ac9-bb4e-2e25fecf108f" />

<br>
<br>
<br>
<br>

Important Notes:
- changing query structure could break the the analyst comments. we only recommend to change or add index name in first line unless you understand what the query does exactly.
- the query relay on the field _raw and _time to generate alert ID, changing these field in the query (run time ) could result in losing old analyst comments and alerts status.
- if your index does not have "_raw" or "_time" fields you can generate them in the query using eval command "| eval _raw=field1+field2+......+fieldN" ( use "rename" commands if the _time fields exists under other name)

## installation:
- the App  requires new version of Splunk enterprise (10.4 or later)
- restart the Search Head is required after installing the App 
- the App support search head cluster since its maintain kvstore and lookup files to manage the alerts.



## SOAR playbooks:

two Splunk SOAR playbooks were developed to interact with the App for automation (or enrich alerts with external AI analysis):
1. **secalerts_get_alerts**

this playbook (input playbook) used to retrieving alerts within specific time range based on the dynamic query the admin configured. the playbook has two inputs ( earliest_time and latest_time). this playbook will fetch the alerts details with the last update (last status,comment, ..etc).

- **input:** earliest_time , latest_time
- **output:** alerts_list
<img width="300" height="266" alt="input_get_alerts" src="https://github.com/user-attachments/assets/9c4823df-b39e-436b-a92e-fcdce62c5f4a" />
<img width="342" height="464" alt="get_alerts" src="https://github.com/user-attachments/assets/71fd77f6-6326-4080-86dd-2288c4c106d0" />


<br>
<br>

  
2. **secalerts_edit_alert**

this playbook (input playbook) used to update specific alert status, disposition, assigned to, or add comment. the only required input field is alert_id which can be fetched by the previous playbook.

- **input:** alert_id (required), comment, disposition, status, assigned_to
- **output:** check_alert_id
  - if "alert_id" is valid the result will be (check_alert_id="valid alert ID")  and the alert will be updated.
  - if "alert_id" is invalid the result will be (check_alert_id="Error: invalid alert ID") and there will be no update to "Security Alerts App"
<img width="265" height="379" alt="input_to_edit_alerts" src="https://github.com/user-attachments/assets/6750d290-00cf-4c74-afee-b477d8e08ad2" />

<img width="435" height="475" alt="edit_alerts" src="https://github.com/user-attachments/assets/f6b72d71-9c48-47cd-aaac-e109096ad3ab" />





 

