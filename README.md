# Entra-ID-Protection-Deep-Dive
Introduction
This technical guide covers a deep Identity Protection content, if you would like to demo to your customer ID Protection Dashboard, detections, Risky Users...etc, use Alpine Ski House environment. Instructions how to get access could be found at Security Demos.

Overview
Entra ID Protection is a service that helps you protect your organization from identity-based threats. It uses machine learning and heuristics to detect anomalies and risky sign-in behavior, and it alerts you to potential breaches or compromised accounts. It also allows you to take actions to remediate the risks and secure your identities, 
for example feeding the signals into tools like Conditional Access to make access decisions or fed back to a security information and event management (SIEM) tool for further
investigation and correlation.
<img width="940" height="468" alt="image" src="https://github.com/user-attachments/assets/410e4c33-32e9-408a-9242-e11106efc697" />
To use all Entra ID Identity Protection features, you need to have Microsoft Entra ID P2 licenses , however some limited features are available also with Entra ID Free and Entra ID P1 licenses. For more information please check: Entra ID Protection License requirements

Workload Identities Premium licences are required for full risk detection of workload identity risks, they are required also to protect workload identities with conditional access policies.

Detect
Risk detection, risky sign-in, risky user and risky workload identities
Start the discussion by explaining the concept of risk detection, risky sign-in, risky user and risky workload identities, spend a moment to explain these concepts and the relationship between them.

Risk detection is a powerful resource that can include any suspicious or anomalous activity related to a user in the directory. (there are detections for workload identity as well). Each risk detected is reported as risk detection.

Risky sign-in: Sign-in risk detections represent the probability that a given authentication request isn't done by the legitimate owner of the account, A risky sign-in is reported when there are one or more risk detections reported for that sign-in.

Risky user: The probability that given user is compromised.

A Risky user is reported when either or both of the following are true:

The user has one or more Risky sign-ins. (real-time)
One or more risk detections are reported. (offline)
Risky workload identities: the probability that given application or service principal is compromised.

Use the two images below (slides) to explain the hierarchy of risk in ID Protection.
<img width="940" height="628" alt="image" src="https://github.com/user-attachments/assets/959a3397-241f-437a-bc68-6891938d1097" />
Important
It's important to understand the hierarchy of risk in Identity Protection.
When we think about what makes a user risky, there is this pyramid of risk. At the bottom, we have risk detections. These are the specific anomalies that we detect that indicate that the user might be compromised. For example, we might detect atypical travel patterns, unfamiliar sign-in properties, or leaked credentials. Most of our types of risk detections can be traced to a particular sign-in, during which we observed the suspicious behavior. These create "risky sign-ins". For a sign-in to be risky, it means that it has one or more underlying risk detections. Both risky sign-ins and risk detections contribute to the overall user risk- which represents the overall likelihood that particular identity is compromised. At each of these layers, we have a corresponding report in our portal experience as well as an API.
Explain to the customer the difference between Real-time and offline detection.

ID Protection utilizes techniques to increase the precision of user and sign-in risk detections by calculating some risks in real-time or offline after authentication. Detecting risk in real-time at sign-in gives the advantage of identifying risk early so that customers can quickly investigate the potential compromise. On detections that calculate risk offline, they can provide more insight as to how the threat actor gained access to the account and the impact on the legitimate user. Some detections can be triggered both offline and during sign-in, which increases confidence in being precise on the compromise.

Detections triggered in real-time take 5-10 minutes to surface details in the reports. Offline detections take up to 48 hours to surface in the reports, as it takes time to evaluate properties of the potential risk.

Explain to the customer that ID Protection leverage detection signals from other products like defender for cloud app and defender for endpoint for detection and risk levels.

Examples:

Possible attempt to access Primary Refresh Token (PRT) detection by MDE.

Impossible travel detection by defender for cloud app.

Mass Access to Sensitive Files detection by defender for cloud app.

ID Protection dashboard
Please mention the new Identity Protection dashboard, a centralized view of the identity risk level in your organization, with insights into the sources and trends of the risks.

The core components of this dashboard are:

Metric cards: providing four key metrics to help you understand the effectiveness of the security measures you have in place:
Number of attacks blocked
Number of users protected
Mean time your users take to self-remediate their risks
Number of new high-risk users detected
Attack Graphic: it displays common identity-based attack patterns, represented by MITRE ATT&CK techniques, detected for your tenant. For more information, see the section Risk detection type to MITRE attack type mapping.
MAP: displaying the country location of the risky sign-ins in your tenant
Recommendations: based on the attacks detected in your tenant over the past 30 days, you'll see:
Up to three recommendations if specific attacks occur in their tenant.
Insight into the impact of the attack.
Direct links to take appropriate actions for remediation.
Recent Activities: summary of recent risk-related activities in your tenant, like: Attack Activity, Admin Remediation Activity
Investigate
Identity Protection provides organizations with reporting they can use to investigate identity risks in their environment. These reports include

