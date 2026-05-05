---
title: "Access control with IAM Identity Center session tags"
url: "https://aws.amazon.com/blogs/security/access-control-with-iam-identity-center-session-tags/"
date: "Tue, 28 Apr 2026 16:33:06 +0000"
author: "Rashmi Iyer"
feed_url: "https://aws.amazon.com/blogs/security/feed/"
---
<p>As organizations expand their <a href="https://aws.amazon.com" rel="noopener noreferrer" target="_blank">Amazon Web Services (AWS)</a> footprint, managing secure, scalable, and cost-efficient access across multiple accounts becomes increasingly important. <a href="https://aws.amazon.com/iam/identity-center/" rel="noopener noreferrer" target="_blank">AWS IAM Identity Center</a> offers a centralized, unified solution for managing workforce access to AWS accounts. It simplifies authentication, enhances security, and provides a seamless user sign-in experience to AWS services across diverse environments.</p> 
<p>By combining IAM Identity Center <a href="https://docs.aws.amazon.com/singlesignon/latest/userguide/permissionsetsconcept.html" rel="noopener noreferrer" target="_blank">permission sets</a> with <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/id_session-tags.html" rel="noopener noreferrer" target="_blank">session tags</a>, organizations can unlock powerful capabilities for fine-grained access control and resource optimization. You can use session tags to pass dynamic attributes from your external identity provider into AWS, enabling more context-aware permissions and better cost visibility. This integration makes it possible to use advanced AWS features such as <a href="https://aws.amazon.com/blogs/big-data/introducing-aws-glue-usage-profiles-for-flexible-cost-control/" rel="noopener noreferrer" target="_blank">AWS Glue usage profiles</a> and <a href="https://aws.amazon.com/blogs/mt/configuring-aws-systems-manager-session-manager-support-federated-users-using-session-tags/" rel="noopener noreferrer" target="_blank">AWS Systems Manager Session Manager run as</a> to enforce fine-grained access control, so that administrators can dynamically map permissions and runtime configurations based on user attributes passed during federated access.</p> 
<p>In this post, I demonstrate how session tags derived from directory group attributes in Microsoft Entra ID can deliver functionality equivalent to <a href="https://aws.amazon.com/iam" rel="noopener noreferrer" target="_blank">AWS Identity and Access Management (IAM)</a> role tags. Using role tags, you can implement <a href="https://aws.amazon.com/identity/attribute-based-access-control/" rel="noopener noreferrer" target="_blank">attribute-based access control (ABAC)</a> using IAM Identity Center, while maintaining centralized and efficient access management. To demonstrate this, you can configure an AWS Glue usage profile, as described in <a href="https://aws.amazon.com/blogs/big-data/introducing-aws-glue-usage-profiles-for-flexible-cost-control/" rel="noopener noreferrer" target="_blank">Introducing AWS Glue usage profiles for flexible cost control</a>, where session tags can be passed through Identity Center and an external identity provider like Microsoft Entra ID. This approach is extensible to other AWS services such as AWS Systems Manager Session Manager (run as) and can also be used with other identity providers.</p> 
<h2>User authentication and IAM Identity Center Federation flow</h2> 
<p>The following figure shows the architecture and workflow of the solution. </p> 
<div class="wp-caption aligncenter" id="attachment_41901" style="width: 1437px;">
 <img alt="Figure 1 – User authentication and federation flow between Microsoft Entra and AWS" class="size-full wp-image-41901" height="666" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/23/Access-control-1.png" width="1427" />
 <p class="wp-caption-text" id="caption-attachment-41901">Figure 1 – User authentication and federation flow between Microsoft Entra and AWS</p>
