# CiscoFDM-unsupportedConfig_workAround
- Some configs from legacy cisco ASA, when migrated to Cisco Firepower Managed via Firepower Device Manager are not supported.
- Solution
  1. configure it manually using **LinaConfigTool**
  2. add crontab task to re-run the tool
     - This is needed because once an admin make a change via GUI and deploy the changes, the configuration done by LinaConfigTool is not saved.

# In this example "pat-pool" is not supported in FDM by the time of this writing
•	Login to cli and enter expert mode
•	Enter sudo mode
•	Run lina config tool
  - /ngfw/usr/local/sf/bin/LinaConfigTool "nat (inside,outside) 3 source dynamic any pat-pool <network-object-pool>"
  - ![image](https://github.com/romarroca/CiscoFDM-unsupportedConfig_workAround/assets/87074019/666eab6c-8da9-40b3-b29a-0fa4b18bac64)
  - Verify using system support diagnostic-cli
  - ![image](https://github.com/romarroca/CiscoFDM-unsupportedConfig_workAround/assets/87074019/265ce77e-eeb4-46ff-9f0b-4a6f887cf215)
  - Unfortunately, if the user change and deploys a config, the added nat statements will be removed as it is not part of the Web interface deploy.
  - ![image](https://github.com/romarroca/CiscoFDM-unsupportedConfig_workAround/assets/87074019/7138cc54-923e-4e4f-ae91-0bbf3e641e71)
  - As a workaround, admin can add the one-liner after deploying any changes to make sure that the pat-pool is added. 
As this is just a temporary solution while we are waiting for the Firepower Management Center which fully supports pat-pool, admin can create cronjob which executes the one-liner every 1minute (can be changed)
  - In the linux shell edit crontab and add the LinaConfigTool one liner
  - ![image](https://github.com/romarroca/CiscoFDM-unsupportedConfig_workAround/assets/87074019/359bcf12-1ee3-48b0-b91d-4ad737adcde7)

#Disclaimer
This workaround is intended for short-term use, particularly for urgent requirements that necessitate immediate configuration changes. For long-term stability and management, it's strongly recommended to connect your Firepower Sensor to a dedicated Firepower Management Center. We get it—using a single firewall might make you question this approach, but Cisco designed it this way for optimized performance and security.

