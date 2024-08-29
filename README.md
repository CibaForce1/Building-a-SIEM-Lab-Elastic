# Building a SIEM Lab — Elastic

## Objective

The objective of this project was to create a Security Information and Event Management (SIEM) lab using Elastic, configure an Elastic Agent on a Kali Linux virtual machine to collect and monitor security events, create new alert rules, as well as utilize pre-existing alert rules to understand and respond to potential security incidents.

### Skills Learned

- Setting up and configuring SIEM environments
- Installing and managing Elastic Agent on Linux systems
- Generating and analyzing security events
- Creating visualization dashboards for event analysis
- Configuring and testing custom and pre-existing alert rules
- Incident response and alert management

### Tools Used

- Elastic SIEM: Used for setting up the SIEM environment and monitoring security events.
- VMware: Utilized for creating and managing the virtual environment.
- Kali Linux: The operating system on the virtual machine for generating security events.
- Nmap: Used to generate network scan events.
- John the Ripper: Employed to trigger alerts by simulating password attack tools.
- Hydra: Also used to trigger alerts by simulating password attack tools.

## Steps
Part 1: Setup and Configuration

Here, we will create an Elastic account, configure an Elastic Agent via a Kali Linux VM to collect logs, generate security events on the Kali VM, query these security events in Elastic, create a dashboard for visualization, and set up an alert.

Part 2: Utilizing Pre-existing Alert Rules

In the second part, we will utilize pre-existing alert rules on Elastic. We will assign the alerts to the created Agent and work on resolving the alerts.

PS. both VMware & Kali Linux VM were installed and used in previous projects; 

i. Incident response

ii. Vulnerability assessment

**Part 1: Setup and Configuration**

Step 1: Creating an Elastic Account

- To achieve this, we simply sign up on the Elastic Cloud registration website for a free/trial account using the link below:

https://cloud.elastic.co/registration
- Upon completing the sign-up, we choose “Create deployment,” select “Elasticsearch” as the deployment type, and then select the region and deployment size.


Step 2: Configure Elastic Agent via Kali Linux VM

Here, we create an agent to generate telemetry from our endpoint (Kali Linux VM) to Elastic SIEM. The following steps are employed to achieve this:
- On Elastic, we navigate to the integration page by clicking on the “Add Integrations” icon located at the top right of our screen.

