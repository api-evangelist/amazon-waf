---
title: "Can I do that with policy? Understanding the AWS Service Authorization Reference"
url: "https://aws.amazon.com/blogs/security/can-i-do-that-with-policy-understanding-the-aws-service-authorization-reference/"
date: "Mon, 27 Apr 2026 16:01:46 +0000"
author: "Anshu Bathla"
feed_url: "https://aws.amazon.com/blogs/security/feed/"
---
<div class="Page-articleBody"> 
 <div class="RichTextArticleBody RichTextBody"> 
  <p>Understanding what <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/iam/" rel="noopener" target="_blank">AWS Identity and Access Management (IAM)</a></span> policies can control helps you build better security controls and avoid spending time on approaches that won’t work. You’ve likely encountered questions like:</p> 
  <ul class="rte2-style-ul" id="rte-31cbfb80-3cf2-11f1-a752-e7f762a62896"> 
   <li>Can I use AWS Organizations service control policies (SCPs) to prevent the creation of security groups that allow traffic from <code class="CodeInline" style="color: #000;">0.0.0.0/0</code>?</li> 
   <li>Can I block uploads unless objects are encrypted?</li> 
   <li>Can I prevent functions with more than 512 MB of memory allocated?</li> 
  </ul> 
  <p>Some of these are possible with IAM policies. Others are not. The difference is determined by a fundamental principle of AWS authorization: <i>Policies make decisions based on information available in the authorization context at the time of the API call.</i></p> 
  <p>In this blog post, you learn how to use the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html" rel="noopener" target="_blank">AWS Service Authorization Reference</a></span> to determine what’s achievable with IAM policies, recognize scenarios that need alternative solutions, and build more effective security controls in your AWS environment.</p> 
  <div class="RichTextHeading"> 
   <h2>Understanding AWS authorization context</h2> 
   <p></p>
  </div> 
  <p>When you make an AWS API request through the AWS Management Console, <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/cli" rel="noopener" target="_blank">AWS Command Line Interface (AWS CLI)</a></span>, or AWS SDK, the specific AWS service (such as Amazon S3 or Amazon EC2) receiving the request assembles a <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic_policy-eval-reqcontext.html" rel="noopener" target="_blank">request context</a></span> containing information about that request. This context is used for policy evaluation decisions. Request context is structured using the Principal, Action, Resource, Condition (PARC) model, which has four key components.</p> 
  <ul class="rte2-style-ul" id="rte-31cbfb81-3cf2-11f1-a752-e7f762a62896"> 
   <li><b>Principal</b>: Identifies the requester and their attributes (tags, session context)</li> 
   <li><b>Action</b>: Specifies the AWS API operation being requested (for example, <code class="CodeInline" style="color: #000;">s3:PutObject, ec2:RunInstances</code>)</li> 
   <li><b>Resource:</b> Defines the target AWS resource using Amazon Resource Names (ARNs)</li> 
   <li><b>Condition:</b> Provides additional context available at request time, such as IP address, time, encryption parameters, MFA status, and service-specific attributes</li> 
  </ul> 
  <p>The following example shows the typical request context for an Amazon S3 object upload:</p> 
  <ul class="rte2-style-ul" id="rte-31cbfb82-3cf2-11f1-a752-e7f762a62896"> 
   <li><b>Principal</b>: <code class="CodeInline" style="color: #000;">AIDA123456789EXAMPLE</code></li> 
   <li><b>Action</b>: <code class="CodeInline" style="color: #000;">s3:PutObject</code></li> 
   <li><b>Resource</b>: <code class="CodeInline" style="color: #000;">arn:aws:s3:::my-bucket/documents/samplereport.pdf</code></li> 
   <li><b>Condition</b>: 
    <ul class="rte2-style-ul" id="rte-31cbfb83-3cf2-11f1-a752-e7f762a62896"> 
     <li><code class="CodeInline" style="color: #000;">aws:PrincipalTag/Department=Finance</code></li> 
     <li><code class="CodeInline" style="color: #000;">aws:RequestedRegion=us-east-1</code></li> 
     <li><code class="CodeInline" style="color: #000;">aws:SourceIp=x.x.x.x</code></li> 
     <li><code class="CodeInline" style="color: #000;">aws:MultiFactorAuthPresent=true</code></li> 
     <li><code class="CodeInline" style="color: #000;">s3:x-amz-server-side-encryption=AES256</code></li> 
     <li><code class="CodeInline" style="color: #000;">s3:x-amz-storage-class=STANDARD_IA</code></li> 
    </ul> </li> 
  </ul> 
  <p>IAM policies can evaluate request metadata like encryption method and storage class being specified. However, it cannot evaluate the actual file contents, object size, or specific data patterns. Policy evaluation occurs at the time of the request, using the information present in the authorization context.</p> 
  <div class="RichTextHeading"> 
   <h2>An essential resource: The Service Authorization Reference</h2> 
   <p></p>
  </div> 
  <p>The <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html" rel="noopener" target="_blank">Service Authorization Reference</a></span> is the authoritative documentation for understanding what policies can control. For every AWS service, it documents:</p> 
  <ul class="rte2-style-ul" id="rte-31cbfb84-3cf2-11f1-a752-e7f762a62896"> 
   <li><b>Actions</b>: Every controllable operation</li> 
   <li><b>Resources</b>: Resource types that can be targeted</li> 
   <li><b>Condition keys</b>: The exact context information available for policy decisions</li> 
  </ul> 
  <p>Condition keys are broadly divided into two categories. <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html" rel="noopener" target="_blank">Global condition keys</a></span>, which can be used across AWS services, and service-specific condition keys, which are defined for use with an individual AWS service. Use the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html" rel="noopener" target="_blank">Service Authorization Reference</a></span> to find the global-condition keys or service-specific condition keys for each AWS service.</p> 
  <div class="RichTextHeading"> 
   <h2>How to use the Service Authorization Reference</h2> 
   <p></p>
  </div> 
  <p>Follow these steps to determine if your requirement can be controlled with IAM policies:</p> 
  <ol class="rte2-style-ol" id="rte-31cc2290-3cf2-11f1-a752-e7f762a62896" start="1"> 
   <li><b>Navigate to your service</b>: Go to the page for the specific AWS service you’re working with, such as <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazons3.html" rel="noopener" target="_blank">Actions, resources, and condition keys for Amazon S3</a></span>.</li> 
   <li><b>Find the action you want</b>: Find the API operation you want to control. Be precise, different actions have different available condition keys.</li> 
   <li><b>Examine available condition keys</b>: The <b>Condition keys</b> column shows what context information AWS makes available for that action.</li> 
   <li><b>Make your feasibility determination</b>: If the information you need isn’t listed as a condition key, you will not be able to control it with IAM policies alone.</li> 
  </ol> 
  <p>Let’s take an example from the <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/ec2" rel="noopener" target="_blank">Amazon Elastic Compute Cloud (Amazon EC2)</a></span> <code class="CodeInline" style="color: #000;">ec2:RunInstances</code> action to see what you can and can’t control. In the Service Authorization Reference <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonec2.html" rel="noopener" target="_blank">under the Amazon EC2 section</a></span>, examine the <code class="CodeInline" style="color: #000;">RunInstances</code> action and check the <b>Resource types</b> column. The <code class="CodeInline" style="color: #000;">RunInstances</code> action affects multiple resource types, each with its own set of condition keys.</p> 
  <p>For the <code class="CodeInline" style="color: #000;">instance*</code> resource type:</p> 
  <ul class="rte2-style-ul" id="rte-31cc2291-3cf2-11f1-a752-e7f762a62896"> 
   <li><code class="CodeInline" style="color: #000;">ec2:InstanceType</code>: Can restrict instance types</li> 
   <li><code class="CodeInline" style="color: #000;">ec2:EbsOptimized</code>: Can require EBS optimization</li> 
   <li><code class="CodeInline" style="color: #000;">aws:RequestTag/</code>: Can enforce tagging requirements</li> 
  </ul> 
  <p>For the <code class="CodeInline" style="color: #000;">network-interface*</code> resource type:</p> 
  <ul class="rte2-style-ul" id="rte-31cc2292-3cf2-11f1-a752-e7f762a62896"> 
   <li><code class="CodeInline" style="color: #000;">ec2:Subnet</code>: Can control subnet placement</li> 
   <li><code class="CodeInline" style="color: #000;">ec2:Vpc</code>: Can limit to specific virtual private clouds (VPCs)</li> 
   <li><code class="CodeInline" style="color: #000;">ec2:AssociatePublicIpAddress</code>: Can control public IP assignment</li> 
  </ul> 
  <blockquote>
   <p> <b>Note:</b> These are a few examples from the many condition keys available for each resource type under the <code class="CodeInline" style="color: #000;">RunInstances</code> action. The Service Authorization Reference lists dozens of condition keys across resource types (instance, network interface, security group, subnet, volume, and so on) that <code class="CodeInline" style="color: #000;">RunInstances</code> affects. Consult the complete reference to see the available options for your specific use case.</p>
  </blockquote> 
  <div class="RichTextHeading"> 
   <h2>Access the Service Authorization Reference programmatically</h2> 
   <p></p>
  </div> 
  <p>Beyond the human-readable documentation, AWS provides the Service Authorization Reference in machine-readable JSON format to streamline automation of policy management workflows. Use this programmatic access to incorporate authorization metadata into your development and security workflows.<br /> For detailed information about the JSON structure and field definitions, see the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/service-reference.html" rel="noopener" target="_blank">Simplified AWS service information for programmatic access</a></span>.<br /> Developers can use tools like the <span class="LinkEnhancement"><a class="Link" href="https://awslabs.github.io/mcp/servers/iam-mcp-server" rel="noopener" target="_blank">IAM MCP Server</a></span> for AWS IAM operations. This server provides AI assistants with the ability to manage IAM users, roles, policies, and permissions while following security best practices.</p> 
  <div class="RichTextHeading"> 
   <h2>Using IAM policies to control specific scenarios</h2> 
   <p></p>
  </div> 
  <p>The following examples show how you can use IAM policies to control specific scenarios.</p> 
  <div class="RichTextHeading"> 
   <h3>Example 1: Enforce AES256 server-side encryption on S3 objects</h3> 
   <p></p>
  </div> 
  <p>In the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazons3.html" rel="noopener" target="_blank">Amazon S3 Service Authorization Reference</a></span>, under <code class="CodeInline" style="color: #000;">s3:PutObject</code> action, the <code class="CodeInline" style="color: #000;">s3:x-amz-server-side-encryption</code> condition key is available in the authorization context, which can be used to control the server-side encryption of S3 objects with AES-256. Here is the required policy.</p> 
  <p><b>Policy 1</b>: Deny Amazon S3 object upload if the encryption doesn’t use AES-256</p> 
  <div class="Enhancement"> 
   <div class="Enhancement-item"> 
    <div class="CodeBlockWP hide-language"> 
     <div class="code-toolbar"> 
      <pre class="unlimited-height-code language-text"><code class="language-text">{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "DenyUnencryptedObjectUploads",
			"Effect": "Deny",
			"Action": "s3:PutObject",
			"Resource": "arn:aws:s3:::my-bucket/*",
			"Condition": {
				"StringNotEquals": {
					"s3:x-amz-server-side-encryption": "AES256"
				}
			}
		}
	]
}</code></pre> 
      <p></p>
     </div> 
     <p></p>
    </div> 
    <p></p>
   </div> 
   <p></p>
  </div> 
  <p>Policy 1 is a resource-based policy that can be applied on an S3 bucket to restrict object uploads. It denies a <code class="CodeInline" style="color: #000;">PutObject</code> request when the server-side encryption isn’t using the AES-256 encryption algorithm.</p> 
  <div class="RichTextHeading"> 
   <h3>Example 2: Allow different instance types based on the user’s cost center tag.</h3> 
   <p></p>
  </div> 
  <p>When checking the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonec2.html" rel="noopener" target="_blank">Amazon EC2 Service Authorization Reference</a></span> for <code class="CodeInline" style="color: #000;">ec2:RunInstances</code>, the <code class="CodeInline" style="color: #000;">ec2:InstanceType</code> condition key, which is resource specific, is available. To restrict instance types based on who is launching them (rather than just what is being launched), you can either combine this with a global condition key or attach different policies to different principals. By using <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-principaltag" rel="noopener" target="_blank">aws:PrincipalTag/tag-key</a></span> alongside <code class="CodeInline" style="color: #000;">ec2:InstanceType</code>, you can identify the user’s cost center from their IAM identity tags and then apply different instance type restrictions accordingly. This allows a single policy to dynamically enforce different permissions based on the requester’s identity.</p> 
  <p><b>Policy 2</b>: Restricting EC2 instance types by cost center</p> 
  <div class="Enhancement"> 
   <div class="Enhancement-item"> 
    <div class="CodeBlockWP hide-language"> 
     <div class="code-toolbar"> 
      <pre class="unlimited-height-code language-text"><code class="language-text">{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "AllowDevInstanceTypes",
			"Effect": "Allow",
			"Action": "ec2:RunInstances",
			"Resource": "arn:aws:ec2:*:*:instance/*",
			"Condition": {
				"StringEquals": {
					"aws:PrincipalTag/CostCenter": "Development"
				},
				"StringLike": {
					"ec2:InstanceType": "t3.*"
				}
			}
		},
		{
			"Sid": "AllowProdInstanceTypes",
			"Effect": "Allow",
			"Action": "ec2:RunInstances",
			"Resource": "arn:aws:ec2:*:*:instance/*",
			"Condition": {
				"StringEquals": {
					"aws:PrincipalTag/CostCenter": "Production"
				},
				"StringLike": {
					"ec2:InstanceType": [
						"m5.*",
						"c5.*",
						"r5.*"
					]
				}
			}
		}
	]
}</code></pre> 
      <p></p>
     </div> 
     <p></p>
    </div> 
    <p></p>
   </div> 
   <p></p>
  </div> 
  <p>This is an <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html" rel="noopener" target="_blank">identity-based policy</a></span> that you can attach to IAM users, groups, or roles to control EC2 instance launches based on cost allocation. In the first statement, <code class="CodeInline" style="color: #000;">aws:PrincipalTag</code>, which is a global condition key (tags attached to the IAM user or role), is used to determine which instance types are allowed. Users tagged with <code class="CodeInline" style="color: #000;">CostCenter=Development</code> can only launch cost-effective T3 instance types (t3.micro, t3.small, t3.medium, and so on)with the service specific key <code class="CodeInline" style="color: #000;">ec2:InstanceType</code>.</p> 
  <p> In the second statement, users tagged with <code class="CodeInline" style="color: #000;">CostCenter=Production</code> can launch more powerful instance types from the M5 (general purpose), C5 (compute optimized), and R5 (memory optimized) families. This approach lets organizations enforce cost controls and allocate resources based on workload requirements. Each cost center maintains flexibility for its specific needs.</p> 
  <blockquote>
   <p><strong>Note</strong>: Additional resources are required in the IAM policy to successfully launch EC2 instances. For the complete list, see <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ExamplePolicies_EC2.html#iam-example-runinstances" rel="noopener" target="_blank">Launch Instances</a></span>.</p>
  </blockquote> 
  <div class="RichTextHeading"> 
   <h3>Example 3: Users can only access and update DynamoDB items where the partition key matches their username.</h3> 
   <p></p>
  </div> 
  <p>You have identified that <code class="CodeInline" style="color: #000;">GetItem</code>, <code class="CodeInline" style="color: #000;">PutItem</code>,and <code class="CodeInline" style="color: #000;">UpdateItem</code> actions are required. Corresponding to these actions, you can use the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/specifying-conditions.html#FGAC_DDB.ConditionKeys" rel="noopener" target="_blank"></a></span> condition key to expose partition key values in the authorization context as described in the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazondynamodb.html" rel="noopener" target="_blank">Amazon DynamoDB Service Authorization Reference</a></span></p> 
  <p><b>Policy 3</b>: DynamoDB fine-grained access control</p> 
  <div class="Enhancement"> 
   <div class="Enhancement-item"> 
    <div class="CodeBlockWP hide-language"> 
     <div class="code-toolbar"> 
      <pre class="unlimited-height-code language-text"><code class="language-text">{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"dynamodb:GetItem",
				"dynamodb:PutItem",
				"dynamodb:UpdateItem"
			],
			"Resource": "arn:aws:dynamodb:us-east-1:111122223333:table/UserProfiles",
			"Condition": {
				"ForAllValues:StringEquals": {
					"dynamodb:LeadingKeys": ["${aws:username}"]
				}
			}
		}
	]
}</code></pre> 
      <p></p>
     </div> 
     <p></p>
    </div> 
    <p></p>
   </div> 
   <p></p>
  </div> 
  <p></p> 
  <p>The policy allows users to perform read and write actions (<code class="CodeInline" style="color: #000;">GetItem</code>, <code class="CodeInline" style="color: #000;">PutItem</code>, and <code class="CodeInline" style="color: #000;">UpdateItem</code>) on the <code class="CodeInline" style="color: #000;">UserProfiles</code> table, but only for items where the partition key value equals their own username (using the <code class="CodeInline" style="color: #000;">${aws:username}</code> policy variable). For example, if user <code class="CodeInline" style="color: #000;">alice</code> attempts to access an item with partition key <code class="CodeInline" style="color: #000;">bob</code>, the request will be denied.</p> 
  <div class="RichTextHeading"> 
   <h2>Scenarios that need more than policies alone</h2> 
   <p></p>
  </div> 
  <p>Some requirements can’t be met using IAM policies. Here are three common scenarios that aren’t achievable with IAM policies alone.</p> 
  <div class="RichTextHeading"> 
   <h3>Scenario 1: Block users from creating security group rules that allow traffic from 0.0.0.0/0 on TCP port 22</h3> 
   <p></p>
  </div> 
  <p>Upon checking the Amazon EC2 Service Authorization Reference, you will find that the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonec2.html#amazonec2-actions-as-permissions" rel="noopener" target="_blank">ec2:AuthorizeSecurityGroupIngress</a></span> action is required in an IAM policy to add an inbound access rules to a security group.</p> 
  <p>To verify this in the Service Authorization Reference, navigate to the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonec2.html" rel="noopener" target="_blank">Amazon EC2 Service Authorization Reference</a></span> and search for the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_AuthorizeSecurityGroupIngress.html" rel="noopener" target="_blank">AuthorizeSecurityGroupIngress</a></span> action, which is the action that creates security group rules. After you locate this action, review the <b>Condition keys</b> column and look for condition keys related to CIDR blocks, IP ranges, ports, or protocols. Available condition keys for <code class="CodeInline" style="color: #000;">ec2:AuthorizeSecurityGroupIngress</code> include:</p> 
  <ul class="rte2-style-ul" id="rte-31cc2293-3cf2-11f1-a752-e7f762a62896"> 
   <li><code><span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonec2.html#amazonec2-aws_ResourceTag___TagKey_" rel="noopener" target="_blank">aws:ResourceTag/${TagKey}</a></span></code>: Filters access by a tag key and value pair of a resource</li> 
   <li><code><span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonec2.html#amazonec2-ec2_ResourceTag___TagKey_" rel="noopener" target="_blank">ec2:ResourceTag/${TagKey}</a></span></code>: Filters access by a tag key and value pair of a resource</li> 
   <li><code><span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonec2.html#amazonec2-ec2_SecurityGroupID" rel="noopener" target="_blank">ec2:SecurityGroupID</a></span></code>: Filters access by the ID of a security group</li> 
   <li><code><span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonec2.html#amazonec2-ec2_Vpc" rel="noopener" target="_blank">ec2:Vpc</a></span></code>: Filters access by the ARN of the VPC</li> 
  </ul> 
  <p>Notice there are no condition keys for CIDR blocks (such as <code class="CodeInline" style="color: #000;">0.0.0.0/0</code>), port numbers (such as <code class="CodeInline" style="color: #000;">22</code>), or protocols (such as TCP). The authorization context doesn’t include information about the specific CIDR blocks, ports, or protocols being added to the security group rule, so IAM policies can’t control these attributes.</p> 
  <p><b>Solution</b><br /> Take a reactive approach using the <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/config/" rel="noopener" target="_blank">AWS Config</a></span> managed rule <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/config/latest/developerguide/restricted-ssh.html" rel="noopener" target="_blank">INCOMING_SSH_DISABLED</a></span> to detect overly permissive rules. You can also use a combination of <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/eventbridge/" rel="noopener" target="_blank">Amazon EventBridge</a></span> and Lambda to either send a notification to your security team for the non-compliant configuration or to restrict the security group through an automation. For more information, see <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/security/how-to-automatically-revert-and-receive-notifications-about-changes-to-your-amazon-vpc-security-groups/" rel="noopener" target="_blank">How to Automatically Revert and Receive Notifications About Changes to Your Amazon VPC Security Groups</a></span>.</p> 
  <div class="RichTextHeading"> 
   <h3>Scenario 2: Prevent creation of Lambda functions with more than 512 MB of memory allocated</h3> 
   <p></p>
  </div> 
  <p>Following the same verification methodology described in Scenario 1, navigate to the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/service-authorization/latest/reference/list_awslambda.html" rel="noopener" target="_blank">AWS Lambda Service Authorization Reference</a></span> and examine the <code class="CodeInline" style="color: #000;">CreateFunction</code> action’s condition keys for the <code class="CodeInline" style="color: #000;">function*</code> resource type.</p> 
  <p>Available condition keys for lambda:<code class="CodeInline" style="color: #000;">CreateFunction</code> with the <code class="CodeInline" style="color: #000;">function*</code> resource type include:</p> 
  <ul class="rte2-style-ul" id="rte-31cc2294-3cf2-11f1-a752-e7f762a62896"> 
   <li><code class="CodeInline" style="color: #000;">lambda:CodeSigningConfigArn</code>: Filters access by the ARN of the code signing</li> 
   <li><code class="CodeInline" style="color: #000;">configuration-lambda:Layer</code>: Filters access by the ARN of a version of an AWS Lambda layer</li> 
   <li><code class="CodeInline" style="color: #000;">lambda:VpcIds</code>: Filters access by the ID of the VPC configured for the Lambda function</li> 
  </ul> 
  <p>There is no condition key for memory allocation (<code class="CodeInline" style="color: #000;">MemorySize</code> parameter), timeout settings, storage configuration (<code class="CodeInline" style="color: #000;">EphemeralStorage</code>), or runtime selection. Because memory allocation isn’t exposed in the authorization context, IAM policies can’t restrict this parameter.</p> 
  <p><b>Solution</b></p> 
  <ul class="rte2-style-ul" id="rte-31cc2295-3cf2-11f1-a752-e7f762a62896"> 
   <li>Use a policy-as-code (PaC) approach with <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/cloudformation-cli/latest/hooks-userguide/what-is-cloudformation-hooks.html" rel="noopener" target="_blank">AWS CloudFormation Hooks</a></span>. For more information, see <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/infrastructure-and-automation/a-practical-guide-to-getting-started-with-policy-as-code/" rel="noopener" target="_blank">A practical guide to getting started with policy as code</a></span>.</li> 
   <li>Use <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/servicecatalog/latest/adminguide/introduction.html" rel="noopener" target="_blank">AWS Service Catalog</a></span> to provide pre-approved Lambda configurations.</li> 
   <li>Use <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/config/latest/developerguide/lambda-function-settings-check.html" rel="noopener" target="_blank">LAMBDA_FUNCTION_SETTINGS_CHECK</a></span> to apply an AWS Config rule to detect non-compliant functions.</li> 
  </ul> 
  <div class="RichTextHeading"> 
   <h2>Key takeaways</h2> 
   <p></p>
  </div> 
  <p>Keep these principles in mind when working with IAM policies:</p> 
  <ul class="rte2-style-ul" id="rte-31cc49a0-3cf2-11f1-a752-e7f762a62896"> 
   <li>Policies control what’s in the authorization context, not all elements you see in API documentation</li> 
   <li>The Service Authorization Reference is authoritative; if something isn’t listed as a condition key, you can’t control it with policies</li> 
   <li>Different actions have different available contexts even within the same service</li> 
   <li>Alternative approaches exist. AWS Config, EventBridge, and service-specific controls can be used to achieve your goals when policies alone can’t</li> 
   <li>Layered security is essential; combine preventive, detective, and responsive controls to help ensure that your data is secure</li> 
  </ul> 
  <div class="RichTextHeading"> 
   <h2>Conclusion</h2> 
   <p></p>
  </div> 
  <p>In this post, you learned how to use the AWS Service Authorization Reference to determine what’s achievable with IAM policies and recognize scenarios that require alternative solutions. By understanding that policies can only make decisions based on information available in the authorization context, you can build more effective security controls and avoid spending time on approaches that won’t work. </p> 
  <p>The Service Authorization Reference is your authoritative source for understanding policy capabilities. When you need to implement a control, start there to see if the required condition keys exist. If they don’t, you will need to layer in detective or responsive controls using services like AWS Config, Amazon EventBridge, or AWS Lambda. </p> 
  <p>Remember that effective AWS security isn’t about finding one perfect control, it’s about combining preventive, detective, and responsive measures to create defense in depth. IAM policies are powerful tools for prevention and work as part of a comprehensive security strategy.</p> 
  <p><b>Next steps:</b></p> 
  <ul class="rte2-style-ul" id="rte-31cc49a1-3cf2-11f1-a752-e7f762a62896"> 
   <li>Explore <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html" rel="noopener" target="_blank">IAM policy simulator</a> to test and troubleshoot your policies</li> 
   <li>Read <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/security/iam-policy-types-how-and-when-to-use-them" rel="noopener" target="_blank">IAM policy types: How and when to use them</a></span> to understand when to use different policy types</li> 
   <li>Watch the video on <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/video/watch/625f28e7a2e/" rel="noopener" target="_blank">Understanding AWS IAM Policy Language: Elements, Evaluation, and Best Practices</a></span></li> 
  </ul> 
  <p>If you have feedback about this post, submit comments in the Comments section below.</p> 
  <hr />
 </div> 
</div> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image"> 
   <img alt="Author" class="aligncenter size-full wp-image-22385" height="160" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2025/01/09/Anshu-Bathla.jpg" width="120" /> 
  </div> 
  <h3 class="lb-h4">Anshu Bathla</h3> 
  <p>Anshu is a Senior Lead Consultant – SRC at AWS, based in Gurugram, India. He works with customers across diverse verticals to help strengthen their security infrastructure and achieve their security goals. Outside of work, Anshu enjoys reading books and gardening at his home garden.</p> 
  <p></p> 
 </div> 
 <div class="blog-author-box"> 
  <div class="blog-author-image"> 
   <img alt="Author" class="aligncenter size-full wp-image-22385" height="160" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/20/Prafful-Gupta-author.jpg" width="120" /> 
  </div> 
  <h3 class="lb-h4">Prafful Gupta</h3> 
  <p>Prafful is an Associate Delivery Consultant at AWS, based in Gurugram, India. Having started his professional journey with Amazon, he specializes in DevOps and Generative AI solutions, helping customers navigate their cloud transformation journeys. Beyond work, he enjoys networking with fellow professionals and spending quality time with family.</p> 
  <p></p> 
 </div> 
</footer>