</div> 
<p>The user authentication and federation flow includes the following steps:</p> 
<ol> 
 <li>User accesses application using a browser.</li> 
 <li>The enterprise application (configured in Azure) initiates authentication.</li> 
 <li>Microsoft Entra ID handles sign-in.</li> 
 <li>Users and groups are managed in Entra ID.</li> 
 <li>A SAML trust is established between Entra ID and IAM Identity Center.</li> 
 <li>SCIM provisioning syncs users and groups from Entra ID to AWS.</li> 
 <li>Synced users and groups appear in Identity Center.</li> 
 <li>Session tags are passed during SAML authentication. 
  <ul> 
   <li>Entra ID can send user attributes (department, role, cost center, project ID, and so on) as SAML attributes.</li> 
   <li>Identity Center consumes these as session tags, which are used for fine-grained access control and attribute-based access control inside AWS.</li> 
  </ul> </li> 
 <li>Admins define permission sets for users and groups in Identity Center.</li> 
 <li>Users get federated access to AWS using their Entra ID credentials.</li> 
 <li>Users sign in through AWS Management Console or <a href="https://aws.amazon.com/cli/" rel="noopener noreferrer" target="_blank">AWS Command Line Interface (AWS CLI)</a> using those permissions.</li> 
 <li>Access is granted to specific AWS accounts under <a href="https://aws.amazon.com/organizations" rel="noopener noreferrer" target="_blank">AWS Organizations</a>.</li> 
</ol> 
<h2>Prerequisites</h2> 
<p>To follow the steps in this post, you need the following prerequisites:</p> 
<ol> 
 <li>An <a href="https://docs.aws.amazon.com/singlesignon/latest/userguide/enable-identity-center.html" rel="noopener noreferrer" target="_blank">organization instance</a> of IAM Identity Center enabled.</li> 
 <li>A Microsoft Entra ID tenant. For more information, see <a href="https://learn.microsoft.com/en-us/entra/fundamentals/create-new-tenant" rel="noopener noreferrer" target="_blank">Quickstart: Create a new tenant in Microsoft Entra ID</a>.</li> 
 <li>Access to an external identity provider such as <a href="https://www.microsoft.com/en-ca/security/business/identity-access/microsoft-entra-id" rel="noopener noreferrer" target="_blank">Microsoft Entra ID</a> to federate users into AWS. You can enable federated access between Microsoft Entra ID and IAM Identity Center by completing the steps in <a href="https://docs.aws.amazon.com/singlesignon/latest/userguide/idp-microsoft-entra.html" rel="noopener noreferrer" target="_blank">Configure SAML and SCIM with Microsoft Entra ID and IAM Identity Center</a>. They include configuring SAML and SCIM integration between the two systems, testing the SAML connection to help ensure authentication is functioning correctly, and enabling SCIM synchronization to automate user and group provisioning.</li> 
