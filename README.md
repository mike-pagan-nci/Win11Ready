# Win11Ready

Original script by https://github.com/Tachaeon

Description
This script assesses a computer's Windows 11 readiness based on Microsoft's criteria

You can run the script two ways:
1. Run the script locally on each computer and a file will be created in a local directory you specify
2. Deploy a scheduled task via group policy that runs the script on remote computers and copies the results file to a central share

## Results
The script will create a result which is copied into a local directory and/or a network share on a server on the network. The resulting file name will be Capable_COMPUTERNAME.txt or NotCapable_COMPUTERNAME.txt

Example file of a computer that failed to to missing TPM chip, non-compatible CPU, and secure boot not being enabled.
![image](https://github.com/user-attachments/assets/7fbb445e-93ae-4ed3-be3b-bb68f0557c2b)

## Quick steps to create a GPO to create a scheduled task:
1. Copy the script to a share accessible by computers
2. Modify the script to add the location of the share
3. Create a new GPO
4. Navigate to Scheduled Tasks under Computer Configuration

    ![image](https://github.com/user-attachments/assets/35e2994e-046c-4c6c-a69c-95ffb7d2b447)

5. Right-click in the white space and choose New > Scheduled task (at least Windows 7)

    ![image](https://github.com/user-attachments/assets/7b23961f-d629-45b6-848f-1e45677de6ec)

6. Select Change User or Group
7. Type SYSTEM into the "Name" field and click Check Name, then OK

    ![image](https://github.com/user-attachments/assets/abf8d3cf-6efc-4a38-9da2-3a364ea71246)

8. Also select Run whether user is logged on or not and Run with Highest Privileges

    ![image](https://github.com/user-attachments/assets/631470e2-96c1-48dc-b5fa-d6c29f6de617)

9. Add a Trigger (schedule) on the Triggers tab
10. On the Actions tab, click New
11. Fill out script information:
    - Program/Script: %SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe
    - Add Arguments: -ExecutionPolicy Bypass -file "\\SERVER\SHARE\Win11Ready.ps1"
    - Start in: C:\
   
    ![image](https://github.com/user-attachments/assets/8699a161-610c-4bfc-851d-6efce7fe236c)

12. Configure the Conditions, Settings, and Common tabs to meet your needs
13. Click on OK
14. Assign the GPO to an Organizational Unit (OU) that contains computers
15. Monitor that the scheduled task deploys and test script
