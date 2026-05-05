---
title: "Optimize security operations through an AWS Security Hub POC"
url: "https://aws.amazon.com/blogs/security/how-to-develop-an-aws-security-hub-poc/"
date: "Mon, 27 Apr 2026 18:55:44 +0000"
author: "Kyle Shields"
feed_url: "https://aws.amazon.com/blogs/security/feed/"
---
<div class="Page-articleBody"> 
 <div class="RichTextArticleBody RichTextBody"> 
  <blockquote>
   <p><strong>April 27, 2026</strong>: This post was first published in September 2025 when the enhanced AWS Security Hub was in public preview. It has since been updated to reflect the general availability of Security Hub. This revision also provides a more detailed, step-by-step framework for planning your POC.</p>
  </blockquote> 
  <hr /> 
  <p><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/security-hub/" rel="noopener" target="_blank">AWS Security Hub</a></span> prioritizes your critical security issues and helps you respond at scale to protect your environment. The service sharpens findings through aggregation, correlation, and enrichment of AWS native security signals into actionable insights, enabling faster and more efficient response times. You can use these capabilities to gain visibility across your cloud environment through centralized management in a unified cloud security solution. Security Hub creates a cloud-native application protection platform (CNAPP) and through the AWS free trial, you can create a comprehensive proof of concept (POC) evaluation without significant upfront investment in time or resources.</p> 
  <p>In this blog post, we guide you through planning and implementation POC for Security Hub to assess the implementation, functionality, cost estimate, and value of Security Hub in your environment. We walk you through the following steps:</p> 
  <ol class="rte2-style-ol" id="rte-28cd00b5-9a45-11f0-b749-d986507364f3" start="1" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px; color: #333333; font-family: AmazonEmber, 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; font-style: normal; font-weight: 400; letter-spacing: normal; text-align: start; text-indent: 0px;" type="1"> 
   <li>Understand the value of Security Hub</li> 
   <li>Determine success criteria for the POC</li> 
   <li>Define Security Hub configuration</li> 
   <li>Prepare for deployment</li> 
   <li>Enable Security Hub</li> 
   <li>Validate deployment</li> 
  </ol> 
  <div class="RichTextHeading"> 
   <h2>Understand the value of Security Hub</h2> 
  </div> 
  <div class="wp-caption alignnone" id="attachment_39957" style="width: 1440px;">
   <img alt="Figure1: AWS Security Hub overview" class="size-full wp-image-39957" height="775" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/09/26/Image-1-POC.png" width="1430" />
   <p class="wp-caption-text" id="caption-attachment-39957">Figure 1: AWS Security Hub overview</p>
  </div> 
  <p>Figure 1 provides a visualization of how Security Hub unifies signals from multiple AWS security services, partner solutions capabilities. The signals, which are ingested by Security Hub from multiple AWS security services and curated partner solutions include:</p> 
  <ul class="rte2-style-ul" id="rte-4da7b4b0-9a46-11f0-b749-d986507364f3" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px; color: #333333; font-family: AmazonEmber, 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; font-style: normal; font-weight: 400; letter-spacing: normal; text-align: start; text-indent: 0px;"> 
   <li><b>Threats</b>: <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-active.html" rel="noopener" target="_blank">Amazon GuardDuty</a></span> findings</li> 
   <li><b>Vulnerabilities</b>: <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/inspector/latest/user/findings-types.html" rel="noopener" target="_blank">Amazon Inspector</a></span> vulnerability findings</li> 
   <li><b>Controls</b>: <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings.html" rel="noopener" target="_blank">AWS Security Hub CSPM</a></span> findings</li> 
   <li><b>Configurations</b>: Resource inventory</li> 
   <li><b>Network exposures</b>: <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/inspector/latest/user/findings-types.html" rel="noopener" target="_blank">Amazon Inspector</a></span> network reachability findings</li> 
   <li><b>Sensitive data</b>: <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/macie/latest/user/findings-types.html" rel="noopener" target="_blank">Amazon Macie</a></span> findings</li> 
   <li><b>Extended Plan: </b><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/aws/aws-security-hub-extended-o%EF%AC%80ers-full-stack-enterprise-security-with-curated-partner-solutions/" rel="noopener" target="_blank">Curated partner solutions</a></span></li> 
  </ul> 
  <p>At its core, Security Hub provides four key capabilities in one unified solution:</p> 
  <ol class="rte2-style-ol" id="rte-f46097b1-9a44-11f0-b749-d986507364f3" start="1" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px; color: #333333; font-family: AmazonEmber, 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; font-style: normal; font-weight: 400; letter-spacing: normal; text-align: start; text-indent: 0px;"> 
   <li><b>Unified security operations</b>: Security Hub delivers a unified security operations experience, bringing your security signals into a single consolidated view and avoiding the need to switch between multiple security tools. This provides comprehensive visibility across your entire estate, including AWS, multi-cloud, and on-premises, so your security teams can efficiently detect, prioritize, and respond to potential security risks.</li> 
   <li><b>Intelligent prioritization helps focus on what matters most</b>: Security Hub helps you identify and prioritize critical security risks that might be missed when viewing findings in isolation. Security findings are correlated by analyzing resource relationships and signals from AWS security services and capabilities.</li> 
   <li><b>Actionable insights guide security teams on next steps</b>:<b> </b>Gain actionable insights through advanced analytics to transform correlated findings into clear, prioritized insights that highlight the most critical security risks in your environment. You can quickly understand potential impacts, visualize relationships, and identify which security issues pose the greatest risk to critical resources.</li> 
   <li><b>Streamlined security response and automation capabilities</b>: Security Hub enhances your security operations by enabling streamlined response capabilities. It seamlessly integrates with your existing ticketing systems to help facilitate efficient incident management.</li> 
  </ol> 
  <p>With this integrated approach your security team can:</p> 
  <ul class="rte2-style-ul" id="rte-4da7b4b1-9a46-11f0-b749-d986507364f3" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px; color: #333333; font-family: AmazonEmber, 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; font-style: normal; font-weight: 400; letter-spacing: normal; text-align: start; text-indent: 0px;"> 
   <li>Investigate critical risks that need immediate attention from a single pane of glass</li> 
   <li>Monitor security trends and attack surface across your environments</li> 
   <li>Fix what really matters across the entire attack chain and path</li> 
   <li>Automate responses to streamline remediation</li> 
  </ul> 
  <p><b>Understand the Open Cybersecurity Schema Framework</b></p> 
  <p>Security Hub uses the <span class="LinkEnhancement"><a class="Link" href="https://schema.ocsf.io/" rel="noopener" target="_blank">Open Cybersecurity Schema Framework (OCSF)</a></span> to help standardize security data and analysis and enable better integration between security tools. This standardization helps simplify how security findings are structured and analyzed across your environment. This standardized data model enables seamless integration and data exchange across your security tooling, providing normalized and consistent data formats. When implementing your Security Hub POC, make sure that you’re familiar with the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/securityhub/latest/userguide/ocsf-aws-extension.html" rel="noopener" target="_blank">OCSF specifications Security Hub uses</a></span>.</p> 
  <p>Additionally, confirm that any analytics or security information and event management (SIEM) tools you plan to integrate with support the OCSF data format to maximize the value of the consolidated security insights provided by Security Hub.</p> 
  <div class="RichTextHeading"> 
   <h2>Determine success criteria</h2> 
  </div> 
  <p>Establishing success criteria helps benchmark the outcomes of the POC with the goals of the business. Some example criteria and key performance indicators (KPI) include:</p> 
  <ul class="rte2-style-ul" id="rte-4da7b4b3-9a46-11f0-b749-d986507364f3" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px; color: #333333; font-family: AmazonEmber, 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; font-style: normal; font-weight: 400; letter-spacing: normal; text-align: start; text-indent: 0px;"> 
   <li><b>Alert consolidation metrics</b>: Determine what resources you’re currently using to correlate security events and signals to understand their relationship. Review the process and note if it’s completed outside of AWS or through a SIEM. By setting a benchmark to reduce correlation overhead you can significantly improve efficiency and accelerate security investigations and posture improvement.</li> 
   <li><b>Response time improvements</b>: Reducing your time to detect, investigate, and resolve security events and improve security posture is essential to streamlined security operations. Security Hub provides visualizations for potential attack paths that adversaries could use to exploit resources and helps assess the potential blast radius. 
    <ul class="rte2-style-ul" id="rte-4da7b4b5-9a46-11f0-b749-d986507364f3" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px;"> 
     <li>Reduced mean time to detect (MTTD) security incidents.</li> 
     <li>Reduced mean time to response (MTTR) for critical findings.</li> 
     <li>Reduced time to identify potentially affected resources in blast radius.</li> 
     <li>Increased accuracy of attack path analysis.</li> 
     <li>Number of controls implemented based on attack path insights (post investigation).</li> 
    </ul> </li> 
   <li><b>Automation capabilities</b>: Having response playbooks as part of your incident management and response plan helps ensure comprehensive investigations lead to improved security posture. Review your automation capabilities to see if portions of or entire playbooks can be automated. 
    <ul class="rte2-style-ul" id="rte-4da7b4b6-9a46-11f0-b749-d986507364f3" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px;"> 
     <li>Potentially increased percentage of security findings automatically routed to the correct teams by using Jira Cloud, ServiceNow, or a third-party tool.</li> 
     <li>Reduced average time from detection to ticket creation.</li> 
    </ul> </li> 
   <li><b>Severity and risk classification: </b>Review your organization’s inventory of assets to determine if it’s complete and if you can use telemetry to determine the severity level and associated risks. 
    <ul class="rte2-style-ul" id="rte-4da7b4b7-9a46-11f0-b749-d986507364f3" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px;"> 
     <li>Reduced time to identify critical resources and service coverage gaps affected by new vulnerabilities, threats, and misconfigurations.</li> 
     <li>Faster and more accurate severity label and risk calculation of findings.</li> 
     <li>Reduced time to identify service gap coverage.</li> 
    </ul> </li> 
  </ul> 
  <p>After establishing your success criteria, it’s essential to evaluate organizational readiness and potential constraints that might impact your POC implementation. Begin by conducting a comprehensive assessment of your current environment: Determine if the foundational security services (GuardDuty, Amazon Inspector, Security Hub CSPM, and Macie) are enabled across your accounts, and identify your critical workloads and if there are any excessive attack surfaces.</p> 
  <p>Review your success criteria to make sure that your goals are realistic given your timeframe and potential constraints that are specific to your organization. For example:</p> 
  <ul class="rte2-style-ul" id="rte-40794e12-2c55-11f1-b334-efd1f6896f7c"> 
   <li>Do you have full control over the configuration of AWS services that are deployed in an organization?</li> 
   <li>Do you have resources that can dedicate time to implement and test?</li> 
   <li>Is this time convenient for relevant stakeholders to evaluate the service?</li> 
  </ul> 
  <div class="RichTextHeading"> 
   <h2>Maximize your POC value through service activation</h2> 
  </div> 
  <p>To get the most comprehensive evaluation of the capabilities of Security Hub, coordinate the activation of underlying security services to optimize the overlapping trial periods available at no additional cost. The following is a list of the underlying security services, and their free trial length:</p> 
  <ul class="rte2-style-ul" id="rte-4da7b4b8-9a46-11f0-b749-d986507364f3" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px; color: #333333; font-family: AmazonEmber, 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; font-style: normal; font-weight: 400; letter-spacing: normal; text-align: start; text-indent: 0px;"> 
   <li><b>Security Hub</b>: 30-day trial (essential plan capabilities)</li> 
   <li><b>GuardDuty</b>: 30-day trial (covers most protection plans except GuardDuty Malware Protection)</li> 
   <li><b>Security Hub CSPM</b>: 30-day trial</li> 
   <li><b>Macie</b>: 30-day trial</li> 
   <li><b>Amazon Inspector</b>: 15-day trial</li> 
  </ul> 
  <p>Consider enabling these services simultaneously so that you have at least two weeks of overlapping coverage to evaluate the full correlation and risk prioritization capabilities of Security Hub across each service. Optionally, if you want to conduct a POC with minimal configuration because of limitations, you can enable only Security Hub CSPM and Amazon Inspector during the initial POC phase to properly assess the results and data.</p> 
  <blockquote>
   <p><b>Note</b>: Document your activation dates and trial expiration dates carefully. Create calendar reminders for trial end dates and schedule your key POC evaluation milestones to occur while services are active. This will help make sure that you can thoroughly assess the unified security operations capabilities of Security Hub when services are running at full capacity.</p>
  </blockquote> 
  <p>If you already have one or more of these underlying services enabled, you can proceed to enable the new Security Hub. To fully use the new Security Hub capabilities, particularly the exposure findings feature, specific service dependencies must be met, both Security Hub CSPM and Amazon Inspector are essential because they provide the telemetry needed for the Security Hub correlation engine and exposure findings. The combination enables Security Hub to deliver comprehensive risk analysis and prioritization by correlating configuration risks with runtime vulnerabilities. If you have other security services already enabled (such as GuardDuty or Macie), you can maintain these existing services while enabling Security Hub, and it will automatically begin incorporating their findings into its consolidated view, enhancing your overall security posture visualization.</p> 
  <div class="RichTextHeading"> 
   <h2>Define your Security Hub configuration</h2> 
  </div> 
  <p>After your success criteria have been established, you’re ready to plan your configuration. Some important decisions include:</p> 
  <ul class="rte2-style-ul" id="rte-e26ac910-9a47-11f0-aa3d-fbe48dae450c" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px; color: #333333; font-family: AmazonEmber, 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; font-style: normal; font-weight: 400; letter-spacing: normal; text-align: start; text-indent: 0px;"> 
   <li><b>Select a delegated administrator</b>: From the <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/organizations" rel="noopener" target="_blank">AWS Organizations</a></span> management account, you can set a delegated administrator for your organization. As a best practice, we recommend using the same delegated administrator across security services for consistent governance and according to our <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/introduction.html" rel="noopener" target="_blank">AWS Security Reference Architecture (AWS SRA)</a></span>.</li> 
   <li><b>Select accounts in scope</b>: Define accounts you want to have Security Hub enabled for.</li> 
   <li><b>Define AWS Regions</b>: Determine Regional restrictions or considerations.</li> 
   <li><b>Determine AWS service integrations</b>: In addition to the core security capabilities of posture management and vulnerability management, Security Hub integrates signals from other AWS security services such as GuardDuty and Macie.</li> 
   <li><b>Define third-party integrations</b>: 
    <ul class="rte2-style-ul" id="rte-e26ac911-9a47-11f0-aa3d-fbe48dae450c" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px;"> 
     <li>For ticketing, Security Hub integrates with popular service management systems such as Atlassian’s Jira Service Management Cloud and ServiceNow.</li> 
     <li>Partners who already support or intend to support the OCSF schema to receive findings from Security Hub include companies such as Arctic Wolf, CrowdStrike, DataBee, Datadog, DTEX Systems, <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/security/how-to-prioritize-security-risks-using-aws-security-hub-exposure-findings/" rel="noopener" target="_blank">Dynatrace</a></span>, Fortinet, IBM, <span class="LinkEnhancement"><a class="Link" href="https://community.netskope.com/discussions-37/level-up-your-cloud-security-integrating-netskope-and-aws-security-hub-2-0-7806" rel="noopener" target="_blank">Netskope</a></span>, <span class="LinkEnhancement"><a class="Link" href="https://orca.security/resources/blog/enhanced-cspm-cdr-aws-security-hub/" rel="noopener" target="_blank">Orca Security</a></span>, Palo Alto Neworks, Rapid7, Securonix, <span class="LinkEnhancement"><a class="Link" href="https://www.sentinelone.com/blog/aws-reinforce-announcements/" rel="noopener" target="_blank">SentinelOne</a></span>, Sophos, Splunk, <span class="LinkEnhancement"><a class="Link" href="https://help.sumologic.com/docs/integrations/amazon-aws/security-hub/" rel="noopener" target="_blank">Sumo Logic</a></span>, <span class="LinkEnhancement"><a class="Link" href="https://www.tines.com/blog/tines-aws-security-hub/" rel="noopener" target="_blank">Tines</a></span>, Trellix, <span class="LinkEnhancement"><a class="Link" href="https://www.wiz.io/blog/wiz-and-aws-security-hub-enhance-cloud-risk-prioritization" rel="noopener" target="_blank">Wiz</a></span>, and Zscaler.</li> 
     <li>Service partners such as <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/apn/how-accenture-accelerates-building-a-secure-cloud-foundation-natively-on-aws/" rel="noopener" target="_blank">Accenture,</a></span> Caylent, <span class="LinkEnhancement"><a class="Link" href="https://www.deloitte.com/us/en/services/consulting/services/ConvergeSECURITY-cloud-security.html" rel="noopener" target="_blank">Deloitte</a></span>, IBM, and <span class="LinkEnhancement"><a class="Link" href="https://www.optiv.com/partners/amazon-web-services-aws" rel="noopener" target="_blank">Optiv</a></span> can help you adopt Security Hub and the OCSF schema.</li> 
    </ul> </li> 
   <li><b>Use the Security Hub cost estimator</b>: Use the <span class="LinkEnhancement"><a class="Link" href="https://github.com/aws-samples/sample-AWS-Security-Hub-Cost-Estimation-Tool" rel="noopener" target="_blank">Security Hub Cost Estimation Tool</a></span> for a pre-enablement cost estimate based on your current spend on Amazon Inspector, Security Hub CSPM, and GuardDuty.</li> 
  </ul> 
  <div class="RichTextHeading"> 
   <h2>Prepare for deployment</h2> 
  </div> 
  <p>After determining your success criteria and Security Hub configuration, identify stakeholders, desired state, and timeframe. Prepare for deployment by completing:</p> 
  <ul class="rte2-style-ul" id="rte-f0df70c0-1686-11f1-b2e6-3f4f75780a41" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px; color: #333333; font-family: AmazonEmber, 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; font-style: normal; font-weight: 400; letter-spacing: normal; text-align: start; text-indent: 0px;"> 
   <li><b>Project plan and timeline:</b> Develop a project plan with defined success criteria, scope boundaries, key milestones, and realistic implementation timelines. Suggested timeline of events: 
    <ul class="rte2-style-ul" id="rte-f0df97d0-1686-11f1-b2e6-3f4f75780a41" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px;"> 
     <li>Before enablement: 
      <ul class="rte2-style-ul" id="rte-f0df97d1-1686-11f1-b2e6-3f4f75780a41" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px;"> 
       <li>Validate core security service configuration for GuardDuty, Amazon Inspector, Security Hub CSPM, and Macie</li> 
       <li>Request approvals for free trial from appropriate stakeholders</li> 
      </ul> </li> 
     <li>Day 0 – Enable the service, become comfortable with the Security Hub layout and begin training security personnel</li> 
     <li>Week 1 – Validate desired coverage of threat detection, vulnerability management, and posture management across accounts and Regions</li> 
     <li>Week 2 – Connect to IT service management (ITSM) tools and begin creating automations for critical workloads and resources</li> 
     <li>Week 3 – Execute a tabletop exercise in response to a selected exposure finding</li> 
     <li>Week 4 – Analyze trends of threats and exposures from day 1 through week 4</li> 
    </ul> </li> 
   <li><b>Identify stakeholders:</b> Identify CISO, information security teams, SOC personnel, incident response teams, security engineers, finance, legal, compliance, external MSSPs, and business unit representatives.</li> 
   <li><b>Develop a RACI matric:</b> Create a detailed RACI chart defining roles and responsibilities across the incident response lifecycle, facilitating accountability and proper communication channels.</li> 
   <li><b>Configure management account access:</b> Secure authorization to delegate administrative access. For more information, see <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-v2-set-da.html" rel="noopener" target="_blank">Permissions required to designate a delegated Security Hub administrator account</a></span>.</li> 
   <li><b>Set up IAM roles and permissions:</b> Use <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/iam" rel="noopener" target="_blank">AWS Identity and Access Management (IAM)</a></span> roles to implement role-based access controls aligned with the RACI chart, including case management, escalation, and read-only roles using AWS managed policies. For more information, see <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/security-ir/latest/userguide/aws-managed-policies.html" rel="noopener" target="_blank">AWS Managed Policies</a></span></li> 
  </ul> 
  <div class="RichTextHeading"> 
   <h2>Enable Security Hub</h2> 
  </div> 
  <p>AWS security services integrate with AWS Organizations to help you centrally manage Security Hub.</p> 
  <ol class="rte2-style-ol" id="rte-66a84f61-9a46-11f0-aa3d-fbe48dae450c" start="1" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px; color: #333333; font-family: AmazonEmber, 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; font-style: normal; font-weight: 400; letter-spacing: normal; text-align: start; text-indent: 0px;"> 
   <li>If you haven’t already done so, enable Security Hub CSPM and Amazon Inspector at a minimum. Also enable any other AWS security services that you want to integrate with Security Hub.</li> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-v2-enable.html" rel="noopener" target="_blank">Enable Security Hub for your organization</a></span> from the organization management account.</li> 
   <li>If setting a delegated administrator for Security Hub, see <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-v2-set-da.html" rel="noopener" target="_blank">Setting a delegated administrator account in Security Hub</a></span> from the management account.<br /> 
    <blockquote>
     <p><b>Note</b>: As a best practice, we recommend using the same delegated administrator across security services for consistent governance.</p>
    </blockquote> </li> 
   <li>Sign in to the delegated administrator with an IAM <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-v2-da-policy.html" rel="noopener" target="_blank">policy</a></span> that gives you permission to enable and disable member accounts. With this policy, you will have granular control to decide what Regions you want enabled.</li> 
   <li>Configure Security Hub plans for deployment. <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/security-hub/pricing/" rel="noopener" target="_blank">Security Hub comes with the Essentials, Threat Analytics, and Extended plans</a></span>.</li> 
   <li>Configure <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-partner-providers.html" rel="noopener" target="_blank">third-party integrations</a></span> to create incidents or issues for Security Hub findings.</li> 
  </ol> 
  <blockquote>
   <p><b>Note</b>: After you enable Security Hub, exposure findings in your environment are created and analyzed immediately. However, it can take up to 6 hours to receive an exposure finding for a resource.</p>
  </blockquote> 
  <div class="RichTextHeading"> 
   <h2>Validate deployment</h2> 
  </div> 
  <p>The final step is to confirm that Security Hub is configured correctly and to evaluate the solution against your success criteria.</p> 
  <ul class="rte2-style-ul" id="rte-e26ac912-9a47-11f0-aa3d-fbe48dae450c" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px; color: #333333; font-family: AmazonEmber, 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; font-style: normal; font-weight: 400; letter-spacing: normal; text-align: start; text-indent: 0px;"> 
   <li><b>Validate policy</b>: Verify that you have the correct permissions to manage member accounts and Regional restrictions are configured correctly.</li> 
   <li><b>Validate integrations</b>: Verify that tickets with <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/securityhub/latest/userguide/security-hub-adv-servicenow-view-ticket.html" rel="noopener" target="_blank">ServiceNow</a></span> or <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/securityhub/latest/userguide/security-hub-adv-jiracloud-view-ticket.html" rel="noopener" target="_blank">Jira Cloud</a></span> are working correctly by signing in to the AWS Management Console for Security hub and choosing <b>Inventory</b> in the navigation pane. Select <b>Findings</b> and verify there is a ticket ID in your finding.</li> 
   <li><b>Assess success criteria</b>: Determine if you achieved the success criteria that you defined at the beginning of the project.</li> 
  </ul> 
  <div class="RichTextHeading"> 
   <h2>Conclusion</h2> 
  </div> 
  <p>In this post, we showed you how to plan and implement an effective Security Hub POC. You learned how to do so through phases, including defining success criteria, configuring Security Hub, and validating that Security Hub meets your business needs. Take advantage of the trial periods to maximize your testing window without incurring significant costs. Throughout the POC, maintain focus on your predefined success criteria while remaining open to unexpected benefits or challenges that might arise. Maintain open communication with your AWS account team to address any questions or concerns to help you get the most out of your Security Hub POC experience.</p> 
  <div class="RichTextHeading"> 
   <h2>Additional resources</h2> 
  </div> 
  <ul class="rte2-style-ul" id="rte-e26ac915-9a47-11f0-aa3d-fbe48dae450c" style="padding: 0px 0px 0px 20px; margin: 0px 0px 0px 2px; color: #333333; font-family: AmazonEmber, 'Helvetica Neue', Helvetica, Arial, sans-serif; font-size: 14px; font-style: normal; font-weight: 400; letter-spacing: normal; text-align: start; text-indent: 0px;"> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-security-hub-adv.html" rel="noopener" target="_blank">AWS Security Hub User Guide</a></span></li> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://www.google.com/url?sa=t&amp;source=web&amp;rct=j&amp;opi=89978449&amp;url=https://aws.amazon.com/blogs/security/how-to-prioritize-security-risks-using-aws-security-hub-exposure-findings/&amp;ved=2ahUKEwjn5oa45JWOAxWbMUQIHXqzHH0QFnoECCAQAQ&amp;usg=AOvVaw1n4PSwJlJw1VurCMWdfVOz" rel="noopener" target="_blank">How to prioritize security risks using AWS Security Hub</a></span></li> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://www.youtube.com/watch?v=prtnhCfjUpM" rel="noopener" target="_blank">How to onboard to AWS Security Hub</a></span></li> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/security/streamline-security-response-at-scale-with-aws-security-hub-automation/" rel="noopener" target="_blank">Streamline security response at scale with AWS Security Hub automation</a></span></li> 
  </ul> 
  <p>If you have feedback about this post, submit comments in the Comments section below. If you have questions about this post, <span class="LinkEnhancement"><a class="Link" href="https://console.aws.amazon.com/support/home" rel="noopener" target="_blank">contact AWS Support</a></span>. </p>
 </div> 
