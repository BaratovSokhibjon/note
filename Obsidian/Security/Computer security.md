
### Thread types 
- hijacking via rogue wireless hotspots commonly referred to as "evil twin hotspots"
- sending fake "authentic" files to install the **ransomware** in host machine to gain access
- stuxnet, a sophisticated nation-state-sponsored malware, exploited USB drives to infiltrate Windows systems.
	- stuxnet is a prime example of a network attack motivated by national security concerns.

### PII, PHI, and PSI
- Personally identifiable information (PII) is any information that can be used to positively identify an individual.
	- Name
	- Social security number
	- Birthdate
	- Credit card numbers
	- Bank account numbers
	- Government issued ID
	- Address information (street, email, phone numbers)
- A subset of PII is protected health information (PHI). The medical community creates and maintains electronic medical records (EMRs) that contain PHI.
- Personal security information (PSI) is another type of PII.
	- usernames
	- password
	- other credentials
> **Most hacks on companies and organizations that have been reported in the news involved stolen PII or PHI.**


> [!NOTE] Lost competitive advantage
> The more intellectual property gets revealed the more the value of the company decreases which makes the security essential. The other aspect is that the users of the company will lose their PII which will cause the users to lose their trust to the company.
> 


### Security Operations Center (SOC):

1. **SIEM (Security Information and Event Management)** - Aggregates and analyzes log data.
2. **IDS/IPS (Intrusion Detection/Prevention Systems)** - Monitors and mitigates network threats.
3. **Threat Intelligence** - Data on cyber threats and vulnerabilities.
4. **Analysts** - Cybersecurity experts (e.g., Tier 1, Tier 2, Incident Responders).
5. **Firewalls** - Network traffic filters.
6. **Endpoint Detection and Response (EDR)** - Monitors device-level threats.
7. **Incident Response Protocols** - Structured mitigation processes.
8. **Dashboards** - Real-time threat visualization.
9. **Automation Tools** - Orchestrates repetitive tasks (e.g., SOAR - Security Orchestration, Automation, and Response).

#### Traditional SOC job tiers:
- **Tier 1**: **Alert analyst**: monitor incoming alerts and pass the ticket to next tier in case of serious incident
	- Monitors incidents
	- Opens ticket
	- Basic threat mitigation
- **Tier 2: Incident reporter:** investigate deeply about the incident and come up with possible remedies
	- Deep Investigation
	- Advises remediation
- **Tier 3: Thread hunter**: they are very expert in the security in each possible field and they are responsible for finding vulnerabilities in the current system
	- In-depth knowledge
	- Threat hunting
	- Preventive measures
- **SOC manager:** resource manager and a point of contact for larger organizational events

![[Pasted image 20250404135537.png]]

#### Technologies in SOC
##### SIEM
> As shown in the figure, a SOC needs a security information and event management system (SIEM), or its equivalent. SIEM makes sense of all the data that firewalls, network appliances, intrusion detection systems, and other devices generate.

![[Pasted image 20250404140311.png]]

#### SOAR
> SOAR platforms are similar to SIEMs in that they aggregate, correlate, and analyze alerts. However, SOAR technology goes a step further by integrating threat intelligence and automating incident investigation and response workflows based on playbooks developed by the security team.

![[Pasted image 20250404140829.png]]


### SOC Metrics or KPI (Key performance indicators)
- **Dwell Time** - the length of time that threat actors have access to a network before they are detected, and their access is stopped.
- **Mean Time to Detect (MTTD)** - the average time that it takes for the SOC personnel to identify valid security incidents have occurred in the network.
- **Mean Time to Respond (MTTR)** - the average time that it takes to stop and remediate a security incident.
- **Mean Time to Contain (MTTC)** - the time required to stop the incident from causing further damage to systems or data.
- **Time to Control** - the time required to stop the spread of malware in the network.

### Availability & Security
Enterprise networks prioritize uptime, balancing downtime costs against redundancy expenses. Tolerance varies by industry—e.g., small retail may accept a single router failure, while e-commerce demands redundancy. Uptime is measured in annual downtime minutes: "five nines" (99.999%) allows 5 minutes, "four nines" (99.99%) permits 53 minutes.