**Prerequisites - Setup The Environment**

Decieding on what enivorment for this technical test was an interesting one as I had not heard much of Vagrant and did want to explore this route but ulitmately after some time with this I decideded to go with Virtual box and run Ubuntu as there was some familiarity from my days through college and througout my own projects.

I obtained the image "Ubuntu 21.04" straight from Ubuntu.com. The reason for not using the image from Virtual box was so I could create an image of my own specifications and total ownership of the image.

Once the image was installed and configured the first command that needed to be run was "sudo apt-get update". This command is used to download package information from all configured sources. When this was completed it was time to setup DataDog account and install the agent which is detialed in the next section.

**Collecting Metrics**

_Installing DataDog Agent_
Once the Ubuntu machine was operational I logged onto the datadog site and installed the agent through the first inital account setup phase.

A big help was to go onto DataDog site and use the tips and notes illustrated:
https://docs.datadoghq.com/agent/basic_agent_usage/ubuntu/?tab=agentv6v7

Below is a photo of the Host that was created along with the tags that I had established for this task:
![image](https://user-images.githubusercontent.com/90416145/133288893-bf3652f8-7f41-4793-937b-76acdf12ae11.png)

To do this I opened up the terminal in my Ubuntu VM and entered the command "sudo vi /etc/datadog-agent/datadog.yaml"
Sudo vi - enters into another terminal to edit the datadog.yaml file from the path /etc/datadog-agent/.
Once opened, I changed the hostname and inserted some tags into the datadog.yaml file by removing the comment # and inserting the new tags.

![image](https://user-images.githubusercontent.com/90416145/133290532-5beffd84-8545-4636-b2d9-cbeaaee7c366.png)

Used the command :wq to save changes and leave.

_Installing  PostGresDB_
I decieded on adding Postgres DB to as it is perhaps not as well known as MySQL and the fact that it is an open source tool has me interested to use as I am a fan of open sourced tools/applications as I think many people can learn and get into without having time restraints  

To install run the command "sudo apt-get install postgressql"
To check it is installed and operational run the command service postgresql status where active entry should be active link in the screenshot below:
![image](https://user-images.githubusercontent.com/90416145/133295300-a2625237-9bdb-45d5-89b3-a5c7e51daec9.png)

To intergrate DataDog into PostGres I followed the steps detailed on the datadog site link below:
https://www.datadoghq.com/blog/collect-postgresql-data-with-datadog/

When the datadog user was added and granted, I confirmed this by typing \du within the postgres command line tool by entering the command "sudo su postgres"
![image](https://user-images.githubusercontent.com/90416145/133294926-2fba51bc-2f39-4567-8a75-4da0d8839431.png)

Subsequently I logged onto Datadog gui and installed the PostGres intergration
![image](https://user-images.githubusercontent.com/90416145/133296005-ee828608-4724-40f1-bdf7-0163cab1f339.png)

_Custom Checks_

Tasks to create a custom metric in which I named my mertic as custom_SimonCheck.yaml / py

Followed the example on DataDog site link below:
https://docs.datadoghq.com/developers/write_agent_check/?tab=agentv6v7

Changed directory by using the command cd /etc/datadog-agent/checks.d

vim custom_Simon_Check.py
![image](https://user-images.githubusercontent.com/90416145/133300229-e52eae46-7e50-4f27-9061-a7e0bf25bb3e.png)

checked to confirm file was present
![image](https://user-images.githubusercontent.com/90416145/133300546-350e2964-656e-4939-bb26-182a8b353518.png)

Changed directory to conf.d to create the .yaml file
sudo vim custom_Simon_Check.yaml to edit the yaml file to add instances

In the image below I have the instance that can now use upto 45 seconds in the custom_Simon_Check.yaml
![image](https://user-images.githubusercontent.com/90416145/133298909-3c557e41-c88d-4019-9f59-27b817e886c3.png)

Confirmed custom_SimonCheck/yaml was present:
![image](https://user-images.githubusercontent.com/90416145/133300765-e6cfa3d7-fc12-48da-95c8-47e4877e3650.png)

To verify check run commandsudo -u dd-agent -- datadog-agent check custom_SimonCheck
![image](https://user-images.githubusercontent.com/90416145/133303280-3bd71bad-bbd8-4c53-aa3e-241e6748d886.png)


_Bonus Question_
The  min_collection_interval is set to a number i.e 45, this means that it could be collected every 45seconds not that it will collect every 45seconds. The collector will try to run the checker every 45 seconds but it maybe late as another task/check is being performed prior to this custom check. If the check takes longer than the allotted time then the Agent will skip the execution until the next interval


