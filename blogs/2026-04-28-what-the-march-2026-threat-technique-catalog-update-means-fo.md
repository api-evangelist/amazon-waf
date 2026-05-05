---
title: "What the March 2026 Threat Technique Catalog update means for your AWS environment"
url: "https://aws.amazon.com/blogs/security/what-the-march-2026-threat-technique-catalog-update-means-for-your-aws-environment/"
date: "Tue, 28 Apr 2026 19:01:37 +0000"
author: "Shannon Brazil"
feed_url: "https://aws.amazon.com/blogs/security/feed/"
---
<div class="Page-articleBody"> 
 <div class="RichTextArticleBody RichTextBody"> 
  <p>The AWS Customer Incident Response Team (AWS CIRT) regularly encounters patterns that repeat across their engagements when helping customers respond to security incidents. We’re passionate about making sure that information is widely accessible so that everyone can improve their security posture and their organization’s resilience to disruption. The primary method we use to share this information is the <span class="LinkEnhancement"><a class="Link" href="https://aws-samples.github.io/threat-technique-catalog-for-aws/" rel="noopener" target="_blank">Threat Technique Catalog for AWS (TTC)</a></span>. The latest update to the catalog for March 2026 addresses identity, persistence, infrastructure destruction, and privilege escalation. Each new entry reflects something we’ve encountered in practice, and each provides straightforward mitigations. This post breaks down what changed, why it matters, and what you can do about it today.</p> 
  <div class="RichTextHeading"> 
   <h2>What we’re seeing</h2> 
   <p></p>
  </div> 
  <p>Based on recent observations, we’ve added three new entries to the TTC.</p> 
  <div class="RichTextHeading"> 
   <h3>Cognito refresh token abuse: The quiet persistence mechanism</h3> 
   <p></p>
  </div> 
  <p><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/cognito" rel="noopener" target="_blank">Amazon Cognito</a></span> refresh tokens are designed for convenience. They let applications obtain new access and ID tokens without requiring users to re-authenticate. The default lifetime is 30 days and is configurable up to 10 years. Cognito provides the flexibility to address a wide range of use cases, however the AWS CIRT has seen this lifetime window used by threat actors in an unauthorized way to maintain persistence by refreshing credentials.</p> 
  <p>When a threat actor obtains a valid refresh token—through credential theft, compromised client-side storage, or elevated permissions—they can call <code class="CodeInline" style="color: #000;">cognito-idp:GetTokensFromRefreshToken</code> to silently generate fresh tokens. The legitimate user’s session continues normally because their application independently refreshes tokens as needed—the threat actor’s refresh calls don’t invalidate the user’s token. This creates a parallel, persistent foothold that’s invisible to the user. In environments where refresh token rotation isn’t enabled, the same token can be reused indefinitely within its validity window.</p> 
  <p>This method of gaining persistent access is often overlooked by response teams who were confident that the initial compromise was contained, only to discover ongoing unauthorized access weeks later through a refresh token they didn’t know existed.</p> 
  <p>Enabling refresh token rotation and reducing the lifetime of tokens can help mitigate this risk. Dive deeper in the TTC (<span class="LinkEnhancement"><a class="Link" href="https://aws-samples.github.io/threat-technique-catalog-for-aws/Techniques/T1098.A006.html" rel="noopener" target="_blank">T1098.A006</a></span>).</p> 
  <div class="RichTextHeading"> 
   <h3>AMI image deletion: Targeting recovery capabilities</h3> 
   <p></p>
  </div> 
  <p>Amazon Machine Images (AMI) are a core part of many solutions and foundational to disaster recovery. They often contain the operating system, application configurations, and everything needed to rebuild your infrastructure. Threat actors know this, and we’re seeing <code class="CodeInline" style="color: #000;">ec2:DeregisterImage</code> used to make it more difficult to recover from an incident.</p> 
  <p>By default, when an AMI is deregistered, it’s gone. Recycle Bin retention rules can allow the recovery of the AMI, but if you haven’t explicitly enabled that functionality, there’s no way to undo the deregister action. Working with customers, we’ve seen cases where the impact of this action goes beyond the immediate loss because the threat actors have also removed the <i>golden images</i> the teams planned to restore from.</p> 
  <p>The TTC has more information about how to detect and mitigate this technique, including how to enable Recycle Bin retention rules for key AMIs (<span class="LinkEnhancement"><a class="Link" href="https://aws-samples.github.io/threat-technique-catalog-for-aws/Techniques/T1098.A006.html" rel="noopener" target="_blank">T1485.A002</a></span>).</p> 
  <div class="RichTextHeading"> 
   <h3>Additional cloud roles: The trust policy blind spot</h3> 
   <p></p>
  </div> 
  <p>We’ve updated <span class="LinkEnhancement"><a class="Link" href="https://aws-samples.github.io/threat-technique-catalog-for-aws/Techniques/T1098.003.html" rel="noopener" target="_blank">T1098.003: Additional Cloud Roles</a></span> to now include <code class="CodeInline" style="color: #000;">UpdateAssumeRolePolicy</code> as a tracked API call. We’ve seen an increase in the use of this call to avoid detections set to flag new role creation (<code class="CodeInline" style="color: #000;">iam:CreateRole</code>). By modifying the trust policy of an existing role, a threat actor with sufficient permissions can use <code class="CodeInline" style="color: #000;">UpdateAssumeRolePolicy</code> to subtly add an external account or an identity they control. No new roles appear. No new policies are created. The existing role simply trusts a new principal which the threat actor can assume.</p> 
  <p>This persistence and privilege escalation technique blends into the volume of normal <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/iam" rel="noopener" target="_blank">AWS Identity and Access Management (IAM)</a></span> operations. It’s especially effective in environments with a large number of roles where trust policy changes aren’t actively monitored.</p> 
  <div class="RichTextHeading"> 
   <h2>The current trend</h2> 
   <p></p>
  </div> 
  <p>A common thread runs through all three of these updates: threat actors are using subtle, default, or unexpected behaviors to sidestep detection. Refresh tokens working as designed. AMI deregistration completing without guardrails. Trust policies being modified through legitimate API calls. These actions might not trigger alarms in most environments because they look like normal operations.</p> 
  <p>This is a shift worth paying attention to. Rather than relying on novel exploits or zero-days, the techniques we’re cataloging reflect threat actors who understand how cloud services work and use that knowledge to hide in plain sight. The implication for security teams is clear: prevention and detection strategies need to mature beyond monitoring for obviously malicious actions. Customers need to be watching for legitimate actions happening in illegitimate context—such as the right API call, made by the wrong principal, at the wrong time.</p> 
  <p>The Threat Technique Catalogue for AWS is designed to help with exactly this. Each technique entry includes detection guidance and mitigations specific to AWS environments. We encourage teams to review the relevant entries and assess whether their current monitoring would catch these patterns:</p> 
  <ul class="rte2-style-ul" id="rte-6c78ee52-2852-11f1-8d6b-970aa9e86c96"> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://aws-samples.github.io/threat-technique-catalog-for-aws/Techniques/T1098.A006.html" rel="noopener" target="_blank">T1098.A006: Cognito Refresh Token Abuse</a></span>: Are you monitoring for <code class="CodeInline" style="color: #000;">cognito-idp:GetTokensFromRefreshToken</code> from unexpected sources? Is refresh token rotation enabled?</li> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://aws-samples.github.io/threat-technique-catalog-for-aws/Techniques/T1485.A002.html" rel="noopener" target="_blank">T1485.A002: AMI Image Deletion</a></span>: Do you have Recycle Bin retention rules protecting your critical AMIs? Would you know if a production AMI was deregistered outside a maintenance window?</li> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://aws-samples.github.io/threat-technique-catalog-for-aws/Techniques/T1098.003.html" rel="noopener" target="_blank">T1098.003: Additional Cloud Roles</a></span>: Are trust policy modifications tracked and alerted on? Could an external account be added to an existing role without anyone noticing?</li> 
  </ul> 
  <p>Each of these techniques leaves traces in <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/cloudtrail" rel="noopener" target="_blank">AWS CloudTrail</a></span>, and the TTC provides specific guidance on what to watch for and how to respond.</p> 
  <div class="RichTextHeading"> 
   <h2>Looking ahead</h2> 
   <p></p>
  </div> 
  <p>The Threat Technique Catalog for AWS exists because we believe the patterns we observe during security engagements shouldn’t stay behind closed doors. When we see techniques repeating across customers, the most effective thing we can do is document them and make that knowledge available so you can act on it before you’re in the middle of an incident.</p> 
  <p>This March update adds three new entries, and the catalog will continue to evolve. Our team regularly updates it based on what we’re seeing in the real world when helping customers respond to security events. We encourage security teams to review the catalog regularly, incorporate its techniques into threat modeling exercises, and use it as a shared vocabulary for discussing cloud-specific threats.</p> 
  <p>Explore the full catalog:<span class="LinkEnhancement"><a class="Link" href="https://aws-samples.github.io/threat-technique-catalog-for-aws/matrix.html" rel="noopener" target="_blank"> Threat Technique Catalog for AWS</a></span></p> 
  <div class="RichTextHeading"> 
   <h3>Additional resources</h3> 
   <p></p>
  </div> 
  <ul class="rte2-style-ul" id="rte-bf7a0e80-2853-11f1-8d6b-970aa9e86c96"> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/storage/reduce-costs-with-customized-delete-protection-for-amazon-ebs-snapshots-and-ebs-backed-amis/" rel="noopener" target="_blank"><u>Reduce costs with customized delete protection for Amazon EBS Snapshots and EBS-backed AMIs</u></a></span></li> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/storage/protect-your-resources-from-deletions-through-rule-lock-for-recycle-bin/" rel="noopener" target="_blank"><u>Protect your resources from deletions through Rule Lock for Recycle Bin</u></a></span></li> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/security/how-to-monitor-optimize-and-secure-amazon-cognito-machine-to-machine-authorization/" rel="noopener" target="_blank"><u>How to monitor, optimize, and secure Amazon Cognito machine-to-machine authorization</u></a></span></li> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/security/aws-cirt-announces-the-launch-of-the-threat-technique-catalog-for-aws/" rel="noopener" target="_blank"><u>AWS CIRT announces the launch of the Threat Technique Catalog for AWS</u></a></span></li> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-using-the-refresh-token.html" rel="noopener" target="_blank"><u>Amazon Cognito refresh tokens documentation</u></a></span></li> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/guardduty/" rel="noopener" target="_blank"><u>Amazon GuardDuty</u></a></span></li> 
  </ul> 
  <p>If you have feedback about this post, submit comments in the <strong>Comments</strong> section below. </p> 
  <hr />
 </div> 
</div> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image"> 
   <img alt="Shannon Brazil" class="aligncenter size-full wp-image-22385" height="160" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/21/Shannon-brazil-author.jpg" width="120" /> 
  </div> 
  <h3 class="lb-h4">Shannon Brazil</h3> 
  <p>Shannon is a security engineer on the AWS Customer Incident Response Team (CIRT), specializing in digital forensics and cloud security investigations. Known in the community as 4n6lady, she is passionate about security education and mentoring the next generation of defenders. </p> 
  <p></p> 
 </div> 
 <div class="blog-author-box"> 
  <div class="blog-author-image"> 
   <img alt="Cydney Stude" class="aligncenter size-full wp-image-41972" height="160" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/28/study-cydney-stude-headshot.png" width="120" /> 
  </div> 
  <h3 class="lb-h4">Cydney Stude</h3> 
  <p>Cydney is a security engineer specializing in threat intelligence and incident response at AWS. Cydney works on the ground in incident response and is passionate about turning observables into security outcomes. Cydney is an author and maintainer of the Threat Technique Catalog for AWS.</p> 
  <p></p> 
 </div> 
</footer>