Risky users.
Risky workload Identities.
Risky sign-in.
Risk detections.
Investigation of events is key to better understanding and identifying any weak points in your security strategy. All of these reports allow for downloading of events in .CSV format or integration with other security solutions like a dedicated SIEM tool for further analysis.

Organizations can take advantage of the Microsoft Graph API integrations to aggregate data with other sources they might have access to as an organization. The Graph API provides access to identity protection entities and operations:

riskyUsers
riskDetection
signIn
ServicePrincipal Risk detection
Risky ServicePrincipal
You can also use the Microsoft Graph PowerShell SDK and Identity Protection APIs to manage risky users using PowerShell.

Organizations who have Microsoft 365 Defender and Microsoft Defender for Identity gain extra value from Identity Protection signals. This value comes in the form of enhanced correlation with other data from other parts of the organization and extra automated investigation and response.

Please follow the article How to investigate risky users? for some tips on investigation.

Respond and remediate
Microsoft Entra Conditional Access offers three risk conditions powered by Microsoft Entra ID Protection signals: Sign-in risk, User risk and service principal risk. Organizations can create risk-based Conditional Access policies by configuring these three risk conditions and choosing an access control method. During each sign-in, ID Protection sends the detected risk levels to Conditional Access, and the risk-based policies apply if the policy conditions are satisfied.

Risk-based Conditional Access policies can be enabled to require access controls such as providing a strong authentication method, perform multifactor authentication, require sign-in, perform a secure password reset based on the detected risk level.
<img width="935" height="463" alt="image" src="https://github.com/user-attachments/assets/f2f960d9-a7f3-4f48-b7fa-6fcc7b87bf5b" />


It’s not recommended to configure risk policies also from Identity Protection blade, it’s highly recommended to use risk based Conditional Access policy as this will offer more granularity and security.

As a starting point we recommend:

User risk policy: Require a secure password change when user risk level is High
Sign-in risk policy: Require Microsoft Entra multifactor authentication when sign-in risk level is Medium or High
Remediation
Risk-based Conditional Access policies can be enabled to require access controls such as providing a strong authentication method, perform multifactor authentication, or perform a secure password reset based on the detected risk level. If the user successfully completes the access control, the risk is automatically remediated.

administrator can manually review risks the portal, through the API, or in Microsoft 365 Defender. Administrators can perform manual actions to dismiss, confirm safe, or confirm compromise on the risks.

Deploy ID Protection
Microsoft Entra multifactor authentication registration policy
Ensuring that users can do strong authentication is the first step when thinking about enforcing risk-based CAP, for users to self-remediate risk though, they must register for Microsoft Entra multifactor authentication before they become risky.

Please explain to the customer the importance of securing security info registration by requiring a location, risk level, strong authentication...etc. below an article on securing security info registration with TAP.

https://learn.microsoft.com/en-us/entra/identity/conditional-access/howto-conditional-access-policy-registration

ID Protection helps you manage the roll-out of Microsoft Entra multifactor authentication registration : you can require MFA registration to your users, no matter what modern authentication app they are signing in to.

This policy helps to ensure that users have a strong method of authentication, and reduces the friction of enabling MFA for the first time. It also plays a key role in preparing your organization to self-remediate from risk detections in Identity Protection.

Using identity protection multifactor authentication registration policy can help to improve the security of your organization, by reducing the risk of compromised credentials and unauthorized access. It can also help to comply with regulatory requirements and industry standards that mandate MFA for certain scenarios or data types. Additionally, it can enhance the user experience, by allowing users to choose their preferred method of MFA, and avoid interruptions during sign-in or self-service password reset.

Please note the User experiences with MFA registration policy: it will prompt your users to register the next time they sign in interactively and they'll have 14 days to complete registration. During this 14-day period, they can bypass registration if MFA isn't required as a condition, but at the end of the period they'll be required to register before they can complete the sign-in process.

Known network location
I It's important to configure named locations in Conditional Access and add your VPN ranges to Defender for Cloud Apps. Sign-ins from named locations marked as trusted or known, improve the accuracy of Microsoft Entra ID Protection risk calculations. These sign-ins lower a user's risk when they authenticate from a location marked as trusted or known. This practice reduces false positives for some detections in your environment.

Explain to the customer that this will reduce false positives.

ID Protection settings
Explain to the customer the option Allow on-premises password change to reset user risk

There might be some customers in a true password-less environment where users don’t know the password and where most of user risks are not triggered by leaked credential detection, it makes sense is this situation to disable the option.

