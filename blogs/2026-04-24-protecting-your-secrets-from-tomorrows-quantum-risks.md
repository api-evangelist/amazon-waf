---
title: "Protecting your secrets from tomorrow’s quantum risks"
url: "https://aws.amazon.com/blogs/security/protecting-your-secrets-from-tomorrows-quantum-risks/"
date: "Fri, 24 Apr 2026 18:53:29 +0000"
author: "Stéphanie Mbappe"
feed_url: "https://aws.amazon.com/blogs/security/feed/"
---
<div class="Page-articleBody"> 
 <div class="RichTextArticleBody RichTextBody"> 
  <p>As outlined in the AWS post-quantum cryptography (PQC) <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/security/aws-post-quantum-cryptography-migration-plan/" rel="noopener" target="_blank">migration plan</a></span>, addressing the risk of <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/security/post-quantum-cryptography/migrating-to-post-quantum-cryptography/" rel="noopener" target="_blank">harvest now, decrypt later</a></span> (HNDL) attack is an important part of your post-quantum plan. Upgrading the client-side of your workloads to support quantum-resistant confidentiality is an important aspect of your side of the <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/security/post-quantum-cryptography/migrating-to-post-quantum-cryptography/" rel="noopener" target="_blank">PQC shared responsibility model</a></span>. Timelines to plan and execute your PQC upgrades vary by region and by industry and will depend on your own business risk profile. To learn more, see the AWS <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/security/post-quantum-cryptography/migrating-to-post-quantum-cryptography/#general--tf3apt" rel="noopener" target="_blank">PQC frequently asked questions</a></span>.</p> 
  <p><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/secrets-manager/" rel="noopener" target="_blank">AWS Secrets Manager</a></span> uses SSL/TLS to communicate with AWS resources, currently supporting TLS 1.2 and 1.3 in all AWS Regions. The service supports using TLS 1.3 with hybrid post-quantum key exchange for clients that support this capability. The <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/payment-cryptography/latest/userguide/pqtls.html" rel="noopener" target="_blank">hybrid post-quantum</a></span> approach establishes TLS connections by combining traditional cryptography (such as X25519) with post-quantum algorithms (ML-KEM), and helps to protect your secrets against both current classical attacks and future quantum computer threats. Regardless of how your workload accesses Secrets Manager, this client-side software upgrade is the only action you need to take to address risk to secrets from HNDL. Your secrets at rest are already encrypted using keys managed by <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/kms" rel="noopener" target="_blank">AWS Key Management Service (AWS KMS)</a></span>. Properly implemented symmetric encryption is considered quantum-resistant; asymmetric cryptography faces quantum threats. To learn more, watch <span class="LinkEnhancement"><a class="Link" href="https://www.youtube.com/watch?v=SG9ndQWH8S4" rel="noopener" target="_blank">AWS re:Inforce 2025 – Post-Quantum Cryptography Demystified</a></span>.</p> 
  <p>To reduce builder effort for client-side upgrades, we’re pleased to announce the following <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/secrets-manager/" rel="noopener" target="_blank">Secrets Manager</a></span> clients now enable and prefer post-quantum TLS when initiating connections to Secrets Manager: <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/secretsmanager/latest/userguide/secrets-manager-agent.html" rel="noopener" target="_blank">Secrets Manager Agent</a></span> (<span class="LinkEnhancement"><a class="Link" href="https://github.com/aws/aws-secretsmanager-agent/releases/tag/v2.0.0" rel="noopener" target="_blank">v2.0.0</a></span> or later), the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/lambda/latest/dg/with-secrets-manager.html#lambda-secrets-manager-extension-approach" rel="noopener" target="_blank">AWS Lambda extension</a></span> (v19 or later) and the <span class="LinkEnhancement"><a class="Link" href="https://github.com/aws/secrets-store-csi-driver-provider-aws" rel="noopener" target="_blank">Secrets Manager CSI Driver</a></span> (<span class="LinkEnhancement"><a class="Link" href="https://github.com/aws/secrets-store-csi-driver-provider-aws/releases/tag/secrets-store-csi-driver-provider-aws-2.0.0" rel="noopener" target="_blank">v2.0.0</a></span> or later). For SDK-based clients, hybrid post-quantum key exchange is available in supported AWS SDKs. Enablement requirements vary by language, version, and operating system. See the following table for your SDK client.</p> 
  <p>This launch is part of the ongoing commitment AWS has made to migrate systems to post-quantum cryptography and making it straightforward for our customers to do the same. See <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/security/post-quantum-cryptography/" rel="noopener" target="_blank">Post-Quantum Cryptography</a></span> to learn more.</p> 
  <div class="RichTextHeading"> 
   <h2>Client hybrid post-quantum key exchange requirements</h2> 
  </div> 
  <p>The following table summarizes the behavior for each client. When the client is upgraded to support hybrid post-quantum key exchange, the Secrets Manager service endpoint automatically selects it during the TLS handshake. Upgrading to the versions listed in the table is the only action you need to take for your workload to begin using hybrid post-quantum key exchange when calling Secrets Manager APIs.</p> 
  <table border="1px" cellpadding="10px" style="border-collapse: separate; text-indent: initial; border-spacing: 2px; border-color: gray; width: 100%;"> 
   <tbody> 
    <tr> 
     <td style="padding: 10px; border: 1px solid #dddddd;"><b>Client</b></td> 
     <td style="padding: 10px; border: 1px solid #dddddd;"><b>Requirements</b></td> 
    </tr> 
    <tr> 
     <td style="padding: 10px; border: 1px solid #dddddd;"><span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/secretsmanager/latest/userguide/secrets-manager-agent.html" rel="noopener" target="_blank">Secrets Manager Agent</a></span></td> 
     <td style="padding: 10px; border: 1px solid #dddddd;">Hybrid PQ key exchange in TLS preferred by default (<a href="https://github.com/aws/aws-secretsmanager-agent/releases/tag/v2.0.0" rel="noopener" target="_blank">v2.0.0 and later</a>)</td> 
    </tr> 
    <tr> 
     <td style="padding: 10px; border: 1px solid #dddddd;"><span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/lambda/latest/dg/with-secrets-manager.html" rel="noopener" target="_blank">AWS Lambda extension</a></span></td> 
     <td style="padding: 10px; border: 1px solid #dddddd;">Hybrid PQ key exchange in TLS preferred by default (Version 19 and later)</td> 
    </tr> 
    <tr> 
     <td style="padding: 10px; border: 1px solid #dddddd;"><span class="LinkEnhancement"><a class="Link" href="https://github.com/aws/secrets-store-csi-driver-provider-aws" rel="noopener" target="_blank">Secrets Manager CSI Driver</a></span></td> 
     <td style="padding: 10px; border: 1px solid #dddddd;">Hybrid PQ key exchange in TLS preferred by default (<span class="LinkEnhancement"><a class="Link" href="https://github.com/aws/secrets-store-csi-driver-provider-aws/releases/tag/secrets-store-csi-driver-provider-aws-2.0.0" rel="noopener" target="_blank">v2.0.0</a></span> and later)</td> 
    </tr> 
    <tr> 
     <td style="padding: 10px; border: 1px solid #dddddd;"><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/sdk-for-rust/" rel="noopener" target="_blank">AWS SDK for Rust</a></span></td> 
     <td style="padding: 10px; border: 1px solid #dddddd;">Hybrid PQ key exchange in TLS preferred by default (<span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/payment-cryptography/latest/userguide/pqtls-details.html#pq-tls-open-ssl:~:text=underlying%20system%20libraries.-,AWS%20SDK%20for%20Rust,-The%20AWS%20SDK" rel="noopener" target="_blank">releases after August 29, 2025</a></span>)</td> 
    </tr> 
    <tr> 
     <td style="padding: 10px; border: 1px solid #dddddd;"><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/sdk-for-go/" rel="noopener" target="_blank">AWS SDK for Go</a></span></td> 
     <td style="padding: 10px; border: 1px solid #dddddd;">Hybrid PQ key exchange in TLS preferred by default (<span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/payment-cryptography/latest/userguide/pqtls-details.html#pq-tls-open-ssl:~:text=system%2Dlevel%20support.-,AWS%20SDK%20for%20Go,-The%20AWS%20SDK" rel="noopener" target="_blank">Go v1.24 and later</a></span>)</td> 
    </tr> 
    <tr> 
     <td style="padding: 10px; border: 1px solid #dddddd;"><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/sdk-for-javascript/" rel="noopener" target="_blank">AWS SDK for Node.js</a></span></td> 
     <td style="padding: 10px; border: 1px solid #dddddd;">Hybrid PQ key exchange in TLS preferred by default (<span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/payment-cryptography/latest/userguide/pqtls-details.html#pq-tls-open-ssl:~:text=140%20for%20Android.-,AWS%20SDK%20for%20Node.js,-As%20of%20Node" rel="noopener" target="_blank">Node.js v22.20 and v24.9.0 and later</a></span>)</td> 
    </tr> 
    <tr> 
     <td style="padding: 10px; border: 1px solid #dddddd;"><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/sdk-for-kotlin/" rel="noopener" target="_blank">AWS SDK for Kotlin</a></span></td> 
     <td style="padding: 10px; border: 1px solid #dddddd;">Hybrid PQ key exchange in TLS preferred by default on Linux <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/payment-cryptography/latest/userguide/pqtls-details.html#pq-tls-open-ssl:~:text=and%20subsequent%20versions.-,AWS%20SDK%20for%20Kotlin,-The%20Kotlin%20SDK" rel="noopener" target="_blank">(v1.5.78 and later)</a></span></td> 
    </tr> 
    <tr> 
     <td style="padding: 10px; border: 1px solid #dddddd;"><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/sdk-for-python/" rel="noopener" target="_blank">AWS SDK for Python</a></span></td> 
     <td style="padding: 10px; border: 1px solid #dddddd;">The AWS SDK for Python (boto3) uses the OS-provided OpenSSL for TLS.<br /> Hybrid PQ key exchange in TLS requires running on a system with <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/payment-cryptography/latest/userguide/pqtls-details.html#pq-tls-open-ssl:~:text=OpenSSL%203.5%20installed.-,AWS%20SDK%20for%20Python%20(Boto3),-The%20AWS%20SDK" rel="noopener" target="_blank">OpenSSL 3.5 or later installed</a></span>.</td> 
    </tr> 
    <tr> 
     <td style="padding: 10px; border: 1px solid #dddddd;"><span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/home.html" rel="noopener" target="_blank">AWS SDK for Java v2</a></span></td> 
     <td style="padding: 10px; border: 1px solid #dddddd;"><span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/payment-cryptography/latest/userguide/pqtls-details.html#pq-tls-open-ssl:~:text=ON%20%5C%0A%20%20%20%20%2DDUSE_OPENSSL%3DOFF%20%5C-,AWS%20SDK%20for%20Java,-As%20of%20v2" rel="noopener" target="_blank">AWS SDK for Java</a></span> v2 requires an AWS CRT HTTP client that supports PQ TLS when configured using <span class="LinkEnhancement"><a class="Link" href="https://sdk.amazonaws.com/java/api/latest/software/amazon/awssdk/http/crt/AwsCrtHttpClient.Builder.html#postQuantumTlsEnabled(java.lang.Boolean)" rel="noopener" target="_blank">postQuantumTlsEnabled</a></span>.</td> 
    </tr> 
    <tr> 
     <td style="padding: 10px; border: 1px solid #dddddd;"><span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/secretsmanager/latest/userguide/retrieving-secrets.html" rel="noopener" target="_blank">Secrets Manager caching clients</a></span></td> 
     <td style="padding: 10px; border: 1px solid #dddddd;">The Secrets Manager caching libraries are built on the AWS SDKs and inherit their TLS behavior. Note for Java: The <span class="LinkEnhancement"><a class="Link" href="https://github.com/aws/aws-secretsmanager-jdbc#:~:text=Enable%20Post%2DQuantum%20TLS%20(PQTLS)%20by%20setting%20the%20following%20in%20the%20secretsmanager.properties%20file%3A" rel="noopener" target="_blank">JDBC driver flag</a></span> and <span class="LinkEnhancement"><a class="Link" href="https://github.com/aws/aws-secretsmanager-caching-java?tab=readme-ov-file#enabling-post-quantum-tls" rel="noopener" target="_blank">Java Caching flag</a></span> must be set to enable Hybrid PQ key exchange in TLS.</td> 
    </tr> 
   </tbody> 
  </table> 
  <p>If you’re using the Secrets Manager Agent, the Lambda extension, or the CSI Driver, upgrade to the listed version to use hybrid post-quantum key exchange in TLS as the default. Customers using the AWS SDK for Rust, Go, or Node.js at the versions listed in the table are already upgraded and no additional action is required. The SDK will select the hybrid post-quantum key exchange for API calls. For customers using the AWS SDK for Python, hybrid post-quantum key exchange in TLS requires OpenSSL 3.5 or later to be present on the host system. Guidance on verifying and enabling this is available in the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/de_de/secretsmanager/latest/userguide/intro.html" rel="noopener" target="_blank">AWS Secrets Manager documentation</a></span>. For customers using the AWS SDK for Java v2, hybrid post-quantum key exchange in TLS requires using the AWS CRT HTTP client. The <span class="LinkEnhancement"><a class="Link" href="https://sdk.amazonaws.com/java/api/latest/software/amazon/awssdk/http/crt/AwsCrtHttpClient.Builder.html#postQuantumTlsEnabled(java.lang.Boolean)" rel="noopener" target="_blank">postQuantumTlsEnabled(true)</a></span> must be set on the CRT client to enable hybrid post-quantum key exchange in TLS.</p> 
  <p>After your client versions meet the requirements listed in the table, you can verify that your connections are actively using hybrid post-quantum key exchange.</p> 
  <div class="RichTextHeading"> 
   <h2>How to verify your connection uses hybrid post-quantum key exchange</h2> 
  </div> 
  <p>With hybrid post-quantum key exchange using ML-KEM now enabled by default for Secrets Manager clients (see the preceding table), most customers will not need ongoing monitoring to verify correct behavior or detect regressions. However, security teams and compliance officers might want to confirm that their Secrets Manager API calls are negotiating the hybrid key exchange. On the server side, you can confirm hybrid post-quantum key exchange in TLS by using <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html" rel="noopener" target="_blank">AWS CloudTrail</a></span>. On the client side, you can inspect TLS handshake details using a utility like Wireshark or by using developer tools built into major web browsers.</p> 
  <p>Verification is a two-step process: first, fetch a secret using your Secrets Manager client to generate a <code class="CodeInline" style="color: #000;">GetSecretValue</code> API call, then confirm in <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/cloudtrail" rel="noopener" target="_blank">AWS CloudTrail</a></span> that the call negotiated hybrid post-quantum key exchange.</p> 
  <div class="RichTextHeading"> 
   <h3>Fetch your secret using your Secrets Manager client</h3> 
  </div> 
  <p>The following examples show how to retrieve your secret using the Secrets Manager Agent, Lambda extension, and CSI Driver—each of which will automatically negotiate hybrid post-quantum key exchange when calling the <code class="CodeInline" style="color: #000;">GetSecretValue</code> API.</p> 
  <p><b>To verify hybrid post-quantum TLS with Secrets Manager Agent on EC2 instance:</b><br /> Install the agent on your <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/ec2" rel="noopener" target="_blank">Amazon Elastic Compute Cloud (Amazon EC2)</a></span> instance and use it as a client to fetch your secret.</p> 
  <ol class="rte2-style-ol" id="rte-1d7ba2b1-3835-11f1-bc8a-c599d2e4deff" start="1"> 
   <li>Follow the instructions for <span class="LinkEnhancement"><a class="Link" href="https://github.com/aws/aws-secretsmanager-agent?tab=readme-ov-file#aws-secrets-manager-agent" rel="noopener" target="_blank">AWS Secrets Manager Agent</a></span>.</li> 
   <li>Ensure that your EC2 instance profile has the permission for <code class="CodeInline" style="color: #000;">secretsmanager:GetSecretValue</code> to fetch the secret.</li> 
   <li>Connect to your <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect.html" rel="noopener" target="_blank">private EC2 instance</a></span>.</li> 
   <li>Install the agent on your EC2 instance.</li> 
   <li>Use the agent to <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/security/how-to-use-the-aws-secrets-manager-agent/#:~:text=user%20/tmp/awssmatoken-,Retrieve%20the%20secret,-Now%20you%20can" rel="noopener" target="_blank">fetch your secret</a></span>.<br /> <code class="CodeInline" style="color: #000;">curl -H “X-Aws-Parameters-Secrets-Token: $(&lt;/tmp/awssmatoken)” localhost:2773/secretsmanager/get?secretId=&lt;YOUR-SECRET-ARN&gt;</code></li> 
   <li>Wait for about 5 minutes for CloudTrail to deliver the logs.</li> 
   <li>Go to the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html" rel="noopener" target="_blank">CloudTrail event history</a></span> and search for the event <code class="CodeInline" style="color: #000;">GetSecretValue</code>.</li> 
  </ol> 
  <p><b>To verify hybrid post-quantum TLS with Lambda extension:</b><br /> Use the AWS parameters and Secrets Manager Lambda extension to create a Lambda function that will consume your secrets from Secrets Manager using direct API calls.</p> 
  <ol class="rte2-style-ol" id="rte-eb502f31-39c4-11f1-a717-1b983d02678f" start="1"> 
   <li>Follow <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/lambda/latest/dg/with-secrets-manager.html#lambda-secrets-manager-extension-approach" rel="noopener" target="_blank">Using the AWS parameters and secrets Lambda extension</a></span> to create the Lambda layer and the Lambda function.</li> 
   <li>Select the latest extension version.</li> 
   <li>Wait for about 5 minutes for CloudTrail to deliver the logs.</li> 
   <li>Go to the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html" rel="noopener" target="_blank">CloudTrail event history</a></span> and search for the event <code class="CodeInline" style="color: #000;">GetSecretValue</code>.</li> 
  </ol> 
  <p><b>To verify hybrid post-quantum TLS with CSI driver on Amazon EKS:</b><br /> On your <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/eks" rel="noopener" target="_blank">Amazon Elastic Kubernetes Service (Amazon EKS)</a></span> cluster, use the <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/security/how-to-use-the-secrets-store-csi-driver-provider-amazon-eks-add-on-with-secrets-manager/" rel="noopener" target="_blank">AWS Secrets Store CSI Driver provider to fetch secrets from Secrets Manager</a></span> in Kubernetes pods:</p> 
  <ol class="rte2-style-ol" id="rte-41b42991-383e-11f1-b7d0-73ebda3131ce" start="1"> 
   <li>Confirm the installed add-on version is 2.0.0 or later.<br /> <code class="CodeInline" style="color: #000;">eksctl get addon --cluster &lt;CLUSTER-NAME&gt; --name aws-secrets-store-csi-driver-provider</code></li> 
   <li>Trigger a secret retrieval by restarting a pod that mounts a secret, or deploying a new one.</li> 
   <li>Wait for about 5 minutes for CloudTrail to deliver the logs.</li> 
   <li>Go to the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html" rel="noopener" target="_blank">CloudTrail event history</a></span> and search for the event <code class="CodeInline" style="color: #000;">GetSecretValue</code>.</li> 
  </ol> 
  <div class="RichTextHeading"> 
   <h3><b>Confirm hybrid post-quantum key exchange using CloudTrail</b></h3> 
  </div> 
  <p><span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-record-contents.html" rel="noopener" target="_blank">CloudTrail logs</a></span> include a <code class="CodeInline" style="color: #000;">tlsDetails</code> field for Secrets Manager API calls. When hybrid post-quantum key exchange in TLS is active, the <code class="CodeInline" style="color: #000;">keyExchange</code> field in <code class="CodeInline" style="color: #000;">tlsDetails</code> will show <code class="CodeInline" style="color: #000;">X25519MLKEM768</code>. Each CloudTrail record includes a <code class="CodeInline" style="color: #000;">tlsDetails</code> field that contains the cipher suite and, where available, the key exchange group negotiated during the TLS handshake.</p> 
  <p>You can work with <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html" rel="noopener" target="_blank">CloudTrail event history</a></span> using the AWS Management Console for CloudTrail or the <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/cli" rel="noopener" target="_blank">AWS Command Line Interface (AWS CLI)</a></span>.</p> 
  <p><b>To look up CloudTrail events using the console:</b></p> 
  <ol class="rte2-style-ol" id="rte-1d7bc9b8-3835-11f1-bc8a-c599d2e4deff" start="1"> 
   <li>Verify you are in the correct AWS Region.</li> 
   <li>Open the CloudTrail console and select <b>Event History</b>.</li> 
   <li>Under <b>Lookup attributes</b> filter, select <b>Event name</b> and <b>GetSecretValue</b>.<br /> 
    <div class="wp-caption aligncenter" id="attachment_41893" style="width: 946px;">
     <img alt="Figure 1: Search CloudTrail event history by event name" class="size-full wp-image-41893" height="294" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/23/Figure1-5.png" width="936" />
     <p class="wp-caption-text" id="caption-attachment-41893">Figure 1: Search CloudTrail event history by event name</p>
    </div> </li> 
   <li>Select your event.<br /> 
    <div class="wp-caption aligncenter" id="attachment_41889" style="width: 946px;">
     <img alt="Figure 2: Select the event" class="size-full wp-image-41889" height="242" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/23/Figure2-2.png" width="936" />
     <p class="wp-caption-text" id="caption-attachment-41889">Figure 2: Select the event</p>
    </div> </li> 
   <li>View the output in the <b>Event Record</b> section of the page.<br /> 
    <div class="wp-caption aligncenter" id="attachment_41890" style="width: 883px;">
     <img alt="Figure 3: CloudTrail - GetSecretValue event" class="size-full wp-image-41890" height="500" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/23/Figure3-3.png" width="873" />
     <p class="wp-caption-text" id="caption-attachment-41890">Figure 3: CloudTrail – GetSecretValue event</p>
    </div> </li> 
  </ol> 
  <p><b>To look up CloudTrail events using AWS CLI :</b><br /> Using AWS CLI, select the last events and look at the output.</p> 
  <div class="Enhancement"> 
   <div class="Enhancement-item"> 
    <div class="CodeBlockWP hide-language"> 
     <div class="code-toolbar"> 
      <pre class="unlimited-height-code language-text"><code class="language-text">aws cloudtrail lookup-events \
--lookup-attributes AttributeKey=EventName,AttributeValue=GetSecretValue \
--max-results 5 \
--region &lt;YOUR-REGION&gt; \
--query 'Events[0].CloudTrailEvent' \
--output text</code></pre> 
     </div> 
    </div> 
   </div> 
  </div> 
  <p><b>Example of CloudTrail Event for GetSecretValue API call:</b></p> 
  <p>In the following example, the <code class="CodeInline" style="color: #000;">userAgent</code> field reflects what it used as a client to connect to Secrets Manager.</p> 
  <blockquote>
   <p><b>Note</b>: The <code class="CodeInline" style="color: #000;">userAgent</code> value depends on the client you use.</p>
  </blockquote> 
  <div class="Enhancement"> 
   <div class="Enhancement-item"> 
    <div class="CodeBlockWP hide-language"> 
     <div class="code-toolbar"> 
      <pre class="unlimited-height-code language-text"><code class="language-text">{
    "eventVersion": "1.11",
    "userIdentity": {
        "type": "AssumedRole",
        "principalId": "AROA123456789EXAMPLE:i-0c1a23fc456b7ab89",
        "arn": "arn:aws:sts::111122223333:assumed-role/YOUR-EC2-INSTANCE-PROFILE/i-0c1a23fc456b7ab89",
        "accountId": "111122223333",
        "accessKeyId": "ASIAIOSFODNN7EXAMPLE",
        "sessionContext": {
            "sessionIssuer": {
                "type": "Role",
                "principalId": "AROA123456789EXAMPLE",
                "arn": "arn:aws:iam::111122223333:role/YOUR-EC2-INSTANCE-PROFILE",
                "accountId": "111122223333",
                "userName": "YOUR-EC2-INSTANCE-PROFILE"
            },
            "attributes": {
                "creationDate": "2026-03-27T17:08:37Z",
                "mfaAuthenticated": "false"
            },
            "ec2RoleDelivery": "2.0"
        },
        "inScopeOf": {
            "issuerType": "AWS::EC2::Instance",
            "credentialsIssuedTo": "arn:aws:ec2:eu-west-2:111122223333:instance/i-0c1a23fc456b7ab89"
        }
    },
    "eventTime": "2026-03-27T17:12:54Z",
    "eventSource": "secretsmanager.amazonaws.com",
    "eventName": "GetSecretValue",
    "awsRegion": "eu-west-2",
    "sourceIPAddress": "1.2.3.4",
    "userAgent": "aws-sdk-rust/1.3.14 os/linux lang/rust/1.94.1 aws-secrets-manager-agent/2.0.0",
    "requestParameters": {
        "secretId": "arn:aws:secretsmanager:eu-west-2:111122223333:secret:your-secret"
    },
    "responseElements": null,
    "requestID": "027507ea-f377-43d9-bf2f-646d4dc19223",
    "eventID": "f9c3ed0f-81f5-450b-a561-2b9e54fa9e73",
    "readOnly": true,
    "resources": [
        {
            "accountId": "111122223333",
            "type": "AWS::SecretsManager::Secret",
            "ARN": "arn:aws:secretsmanager:eu-west-2:111122223333:secret:your-secret"
        }
    ],
    "eventType": "AwsApiCall",
    "managementEvent": true,
    "recipientAccountId": "111122223333",
    "eventCategory": "Management",
    "tlsDetails": {
        "tlsVersion": "TLSv1.3",
        "cipherSuite": "TLS_AES_128_GCM_SHA256",
        "clientProvidedHostHeader": "secretsmanager.eu-west-2.amazonaws.com",
        "keyExchange": "X25519MLKEM768"
    }
}

</code></pre> 
     </div> 
    </div> 
   </div> 
  </div> 
  <p>If the <code class="CodeInline" style="color: #000;">keyExchange</code> field shows <code class="CodeInline" style="color: #000;">X25519MLKEM768</code>, then hybrid post-quantum key exchange in TLS is active. If it shows a traditional algorithm such as <code class="CodeInline" style="color: #000;">X25519</code>, the client is not advertising ML-KEM support, and you should check the client version and configuration.</p> 
  <div class="RichTextHeading"> 
   <h3><b>Troubleshooting</b></h3> 
  </div> 
  <p>If your Secrets Manager API calls aren’t negotiating <code class="CodeInline" style="color: #000;">X25519MLKEM768</code> after updating your clients, check your SDK version, OpenSSL version (Python), and firewall or proxy configuration as shown in the <b>Client Hybrid Post-Quantum Key Exchange Requirements</b> section near the beginning of this post.</p> 
  <div class="RichTextHeading"> 
   <h2>What’s next</h2> 
  </div> 
  <p>This launch is one step in a broader migration. AWS is continuing to roll out ML-KEM support across AWS service HTTPS endpoints as part of Workstream 2 of the <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/security/aws-post-quantum-cryptography-migration-plan/" rel="noopener" target="_blank">AWS PQC Migration Plan</a></span>, with a target of full coverage across public AWS endpoints.</p> 
  <p>Support for CRYSTALS-Kyber, the pre-standardization predecessor to ML-KEM, is phasing out across AWS endpoints in 2026. Customers on older SDK versions that advertise only CRYSTALS-Kyber support will fall back gracefully to traditional TLS rather than negotiate the deprecated algorithm. To avoid this fallback, upgrade to the SDK versions listed in this post.</p> 
  <p>The journey of PQC migration extends beyond confidentiality of data in transit. To stay informed about the latest developments in the AWS PQC journey and your side of shared responsibility, follow the <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/security/post-quantum-cryptography/" rel="noopener" target="_blank">AWS Post-Quantum Cryptography</a></span> page.</p> 
  <div class="RichTextHeading"> 
   <h2>Conclusion</h2> 
  </div> 
  <p>AWS Secrets Manager now enables hybrid post-quantum key exchange using ML-KEM by default to help protect your secrets and support your compliance efforts. This update requires no code changes or configuration updates for customers using the latest client versions.</p> 
  <p>This post covered how AWS Secrets Manager uses hybrid post-quantum cryptography to secure TLS connections, which clients support this capability, and how to verify that your connections are protected against harvest now, decrypt later attacks.</p> 
  <p>To benefit from this announcement today:</p> 
  <ul class="rte2-style-ul" id="rte-1d7bf0c3-3835-11f1-bc8a-c599d2e4deff"> 
   <li>Upgrade your Secrets Manager client (Agent, Lambda extension, or CSI Driver) to the latest available versions to enable hybrid post-quantum key exchange using ML-KEM</li> 
   <li>If your workload uses the AWS SDK instead of a caching client, upgrade your AWS SDK and underlying dependencies to the minimum versions listed in this post</li> 
   <li>Verify hybrid post-quantum key exchange in TLS is active by checking the <code class="CodeInline" style="color: #000;">keyExchange</code> field in CloudTrail <code class="CodeInline" style="color: #000;">tlsDetails</code> for your Secrets Manager API calls</li> 
   <li>Test end-to-end hybrid post-quantum key exchange TLS connectivity in your environment, including network paths that traverse corporate firewalls or proxies</li> 
  </ul> 
  <p>AWS will continue rolling out post-quantum cryptography support. For information about the broader migration effort, see the <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/security/aws-post-quantum-cryptography-migration-plan/" rel="noopener" target="_blank">AWS PQC Migration Plan</a></span>. Keep an updated cryptographic inventory of your broader environment to identify other uses of traditional public-key cryptography that will require migration. The <span class="LinkEnhancement"><a class="Link" href="https://www.cisa.gov/quantum" rel="noopener" target="_blank">CISA Quantum-Readiness guidance</a></span> and the AWS PQC Migration Plan are good starting points.</p> 
  <p><b>Additional resources</b></p> 
  <ul class="rte2-style-ul" id="rte-1d7bf0c5-3835-11f1-bc8a-c599d2e4deff"> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/security/ml-kem-post-quantum-tls-now-supported-in-aws-kms-acm-and-secrets-manager/" rel="noopener" target="_blank">ML-KEM post-quantum TLS now supported in AWS KMS, ACM, and Secrets Manager</a></span></li> 
   <li><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/security/post-quantum-tls-in-python/" rel="noopener" target="_blank">Post-quantum TLS in Python</a></span></li> 
  </ul> 
 </div> 
</div> 
<p>If you have feedback about this post, submit comments in the <strong>Comments</strong> section below. </p> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image"> 
   <img alt="P. Stéphanie Mbappe" class="aligncenter size-full wp-image-41878" height="385" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/22/StephMbappe_288x384.png" width="288" /> 
  </div> 
  <h3 class="lb-h4">P. Stéphanie Mbappe</h3> 
  <p>Stéphanie is a Security Consultant with Amazon Web Services. She delights in assisting her customers at any step of their security journey. Stéphanie enjoys learning, designing new solutions, and sharing her knowledge with others.</p> 
  <p></p>
 </div> 
</footer> 
<footer> 
 <div class="blog-author-box"> 
  <div class="blog-author-image"> 
   <img alt="Tobias Nickl" class="aligncenter size-full wp-image-41877" height="538" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/04/23/TobiasNickl.jpg" width="548" /> 
  </div> 
  <h3 class="lb-h4">Tobias Nickl</h3> 
  <p>Tobias is a Security Consultant at Amazon Web Services, specializing in security architecture and cloud transformation. He partners with AWS customers to design and implement security architectures that address both current and emerging threats. Through his work, he helps organizations build security strategies that evolve with their cloud maturity.</p> 
  <p></p>
 </div> 
</footer>