</ol> 
<h2>Solution implementation</h2> 
<p>With the prerequisites in place, you’re ready to configure access control through IAM Identity center tags by using the following steps.</p> 
<ol> 
 <li>Create an AWS Glue usage profile as described in <a href="https://aws.amazon.com/blogs/big-data/introducing-aws-glue-usage-profiles-for-flexible-cost-control/" rel="noopener noreferrer" target="_blank">Introducing AWS Glue usage profiles for flexible cost control</a> in <strong>Create an AWS Glue usage profile</strong>. For the purposes of this post, create a profile named <em>developer</em>. 
  <ol type="a"> 
   <li>On the AWS Management Console for AWS Glue, choose&nbsp;<strong>Cost management</strong>&nbsp;in the navigation pane.</li> 
   <li>Choose&nbsp;<strong>Create usage profile</strong>.</li> 
   <li>For&nbsp;<strong>Usage profile name</strong>, enter&nbsp;<code style="color: #000000;">developer</code>.</li> 
   <li>Under&nbsp;<strong>Customize configurations for jobs</strong>, for&nbsp;<strong>Number of workers</strong>, for&nbsp;<strong>Default</strong>, enter&nbsp;<code style="color: #000000;">20</code>.</li> 
   <li>For&nbsp;<strong>Default worker type</strong>, select&nbsp;<strong>G.1X</strong>.</li> 
   <li>For&nbsp;<strong>Allowed worker types</strong>, select&nbsp;<strong>G.1X</strong>,&nbsp;<strong>G.2X</strong>,&nbsp;<strong>G.4X</strong>, and&nbsp;<strong>G.8X</strong>.</li> 
   <li>For&nbsp;<strong>Customize configurations for sessions</strong>, configure the same values.</li> 
   <li>Choose&nbsp;<strong>Create usage profile</strong>.</li> 
  </ol> <p> </p>
  <div class="wp-caption aligncenter" id="attachment_41902" style="width: 742px;">
   <img alt="Figure 2 – Glue usage profile creation on the console" class="size-full wp-image-41902" height="650" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/23/Access-control-2.png" width="732" />
   <p class="wp-caption-text" id="caption-attachment-41902">Figure 2 – Glue usage profile creation on the console</p>
  </div> </li> 
 <li>Create a <a href="https://docs.aws.amazon.com/singlesignon/latest/userguide/permissionsetcustom.html#permissionsetscmpconcept" rel="noopener noreferrer" target="_blank">custom permission set</a> instead of using predefined ones. Attach the following <a href="https://docs.aws.amazon.com/aws-managed-policy/latest/reference/about-managed-policy-reference.html" rel="noopener noreferrer" target="_blank">AWS Managed Policies</a> to the custom permission set: 
  <ul> 
   <li><code style="color: #000000;">AWSGlueConsoleFullAccess</code></li> 
   <li><code style="color: #000000;">IAMReadOnlyAccess</code></li> 
  </ul> 
  <blockquote>
   <p><strong>Note:</strong> For fine-grained access control, you can create custom permission sets by combining AWS managed, customer managed, and inline policies in IAM. In this post, you use AWS managed policies with intentionally broad permissions for simplicity. In production, always follow the principles of least privilege and scope permissions appropriately.</p>
  </blockquote> <p> By default, when you create a permission set, the permission set isn’t provisioned (used in any AWS accounts). To provision a permission set in an AWS account, you must assign IAM Identity Center access to users or groups in the account and then apply the permission set to those users and groups. For more information, see <a href="https://docs.aws.amazon.com/singlesignon/latest/userguide/assignusers.html" rel="noopener noreferrer" target="_blank">Assign user or group access to AWS accounts</a>. </p></li> 
 <li>Configure user attributes in Microsoft Entra ID for access control in IAM Identity Center as described in <strong>Step 5</strong> of <a href="https://docs.aws.amazon.com/singlesignon/latest/userguide/idp-microsoft-entra.html" rel="noopener noreferrer" target="_blank">Configure SAML and SCIM with Microsoft Entra ID and IAM Identity Center</a> to set up ABAC. Add claim conditions for attribute mapping based on Entra ID group membership. Assign the <code style="color: #000000;">developer</code> value for users in a corresponding group. This enables logic such as <em>Users in this group receive this profile</em> or <em>All users receive this profile</em>. When using an AWS Glue profile and when making API calls to create AWS Glue resources, admins need to tag the user or role with&nbsp;<code style="color: #000000;">glue:UsageProfile</code>&nbsp;as the key and the profile name as the value.</li> 
 <li>Next, sign in to the enterprise application that you created in the previous step, which has SCIM and SAML connections set up to IAM Identity Center: 
  <ol type="a"> 
   <li>Sign in to <a href="https://portal.azure.com/" rel="noopener noreferrer" target="_blank">Azure</a>.</li> 
   <li>Choose <strong>Enterprise applications</strong>.</li> 
   <li>Select the application that you created<br /> 
    <div class="wp-caption aligncenter" id="attachment_41903" style="width: 1339px;">
     <img alt="Figure 3 – An enterprise application created in Microsoft Entra ID" class="size-full wp-image-41903" height="602" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/23/Access-control-3.png" width="1329" />
     <p class="wp-caption-text" id="caption-attachment-41903">Figure 3 – An enterprise application created in Microsoft Entra ID</p>
    </div> </li> 
  </ol> </li> 
 <li>When you’re signed in to your application, select <strong>Manage </strong>and then <strong>Single sign-on</strong> in the navigation pane, then select <strong>Attributes &amp; Claims</strong>.<br /> 
  <div class="wp-caption aligncenter" id="attachment_41904" style="width: 1348px;">
   <img alt="Figure 4 – Attributes &amp; Claims section in Microsoft Entra ID" class="size-full wp-image-41904" height="568" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/23/Access-control-4.png" width="1338" />
   <p class="wp-caption-text" id="caption-attachment-41904">Figure 4 – Attributes &amp; Claims section in Microsoft Entra ID</p>
  </div> </li> 
 <li>Configure the key value pair that will used as session tags by selecting <strong>Add new claim</strong>.<br /> 
  <div class="wp-caption aligncenter" id="attachment_41905" style="width: 881px;">
   <img alt="Figure 5 – Configuring attributes by adding a new claim" class="size-full wp-image-41905" height="478" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/23/Access-control-5.png" width="871" />
   <p class="wp-caption-text" id="caption-attachment-41905">Figure 5 – Configuring attributes by adding a new claim</p>
  </div> </li> 
 <li>For&nbsp;<strong>Name</strong>, enter <code style="color: #000000;">AccessControl:<span style="color: #ff0000;"><em>&lt;AttributeName&gt;</em></span></code>. Replace <code><span style="color: #ff0000;"><em>&lt;AttributeName&gt;</em></span></code> with the name of the attribute you are expecting in IAM Identity Center. For this example, use <code style="color: #000000;">AccessControl:glue:UsageProfile</code>.</li> 
 <li>In <strong>Claim conditions</strong> set the following: 
  <ul> 
   <li><strong>User type</strong>, select <strong>Members</strong></li> 
   <li><strong>Source</strong>, select <strong>Attribute</strong>.</li> 
   <li><strong>Value</strong>, enter <code style="color: #000000;">developer</code> (without quotation marks). </li> 
  </ul> <p> </p>
  <div class="wp-caption aligncenter" id="attachment_41906" style="width: 936px;">
   <img alt="Figure 6 – Attribute claim addition in Microsoft Entra using group membership" class="size-full wp-image-41906" height="436" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/23/Access-control-6.png" width="926" />
   <p class="wp-caption-text" id="caption-attachment-41906">Figure 6 – Attribute claim addition in Microsoft Entra using group membership</p>
  </div> </li> 
