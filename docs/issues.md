# Issues
other Docs
<!-- * [New Setup](/newSetup.md)
* [Essentials](/essentials.md)
* [Documenting](/documenting.md) -->


- [Issues](#issues)
  - [Checking an Employee](#checking-an-employee)


## Checking an Employee
 1. Find the ClickUp task is related to the issue.[here](/essentials.md#clickup-for-different-regions).
 2. There you can find the installation report in the activity section. Get the RMS link
   ![create](/images/clickup.activity.report.png)
 3. You will be directed to Teltonica RMS. Login if needed. [[reffer this.](/essentials.md#rms-login)]
   ![RMS home](/images/teltonica.rms.png)
 4. Connect to the **RPI**,  With provided username and password.  
   ```text
     Username:  team.mw@i360.lk 
     Password: MW@i360.lk 
   ```
 5. After successfully login to the RPI, Use `scan` to run **nmap** scan on network. which shows the connected IPs, see [Default IP Distribution](/essentials.md#default-ip-distribution) for more details.
 6. Use this command to get data related to a employee.
   ```bash
    touch ./output.log && wget -q -O./output.log --http-user=admin --http-passwd=Hik12345 http://192.168.1.165/ISAPI/AccessControl/UserInfo/Search?format=json --post-data '{"UserInfoSearchCond":{"maxResults": 10,"searchID":"1","searchResultPosition":0,"EmployeeNoList":[{"employeeNo":"<employeeId>"}]}}' && cat ./output.log && echo '' >> ./output.log
   ```
   <span style={{ color: "red" }}>Important:</span>
 * Replace the **\<employeeId>** with the correct id. ([Getting employeeId](/essentials.md#getting-employee-id))
 * Check if the correct **Device IP** used in the address.
 6. Output
    * If employee not synced to the device.
        ![Employee not synced](/images/employee.notsynced.png)
        Fix: Restart the Docker container to re-sync all profiles to the device.([see more.](/essentials.md#restart-docker))
    *
    <!-- Check for more outputs -->