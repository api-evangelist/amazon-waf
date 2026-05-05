---
title: "A technical walkthrough of multicloud full-stack security using AWS Security Hub Extended"
url: "https://aws.amazon.com/blogs/security/a-technical-walkthrough-of-multicloud-full-stack-security-using-aws-security-hub-extended/"
date: "Wed, 22 Apr 2026 16:31:11 +0000"
author: "Matt Meck"
feed_url: "https://aws.amazon.com/blogs/security/feed/"
---
<div class="Page-articleBody"> 
 <div class="RichTextArticleBody RichTextBody"> 
  <p>Building on our recent announcement of <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/aws/aws-security-hub-extended-o%EF%AC%80ers-full-stack-enterprise-security-with-curated-partner-solutions/" rel="noopener" target="_blank">AWS Security Hub Extended</a></span> —our full-stack enterprise security offering — we want to show you how we’re simplifying security procurement and operations for your multicloud environments. Whether you’re a security architect evaluating solutions or a CISO looking to streamline vendor management, this post walks through the streamlined experience that transforms how you acquire, deploy, and manage end-to-end enterprise security solutions across endpoint, identity, email, network, data, browser, cloud, AI, and security operations. Security Hub Extended brings together AWS security services with carefully curated security partners. Delivering better outcomes together through unified procurement, billing, and operations that significantly reduce vendor management overhead so you can focus on what matters most: protecting your organization.</p> 
  <div class="RichTextHeading"> 
   <h2>The challenge we’re addressing</h2> 
  </div> 
  <p>Security teams today spend too much time on vendor management, evaluating services, negotiating contracts, and managing multiple billing cycles instead of focusing on what matters most: managing risk. But the procurement challenge runs even deeper. Until now, customers really only had one option: sign multi-year agreements based solely on proof-of-concept testing and estimated annual usage. This forces organizations to commit budget before they can validate whether a solution will work for them at scale.</p> 
  <p>AWS Security Hub Extended transforms this procurement model. Security Hub Extended offers customers the option to get started with pay-as-you-go pricing and no commitments, so they can move fast and validate solutions in their actual environment. After they’ve confirmed a solution works at scale, they can then align their vendor strategy and sign longer-term commitments for even more favorable pricing.</p> 
  <p>Security Hub Extended provides a curated set of carefully chosen partner solutions with competitive pricing, unified billing through your AWS account, and seamless integration. Our initial launch partners, selected by customers for their proven value, include <span class="LinkEnhancement"><a class="Link" href="https://7ai.com/" rel="noopener" target="_blank">7AI</a></span>, <span class="LinkEnhancement"><a class="Link" href="https://docs.britive.com/" rel="noopener" target="_blank">Britive</a></span>, <span class="LinkEnhancement"><a class="Link" href="https://www.crowdstrike.com/en-us/" rel="noopener" target="_blank">CrowdStrike</a></span>, <span class="LinkEnhancement"><a class="Link" href="https://www.cyera.com/" rel="noopener" target="_blank">Cyera</a></span>, <span class="LinkEnhancement"><a class="Link" href="https://www.island.io/" rel="noopener" target="_blank">Island</a></span>, <span class="LinkEnhancement"><a class="Link" href="https://noma.security/?utm_source=google&amp;utm_medium=cpc&amp;utm_campaign=brand&amp;utm_content=177923075690&amp;utm_term=noma%20security&amp;gad_source=1&amp;gad_campaignid=22804178544&amp;gbraid=0AAAABAbL-6OFVMrIaMUodX8OsdkQvFwoS&amp;gclid=CjwKCAjw-dfOBhAjEiwAq0RwI-POH2mDpOnmvP0sTOkAqukVj_NN44WGBndRlWeeMAgsidrorMz9MxoCevsQAvD_BwE" rel="noopener" target="_blank">Noma</a></span>, <span class="LinkEnhancement"><a class="Link" href="https://www.okta.com/" rel="noopener" target="_blank">Okta</a></span>, <span class="LinkEnhancement"><a class="Link" href="https://www.oligo.security/" rel="noopener" target="_blank">Oligo</a></span>, <span class="LinkEnhancement"><a class="Link" href="https://www.opti.ai/" rel="noopener" target="_blank">Opti</a></span>, <span class="LinkEnhancement"><a class="Link" href="https://www.proofpoint.com/us" rel="noopener" target="_blank">Proofpoint</a></span>, <span class="LinkEnhancement"><a class="Link" href="https://www.sailpoint.com/" rel="noopener" target="_blank">SailPoint</a></span>, <span class="LinkEnhancement"><a class="Link" href="https://www.splunk.com/" rel="noopener" target="_blank">Splunk</a></span>, <span class="LinkEnhancement"><a class="Link" href="https://www.upwind.io/" rel="noopener" target="_blank">Upwind</a></span>, and<span class="LinkEnhancement"><a class="Link" href="https://www.zscaler.com/" rel="noopener" target="_blank"> Zscaler.</a></span></p> 
  <div class="RichTextHeading"> 
   <h2>Getting started with Security Hub Extended</h2> 
  </div> 
  <p><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/security-hub/" rel="noopener" target="_blank">AWS Security Hub</a></span> consolidates threat analytics from <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/guardduty/" rel="noopener" target="_blank">Amazon GuardDuty</a></span>, vulnerability management from <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/inspector/" rel="noopener" target="_blank">Amazon Inspector</a></span>, and sensitive data discovery from <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/macie/" rel="noopener" target="_blank">Amazon Macie</a></span>, correlating these signals with Security Hub Exposure findings to determine overall risk, reachability, and assumability. Security Hub Extended builds on this foundation by adding curated partner solutions, extending these unified security operations across your entire organization including multicloud, on-premises, and endpoint environments. If you’re already using Security Hub, you can navigate directly to the Extended plan section.</p> 
  <p>Getting started with Security Hub is straightforward. From the AWS Management Console, search for <code class="CodeInline" style="color: #000;">Security Hub</code> to start the onboarding walkthrough. If you’re not already a Security Hub customer, you can quickly complete onboarding by designating an AWS organization <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-v2-set-da.html" rel="noopener" target="_blank">delegated administrator (DA)</a></span> account. You can then centrally enable and manage Security Hub across your entire organization’s accounts and AWS Regions from a single location (see <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub-v2.html" rel="noopener" target="_blank">Introduction to AWS Security Hub</a></span>). After you’ve onboarded, navigate to the <b>Extended plan</b> section to add curated partner solutions.</p> 
  <div class="wp-caption aligncenter" id="attachment_41871" style="width: 595px;">
   <img alt="Figure 1- Security Hub centralized configuration" class="size-full wp-image-41871" height="500" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/22/Figure-1-Security-Hub-centralized-configuration.png" width="585" />
   <p class="wp-caption-text" id="caption-attachment-41871">Figure 1: Security Hub centralized configuration</p>
  </div> 
  <p>From this single interface, you can enable detection and response capabilities across your entire organization, provide granular configurations at the organizational unit or member account level, select specific Regions, and turn individual features on or off as needed.</p> 
  <div class="RichTextHeading"> 
   <h2>Understanding risk through attack paths</h2> 
  </div> 
  <p>The Security Hub risk correlation engine identifies potential exposures by correlating threats, vulnerabilities, and misconfigurations to reveal how they connect and could lead to compromise of critical resources.</p> 
  <div class="wp-caption aligncenter" id="attachment_41870" style="width: 1613px;">
   <img alt="Figure 2 - Security Hub exposure attack path visualization" class="size-full wp-image-41870" height="500" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/22/Figure-2-Security-Hub-exposure-attack-path-visualization.png" width="1603" />
   <p class="wp-caption-text" id="caption-attachment-41870">Figure 2: Security Hub exposure attack path visualization</p>
  </div> 
  <p>The attack path visualization in the preceding figure reveals critical insights including upstream root causes and blast radius, showing the potential impact if a threat actor exploits a vulnerability. You can use this visualization to focus on fixing the root cause rather than addressing symptoms. For example, updating one security group configuration can eliminate the entire attack path, cutting off all downstream exposure.</p> 
  <div class="RichTextHeading"> 
   <h2>Accessing Security Hub Extended</h2> 
  </div> 
  <p>You can find Security Hub Extended, shown in the following figure, in the left navigation pane under <b>Management</b> in your Security Hub delegated administrator (DA) account; Security Hub Extended will only be visible from the delegated administrator account. The Extended plan brings curated third-party security solutions directly into the Security Hub experience. Because Extended is built into Security Hub, there’s no separate console to manage. You discover, subscribe to, and operate curated partner solutions from the same place you manage enterprise security, delivering unified operations across your entire security estate.</p> 
  <div class="wp-caption aligncenter" id="attachment_41869" style="width: 487px;">
   <img alt="Figure 3- Security Hub Extended partners" class="size-full wp-image-41869" height="500" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/22/Figure-3-Security-Hub-Extended-partners.png" width="477" />
   <p class="wp-caption-text" id="caption-attachment-41869">Figure 3: Security Hub Extended partners</p>
  </div> 
  <hr /> 
  <p> <span class="LinkEnhancement"><a class="Link" href="https://www.youtube.com/embed/ysrsXdi9cDY?si=EEq0X9CsUxgrIBOB" rel="noopener" target="_blank"></a></span></p>
  <div class="wp-caption aligncenter" id="attachment_41879" style="width: 310px;">
   <a class="Link" href="https://www.youtube.com/embed/ysrsXdi9cDY?si=EEq0X9CsUxgrIBOB" rel="noopener" target="_blank"><img alt="AWS Security Hub Extended - Overview and Demo | Amazon Web Services" class="size-medium wp-image-41879" height="179" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/22/YouTube-video-embed-300x179.png" width="300" /><p class="wp-caption-text" id="caption-attachment-41879">AWS Security Hub Extended – Overview and Demo | Amazon Web Services</p></a>
  </div>
  <p></p> 
  <hr /> 
  <div class="RichTextHeading"> 
   <h2>Transparent, competitive pricing consolidated with Security Hub</h2> 
  </div> 
  <p>Unlike traditional third-party engagements that require lengthy negotiations, private pricing deals, and multi-year commitments, Security Hub Extended offers complete pricing transparency. Every partner solution displays clear, competitive<span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/security-hub/pricing/" rel="noopener" target="_blank"> monthly pay-as-you-go rates</a></span> billed directly with Security Hub requiring no commitments. For example, Cloud Security from Upwind costs $3.75 per resource per month, and Identity Security from Okta costs $20 per user per month.</p> 
  <p>All Security Hub Extended offerings are also eligible for AWS Enterprise Discount Program (EDP) discounts that will be applied automatically. If you have an existing AWS enterprise discount agreement, those discounts automatically apply to Security Hub Extended offerings, further reducing your effective costs. All partner solutions you deploy through Security Hub Extended appear on your consolidated AWS bill, no separate invoices or payment processes.</p> 
  <div class="RichTextHeading"> 
   <h2>Streamlined onboarding</h2> 
  </div> 
  <p>Adopting curated partner solutions through Security Hub Extended is straightforward. Choose <b>View Product</b> to initiate an automated workflow. Depending on the solution, you’ll either be directed to the partner onboarding console or provide information for the partner to guide you through their onboarding process tailored to your environment.</p> 
  <p>Billing begins only after you’re fully activated on the partner solution and starts automatically, no additional action is required to benefit from the unified billing. If you’re already using one of the curated partner solutions, transitioning to Security Hub Extended for consolidated billing and flexible pricing won’t disrupt your current services. Now, instead of receiving separate invoices for each partner in addition to Amazon Inspector, GuardDuty, and Security Hub CSPM you get one unified bill through Security Hub. This consolidates visibility to support better understanding of spend and to manage cost.</p> 
  <div class="RichTextHeading"> 
   <h2>Unified operations</h2> 
  </div> 
  <p>Security Hub Extended unifies security operations by consolidating findings from AWS and curated partner solutions. All findings use the <span class="LinkEnhancement"><a class="Link" href="https://ocsf.io/" rel="noopener" target="_blank">Open Cybersecurity Schema Framework</a></span> (OCSF) for consistency, without the need for complex data normalization, transformation, and extract, transform, and load (ETL) processes.</p> 
  <p>When you deploy solutions such as CrowdStrike, Noma, and Upwind alongside Splunk and 7AI through Security Hub Extended, security findings automatically flow into Security Hub and then seamlessly route to Splunk and 7AI. All in OCSF format so your security team can focus on responding to threats, not managing pipelines, so you can quickly identify and respond to security risks that span boundaries—from endpoint compromises to cloud infrastructure—without spending valuable time on manual integration work.</p> 
  <div class="RichTextHeading"> 
   <h2>The full-stack security vision</h2> 
  </div> 
  <p>Security Hub Extended represents a shift in how you discover, procure, and build comprehensive security programs. Instead of managing dozens of vendor relationships, negotiating separate contracts, agreeing to multi-year annual commitments, and integrating disparate tools, you now have one procurement process through AWS, one bill with transparent competitive pay-as-you-go pricing, one console for unified security operations, one support channel for AWS Enterprise Support customers, and one schema (OCSF) for all security findings. The result: reduced security risk, improved team productivity, and a more unified approach to security operations across your enterprise.</p> 
  <div class="RichTextHeading"> 
   <h2>Get started</h2> 
  </div> 
  <p>Try <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/security-hub/?trk=d8ec3b19-0f37-4f8c-8c12-189f913e205c&amp;sc_channel=el" rel="noopener" target="_blank">Security Hub Extended</a></span> today and experience how simplified procurement and unified operations can transform your security program. Security Hub Extended is generally available globally in all AWS commercial Regions where Security Hub is available. We’ve also published a <span class="LinkEnhancement"><a class="Link" href="https://www.youtube.com/watch?v=ysrsXdi9cDY" rel="noopener" target="_blank">walk through video</a></span> to further explain how Security Hub Extended works.</p> 
  <p>It’s still Day 1, but we’re iterating fast, so share your feedback with us on <span class="LinkEnhancement"><a class="Link" href="https://repost.aws/topics/TAEEfW2o7QS4SOLeZqACq9jA/security-identity-compliance" rel="noopener" target="_blank">AWS re:Post</a></span> for Security Hub or through your AWS Support contacts and watch for future blog posts on our progress.</p> 
  <hr /> 
 </div> 