</ol> 
<p>It’s important to note that the tags are being assigned based on group membership in Microsoft Entra ID. This approach lets you manage access and configuration dynamically without needing to set tags individually for each user. By assigning the tag to a Microsoft Entra ID group, anyone signing in to IAM Identity Center and who is in that group will automatically have the tag value applied to their session.</p> 
<h2>Test the solution</h2> 
<p>Now that the required configuration is complete, test the setup using the developer usage profile created as part of the <strong>Solution implementation</strong> section. Sign in as your user through Microsoft Entra ID using <a href="https://myapps.microsoft.com/" rel="noopener noreferrer" target="_blank">https://myapps.microsoft.com/</a> and verify the job creation using the following steps mentioned.</p> 
<p><strong>To verify successful job creation:</strong></p> 
<ol> 
 <li>Open the AWS Glue console using the developer usage profile.</li> 
 <li>In the navigation pane, choose <strong>ETL jobs</strong>.</li> 
 <li>Select <strong>Script editor</strong>, then choose <strong>Create script</strong>.</li> 
 <li>Create a new job using the values you want to validate.</li> 
</ol> 
<p>The green banner at the top of the screen should say <strong>Successfully updated job</strong>.</p> 
<div class="wp-caption aligncenter" id="attachment_41907" style="width: 688px;">
 <img alt="Figure 7 – Successful AWS Glue job creation with configured parameters for the &lt;em&gt;developer&lt;/em&gt; usage profile" class="size-full wp-image-41907" height="864" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/23/Access-control-7.png" width="678" />
 <p class="wp-caption-text" id="caption-attachment-41907">Figure 7 – Successful AWS Glue job creation with configured parameters for the <em>developer</em> usage profile</p>
</div> 
<h3>Validation using AWS CloudTrail</h3> 
<p>Examine the <code style="color: #000000;">AssumeRoleWithSAML</code> event using <a href="https://aws.amazon.com/cloudtrail/" rel="noopener" target="_blank">AWS Cloudtrail</a>. Use the following steps to verify the sequence of events.</p> 
<ol> 
 <li>Navigate to the CloudTrail console.</li> 
 <li>Select <strong>Event history</strong>.</li> 
 <li>In the <strong>Lookup attributes</strong> dropdown, select <strong>Event name</strong>.</li> 
 <li>Set the event name to <code style="color: #000000;">AssumeRoleWithSAML</code>.</li> 
 <li>Open a relevant event and inspect the <code style="color: #000000;">requestParameters</code> section.</li> 
 <li>Confirm that the expected session tags appear under <code style="color: #000000;">PrincipalTags</code>.</li> 
