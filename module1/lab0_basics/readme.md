\# Module 1, Lab 0, Alex Petit

\## Deployment Evidence

| Deployment | Public IPv4 address | Connection method | Screenshot | Instance terminated? |

|---|---|---|---|---|

| 1 | `54.204.198.153` | EC2 Connect / SSH | `screenshots/deployment-1.png` | Yes|

| 2 | `54.92.154.187` | EC2 Connect | `screenshots/deployment-2.png` | Yes |

| 3 | `100.54.253.144` | EC2 Connect / SSH | `screenshots/deployment-3.png` | Yes |

\## Deployment Process

1. I launched an Amazon EC2 instance using the setup in the class repo, configured with Amazon Linux 2023 and the provided User Data script. I saved this as a template. After the instance reached the Running state and passed its status checks, I connected using EC2 Instance Connect through the AWS Management Console instead of an external SSH client. Once connected, I viewed the SEIR dashboard, which displayed information about CPU usage, memory, disk space, and system uptime. I documented the instance in a screenshot, then terminated the instance.
2. I launched an Amazon EC2 instance using the aforementioned saved launch template configured with Amazon Linux 2023 and the provided User Data script. After the instance reached the Running state and passed its status checks, I connected using EC2 Instance Connect through the AWS Management Console instead of an external SSH client. Once connected, I viewed the SEIR dashboard, which displayed information about CPU usage, memory, disk space, and system uptime. I documented the instance in a screenshot, then terminated the instance.
3. I launched an Amazon EC2 instance using the aforementioned saved launch template configured with Amazon Linux 2023 and the provided User Data script. After the instance reached the Running state and passed its status checks, I connected using EC2 Instance Connect through the AWS Management Console instead of an external SSH client. Once connected, I viewed the SEIR dashboard, which displayed information about CPU usage, memory, disk space, and system uptime. I documented the instance in a screenshot, then terminated the instance.

\## What CPU, memory, disk, and uptime tell me about the instance.

CPU usage shows how hard the EC2 instance's processor is working to handle tasks. Memory (RAM) shows how much temporary working space is being used to run programs and processes, which can affect performance if it gets too full. Disk usage shows how much storage space is being used and how much is still available for files, applications, and system data. Uptime tells us how long the instance has been running since it was last started or restarted. Together, these measurements help us understand the instance's overall health, performance, and whether it has enough resources to operate properly.

\## One thing that became easier after repeating the process.

Launching the instances was much easier from the template than from manually entering all the information. Also, asking ChatGPT for help is becoming more and more natural. 

\## Any errors encountered and how you resolved them.

Fucking retarded ass quota limit thing showed up and prevented me from launching multiple instances at once; I filed the request to increase the quota. 

\## Confirmation that you retained the launch template and cleaned up practice instances.

I did. 