Explain to the option Report suspicious activity, receiving a Push notification in Microsoft authenticator without initiating the authentication indicates a high risk of compromise (Password compromised, token theft ...etc), it’s important to educate end users to report suspicious activity.

Report suspicious activity is available with MS Authenticator and voice calls and it raises immediately user’s risk level to high.

Finally, remind the customer the importance of configuring notifications in ID Protection.

Microsoft Entra ID Protection sends two types of automated notification emails to help you manage user risk and risk detections:

Users at risk detected email
Weekly digest email
<img width="796" height="595" alt="image" src="https://github.com/user-attachments/assets/e0bc084b-43cb-47a5-a09d-8f7816d4f19d" />

Impact analysis of risk-based access policies
Customers are often concerned about the implementation of risk-based Conditional Access policies because of the impact these might have in their productivity.

Such concerns are clearly often overstated and do not, however, preclude the need to make the customer understand the added value that using Identity Protection features would bring at the security level in a Zero Trust approach.

The Identity Protection risk analysis workbook helps administrators understand what would happen if you create and enable Microsoft Entra ID Protection risk based Conditional Access policies in your environment. It provides a detailed breakdown of sign-ins within a selected date range, including:

An impact summary of recommended risk-based access policies, covering user risk scenarios and sign-in risk/trusted network scenarios.
Impact details for unique users, including high-risk users not blocked or prompted to change their password by a risk-based access policy, users who changed their password due to a policy, risky users not successfully signing in due to a policy, and users who remediated risk through on-premises or cloud-based password resets.
Sign-in risk policy scenarios, such as high-risk sign-ins not blocked or self-remediated using multifactor authentication by a risk-based access policy, risky sign-ins unsuccessful due to a policy, and risky sign-ins remediated by multifactor authentication.
Network details, including top IP addresses not listed as a trusted network.
Administrators can use this information to anticipate which users might be affected over time if risk-based Conditional Access policies are enabled.
<img width="796" height="573" alt="image" src="https://github.com/user-attachments/assets/60b413ae-87bc-45e9-bf69-544d8f9bd1a3" />


Important
The only prerequisite is to have configured Entra ID log forwarding to a Log Analytics Workspace.
Please note that there are some usefull KQL queries that we encourage you to utilize, the queries are available here:
https://github.com/chadmcox/Azure_Active_Directory/tree/master/Log%20Analytics

Identity Protection and B2B users
Identity Protection also supports B2B users, or external users who have been invited to collaborate with your organization through Entra ID B2B.

IMPORTANT

The user risk for B2B collaboration users is evaluated at their home directory. The real-time sign-in risk for these users is evaluated at the resource directory when they try to access the resource.

However, there are some limitations to consider when enabling Identity Protection for B2B users.

B2B users cannot perform self-service password reset in your tenant. They need to reset their password in their home tenant
Guest users do not appear in the risky users report. This limitation is due to the risk evaluation occurring in the B2B user's home directory.
Administrators cannot dismiss or remediate a risky B2B collaboration user in their resource directory. This limitation is due to administrators in the resource directory not having access to the B2B user's home directory.
If you want to exclude B2B users from user risk policy or sign-in risk policy, you can create a group that contains only B2B users, and exclude that group from the policy assignment.

By following these guidelines, you can provide a secure and consistent experience for your B2B users, and protect your resources from potential threats.

Important notes
Risk basd conditional access policies help ID protection learning
Some customers are afraid from activating risk based conditional access policies because of the number of risk detections on the reports, sometimes they claim that there are false positives, explain to the customer that risk based conditional access help ID protection to learn and reduce false positives, for example when a user is challenged for strong authentication because of Atypical travel detection, if the user is able to satisfy this challenge and lower the risk then ID Protection will learn from this situation and it will consider this for next authentication attempts.

True password-less customers and automatic user risk remediation
There are some organizations with true password-less environment where User risk remediation triggered by SSPR is not an option for them, we have in the roadmap remediate user risk by requiring sign-in.

When users authenticate with Password-less like WHFB, FIDO or CBA, they got the MFA claim in the token, Require MFA as a response for a risky sign-in in this situation isn’t efficient, it’s better to respond by Requiring an interactive sign with Sign-In Frequency – Every time, Complaint device...etc.

CAE and ID Protection
Without creating any CAP, critical event evaluation is applied by default for Exchange Online, SharePoint Online and Teams.

If one of the events below happens, the access token will be rejected by the resource provider:

User Account is deleted or disabled.
Password for a user is changed or reset.
Multifactor authentication is enabled for the user.
Administrator explicitly revokes all refresh tokens for a user.
High user risk detected by Microsoft Entra ID Protection.