</ol> 
<div class="wp-caption aligncenter" id="attachment_41960" style="width: 1332px;">
 <img alt="Figure 8 – ABAC tags passed during the role assumption" class="size-full wp-image-41960" height="583" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/28/Access-control-8b.png" width="1322" />
 <p class="wp-caption-text" id="caption-attachment-41960">Figure 8 – ABAC tags passed during the role assumption</p>
</div> 
<h2>Using session tags for other use cases</h2> 
<p>The concepts discussed in this post can be extended to configure <a href="https://aws.amazon.com/systems-manager/" rel="noopener noreferrer" target="_blank">AWS Systems Manager</a> <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html" rel="noopener noreferrer" target="_blank">Session Manager</a> <a href="https://docs.aws.amazon.com/systems-manager/latest/userguide/session-preferences-run-as.html" rel="noopener noreferrer" target="_blank">Run As</a> support for federated users using session tags. By default, Session Manager launches sessions using a system-generated <code style="color: #000000;">ssm-user</code> account. For Linux instances, you can optionally configure sessions to run as a specific OS-level user through Session Manager preferences. You can configure your identity provider to pass the user attribute (<code style="color: #000000;">AccessControl: SSMSessionRunAs</code>&nbsp;and name of an OS user account for the key value during federation and the session will be tagged using the attribute value.</p> 
<h2>Clean up</h2> 
<p>To avoid incurring future charges, delete any resources created during this walkthrough if they’re no longer needed:</p> 
<ol> 
 <li>Remove the IAM Identity Center instance and clean up the associated enterprise application in Microsoft Entra.</li> 
 <li>Delete the AWS Glue usage profile.</li> 
 <li>Remove any other AWS resources you provisioned for testing the solution.</li> 
</ol> 
<h2>Conclusion</h2> 
<p>In this post, you learned how to federate access to AWS using AWS IAM Identity Center and SAML 2.0 identity providers like Microsoft Entra ID, enabling a secure, scalable, and centralized approach to managing user access across multiple AWS accounts. By using permission sets, reserved IAM roles, and session tags, organizations can implement fine-grained ABAC without the complexity of managing individual IAM users or static roles.</p> 
<p>As cloud environments become more complex, adopting modern identity federation and ABAC through IAM Identity Center helps security teams maintain control while providing users with seamless, context-aware access to the resources they need.</p> 
<h2>Resources</h2> 
<ul> 
 <li><a href="https://aws.amazon.com/blogs/security/how-to-use-the-passrole-permission-with-iam-roles/" rel="noopener noreferrer" target="_blank">How to use the PassRole permission with IAM roles</a></li> 
 <li><a href="https://docs.aws.amazon.com/singlesignon/latest/userguide/permissionsetcustom.html#permissionsetscmpconcept" rel="noopener noreferrer" target="_blank">Customer managed policies</a></li> 
 <li><a href="https://aws.amazon.com/blogs/mt/configuring-aws-systems-manager-session-manager-support-federated-users-using-session-tags/" rel="noopener noreferrer" target="_blank">Configuring AWS Systems Manager Session Manager run as support for federated users using session tags</a></li> 
 <li><a href="https://learn.microsoft.com/en-us/entra/identity-platform/saml-claims-customization#claims-transformation" rel="noopener noreferrer" target="_blank">Customize SAML token claims</a></li> 
</ul> 
<p>If you have feedback about this post, submit comments in the <strong>Comments</strong> section below.</p> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image"> 
   <img alt="Rashmi Iyer" class="aligncenter size-full wp-image-32817" height="160" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2023/12/21/iyerrash.png" width="120" /> 
  </div> 
  <h3 class="lb-h4">Rashmi Iyer</h3> 
  <p>Rashmi&nbsp;is a Senior Solutions Architect at AWS, supporting financial services enterprises in building secure, resilient, and scalable cloud architectures while ensuring compliance with industry best practices. With over 15 years of experience in the private telco cloud, she has designed and architected complex telecom solutions, specializing in the packet core domain,&nbsp;the backbone of mobile data networks.</p> 
  <p></p>
 </div> 
</footer>