</div> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="Matt Meck" class="aligncenter size-full wp-image-22385" height="160" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/22/Matt-meck-author-2026.jpg" width="120" />
  </div> 
  <h3 class="lb-h4">Matt Meck</h3> 
  <p>Matt is a Worldwide Security Specialist at Amazon Web Services, based in New York, with 10 years of experience in the tech industry. For the past 4 years at AWS, he’s focused on Detection and Response, helping solve complex security challenges in the rapidly evolving security space. He works closely with product teams, customers, partners, and field teams to deliver effective security solutions.</p> 
  <p>&nbsp;</p> 
 </div> 
 <div class="blog-author-box"> 
  <div class="blog-author-image">
   <img alt="Michael Fuller" class="aligncenter size-full wp-image-22385" height="160" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/22/michael-fuller-author.jpg" width="120" />
  </div> 
  <h3 class="lb-h4">Michael Fuller</h3> 
  <p>Michael has been with AWS for 16 years and led product for AWS Security Services for 11 years. Michael has 29 years in the industry and held several roles in product management, business development, and software development for IBM, Cisco, and Amazon. Michael has a Bachelor’s of Science in Computer Engineering from the University of Arizona and an MBA from the University of Washington.</p> 
  <p>&nbsp;</p> 
 </div> 
</footer>