</div> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image"> 
   <img alt="Kyle Shields" class="aligncenter size-full wp-image-39920" height="160" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/09/19/shieldky.jpg" width="120" /> 
  </div> 
  <h3 class="lb-h4">Kyle Shields</h3> 
  <p>Kyle is a Security Specialist Solutions Architect focused on threat detection and incident response at AWS. Today, he’s focused on helping enterprise AWS customers adopt and operationalize AWS Security Incident Response and improve their security posture.</p> 
  <p></p>
 </div> 
</footer> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image"> 
   <img alt="" class="aligncenter size-full wp-image-40948" height="160" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/24/ahmed-adekunle-author.jpg" width="120" /> 
  </div> 
  <h3 class="lb-h4">Ahmed Adekunle</h3> 
  <p>Ahmed is a Security Specialist Solutions Architect focused on detection and response services at AWS. Before AWS, his background was in business process management and AWS technology consulting, helping customers use cloud technology to transform their business. Outside of work, Ahmed enjoys playing soccer, supporting less privileged activities, traveling, and eating spicy food, specifically African cuisine.</p> 
  <p></p>
 </div> 
</footer> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image"> 
   <img alt="Author" class="aligncenter size-full wp-image-22626" height="160" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2021/10/20/Marshall-Jones-Author.jpg" width="120" /> 
  </div> 
  <h3 class="lb-h4">Marshall Jones</h3> 
  <p>Marshall is a Worldwide Security Specialist Solutions Architect at AWS. His background is in AWS consulting and security architecture and focused on a variety of security domains including edge, threat detection, and compliance. Today, he’s focused on helping enterprise AWS customers adopt and operationalize AWS security services to increase security effectiveness and reduce risk.</p> 
  <p></p>
 </div> 
</footer>