![image](https://github.com/user-attachments/assets/6eca7a14-4365-40c2-9abc-540626496d06)


- From the integrations page, we select the Elastic Defend option and click on “Add Elastic Defend.”
  
![image](https://github.com/user-attachments/assets/75598d66-dd42-43f4-9d84-839e2bfbed7b)
![image](https://github.com/user-attachments/assets/11c2a506-7344-40c8-b167-46e7bc2b9507)


- We follow the prompts to “Install Elastic Agent.”
  
![image](https://github.com/user-attachments/assets/35ca550e-dc37-4008-8e9f-26dea0256a9b)


- We copy the provided Linux command and paste it on our host (Kali Linux).

curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.14.3-linux-x86_64.tar.gz
tar xzvf elastic-agent-8.14.3-linux-x86_64.tar.gz
cd elastic-agent-8.14.3-linux-x86_64
sudo ./elastic-agent install --url=https://597ab94823b54b83b4f04d0e9942bf36.fleet.us-central1.gcp.cloud.es.io:443 --enrollment-token=b1hQUUxwRUJHOGRTako2a2NYUVM6R0U3elpuWmdSNEdqenhEWmg5d3ZHZw==

![image](https://github.com/user-attachments/assets/0532cc46-4775-4f02-8466-3449711b3fec)
![image](https://github.com/user-attachments/assets/b3d68ecf-9562-4a2e-af72-6d4c13f48ec0)


- Once the agent installation is complete, we receive the message “Elastic Agent has been successfully installed.”

![image](https://github.com/user-attachments/assets/d1ba7403-b081-4c15-9a36-fa45da75b7f6)


To confirm the agent is running as expected, we run the command below,

sudo systemctl status elastic-agent.service
![image](https://github.com/user-attachments/assets/0bb04a20-9768-4822-9a10-1ba22c0703e0)

At this point, on Elastic, we can see that the agent has been enrolled successfully.
![image](https://github.com/user-attachments/assets/fb30431b-47f8-4729-a9b7-84c513b5f8b3)


Step 3: Generating Security Events on Kali Linux VM

Having fully set up our SIEM and enrolled an agent to Elastic, we will now generate some events to review on Elastic and confirm our agent is working as expected.

To achieve this, we will run the following Nmap commands:

#To scan for open ports

sudo nmap -sS localhost

sudo nmap -sT localhost

sudo nmap -p- localhost

![image](https://github.com/user-attachments/assets/d1959d91-0978-4a32-b4e7-501b0c4aaf93)

Step 4: Viewing Security Events in Elastic

To view the security events generated on Elastic from the commands we’ve run, we follow these steps:

- On Elastic we select “Logs” from the hamburger menu
  
- Click on “Stream” and from the search using the query,
process.args: “nmap”

- We then open the search result and view the security events generated.

![image](https://github.com/user-attachments/assets/116bb9c4-e4c3-4c0b-a7ee-57f1629a149d)
![image](https://github.com/user-attachments/assets/f59b78c3-829f-4c52-8ad5-95f27efb35e1)
![image](https://github.com/user-attachments/assets/165986ed-1778-447c-bbfc-23efcee47926)
![image](https://github.com/user-attachments/assets/747e14dc-e00d-4289-827a-ac74cf9857b6)




Step 5: Create a Dashboard for Visualization

Visualization dashboards provide better analysis of event logs and suspicious patterns identified in our SIEM, which helps in making informed decisions.

To create a dashboard, we follow these steps:

- Under the analytics menu, we select “Dashboards” and then “Create dashboard.”

![image](https://github.com/user-attachments/assets/b4d86d20-7a04-4d3d-90a8-0e6a77625da9)
![image](https://github.com/user-attachments/assets/ef576bea-4388-48f6-bdb5-13f6469ea712)


- We follow the prompts to “Create visualization.”

![image](https://github.com/user-attachments/assets/590d011d-9366-43f0-8207-5c960f7abd4a)

- We customize the visualization pattern and save it.

![image](https://github.com/user-attachments/assets/78725e50-c67a-4f87-9764-7e9801f14ee8)
![image](https://github.com/user-attachments/assets/aef62bf8-1b6e-4441-bfb7-987467849ed2)


Step 6: Setting up an Alert
Setting up an alert is crucial for detecting suspicious events, as it provides timely notifications based on predefined rules or custom queries, giving us ample time to prevent potential security incidents.

To set up an alert, we follow these steps:

- We navigate back to the Elastic home screen and select “Security.”

![image](https://github.com/user-attachments/assets/a5e67320-3da1-4176-93f9-a3784b298832)


- Click on the “Rules” option on the left side of the screen.

![image](https://github.com/user-attachments/assets/601cb5c0-79f2-44f2-9a34-f41276b0a7de)

- We select the “Detection rules (SIEM)” option.

![image](https://github.com/user-attachments/assets/a7ac4a13-9b9b-4bb0-ba8c-c82ddcc0b945)

- Click on the “Create new rule” icon.

![image](https://github.com/user-attachments/assets/e7a5fdd2-5f5c-49cf-99e8-e688319a49e5)


- We choose “Custom query” as the Define rule option, and set the conditions for the rule.

![image](https://github.com/user-attachments/assets/1d68a79e-789c-46b7-9d06-1794647536ac)

Here we are able to set the conditions for the rule, query below is used to detect Nmap scan events.

![image](https://github.com/user-attachments/assets/8d3ff7f9-ae4c-4e5f-b985-ceb937e71ded)

Under the about rule section, we set the rule name, description, and severity levels to help prioritize alerts.

![image](https://github.com/user-attachments/assets/c1efb445-526a-4ce5-b07c-82cc31a665a9)

- We configure the alert channel via the “Rule actions” section. For this project, we will configure the email option.

![image](https://github.com/user-attachments/assets/8889e2be-0c9f-480a-9e42-c49d4e7af645)

By selecting the email option, we are able to set the email connector, the frequency of the alert, the email address for alerts, the subject, and the body of the alert email itself.

![image](https://github.com/user-attachments/assets/483dd539-9da4-46ee-bd5d-7e87726275f8)
![image](https://github.com/user-attachments/assets/7e50a58b-b3e5-49cd-ad35-dd9f83ab8970)



After enabling the rule, we carried out a test to verify the rule is working as intended and that an alert will be generated if all conditions for the rule are met.

To achieve this, we re-run the same Nmap commands as initiated at the beginning of this project. From the images below, we can see that the alerts were generated and a notification was sent to the designated email address.

![image](https://github.com/user-attachments/assets/b5ab390e-e8dc-487c-908d-07c9d30a493a)
![image](https://github.com/user-attachments/assets/2d6e51ca-251f-4410-9ed9-0c09dfbc67ef)


**Part 2: Utilizing Pre-existing Alert Rules**

In the first part, we tested and configured a new rule. In this section, we will trigger events with a pre-installed custom rule and attempt to resolve them on Elastic.

To achieve this, the following steps were carried out:

Step 1: Identify the specific custom rule to trigger.

As there are over 1000 custom rules embedded into Elastic, a quick search for the term “hack” was initiated, revealing the rule “Potential Linux Hack Tool Launched.”

![image](https://github.com/user-attachments/assets/295b07dc-caa6-488e-8ea8-c0a07738824a)
![image](https://github.com/user-attachments/assets/b6c630a4-7a32-4044-8367-e9ce8bc205f6)


When we expand the rule, we find more information on the rule as well as the specific tools that trigger the alert.

![image](https://github.com/user-attachments/assets/6e58fce1-db6c-4cd0-8170-45729d49610d)

Step 2: Based on the defined rule, simply opening the specified hacking tools will trigger the alert.

As captured below, John the Ripper and Hydra (both hacking tools used in password attacks) were launched on different occasions in our Kali Linux VM (host machine).

![image](https://github.com/user-attachments/assets/820e893b-5e47-475b-ac48-bc4102569356)

When we return to the Alerts tab on Elastic, we see that 5 alerts have been generated based on our actions.

![image](https://github.com/user-attachments/assets/cb777b4f-29d4-4e60-9737-60a81e598ca9)

Step 3: Under the “Actions” section, we can “view details” on the alert, “investigate in timeline” of the event, “analyse event,” “open session view,” and resolve the alert.

![image](https://github.com/user-attachments/assets/ab20084e-8140-4c16-bc3a-7f615779a17c)

When we expand the alert, we can assign the alert to an agent as in the real world. We can also see the tools that were launched, the file location, and the status of the affected endpoint.

![image](https://github.com/user-attachments/assets/f6ca9975-cff2-423e-8f5b-d20d0888dda9)
![image](https://github.com/user-attachments/assets/1dc4cac2-4ec0-49b4-8013-aab43c3b271d)


Considering the event was a test and no further compromising actions were carried out, the alert will be “Marked as closed” since, in this case, it is a false positive.

![image](https://github.com/user-attachments/assets/eaba70b0-2669-457b-aaa2-d498fc92941d)


## Conclusion

In this project, we successfully set up a Security Information and Event Management (SIEM) lab using Elastic. We configured an Elastic Agent on a Kali Linux virtual machine to generate and monitor security events. Through this process, we created visualization dashboards for better analysis and set up custom alerts to detect suspicious activities in real-time. Additionally, we utilized and tested pre-existing alert rules to simulate real-world scenarios and understand the response mechanisms available within Elastic. This comprehensive exercise has provided valuable insights into the capabilities of SIEM solutions and their critical role in maintaining robust cybersecurity defences.
