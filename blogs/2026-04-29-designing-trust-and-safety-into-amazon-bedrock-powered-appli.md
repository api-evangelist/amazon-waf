---
title: "Designing trust and safety into Amazon Bedrock powered applications"
url: "https://aws.amazon.com/blogs/security/designing-trust-and-safety-into-amazon-bedrock-powered-applications/"
date: "Wed, 29 Apr 2026 19:27:33 +0000"
author: "Victor Lungu"
feed_url: "https://aws.amazon.com/blogs/security/feed/"
---
<div class="Page-articleBody"> 
 <div class="RichTextArticleBody RichTextBody"> 
  <p>Generative AI brings promising innovation, transforming how individuals and organizations approach everything from customer service to content creation and more. As AI continues to expand its capabilities, organizations are increasingly focused on how they can integrate the responsible AI concepts into the development lifecycle of their AI applications.</p> 
  <p><span class="LinkEnhancement"><a class="Link" href="https://d1.awsstatic.com/products/ai/responsible-ai/Accenture-Responsible-AI-From-Risk-Mitigation-to-Value-Creation.pdf" rel="noopener" target="_blank">Research from Accenture and Amazon Web Services (AWS)</a></span> reveals compelling evidence for the business value of responsible AI practices, both internally within their organizations and externally to their users. Organizations that communicate a mature approach to responsible AI see an 82% improvement in employee trust in AI adoption, which directly leads to increased innovation. Additionally, companies that offer responsible AI-enabled products and services experience a 25% increase in customer loyalty and satisfaction.</p> 
  <div class="RichTextHeading"> 
   <h2>Understanding the core dimensions of responsible AI</h2> 
   <p></p>
  </div> 
  <p>AWS identifies these key dimensions that form the backbone of responsible AI implementation:</p> 
  <ul> 
   <li><b>Safety</b> focuses on preventing harmful system output and misuse. This dimension focuses on steering AI systems to prioritize user and system safety.</li> 
   <li><b>Controllability</b> focuses on mechanisms that monitor and steer AI system behavior. This dimension refers to the ability to manage, guide, and constrain AI systems to operate within specific parameters.</li> 
   <li><b>Fairness</b> considers the impacts of AI on different groups of users.</li> 
   <li><b>Explainability</b> focuses on understanding and evaluating system outputs.</li> 
   <li><b>Security and privacy</b> focuses on making sure that data and models are appropriately obtained, used, and protected.</li> 
   <li><b>Veracity and robustness</b> focuses on achieving correct system outputs, even with unexpected or adversarial inputs.</li> 
   <li><b>Governance</b> makes sure that development, deployment, and management of AI systems align with ethical standards, legal requirements, and societal values.</li> 
   <li><b>Transparency</b> focuses on understanding how AI systems make decisions, why the systems produce specific results, and what data the systems use.</li> 
  </ul> 
  <p>It’s a best practice to review and apply all these dimensions to your AI implementation. For more information, see <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/machine-learning/considerations-for-addressing-the-core-dimensions-of-responsible-ai-for-amazon-bedrock-applications/" rel="noopener" target="_blank">Considerations for addressing the core dimensions of responsible AI for Amazon Bedrock applications</a></span>.</p> 
  <div class="RichTextHeading"> 
   <h2>The responsible AI lifecycle</h2> 
   <p></p>
  </div> 
  <p>When you implement AI systems, you should build safety into every phase of the AWS responsible AI lifecycle. The responsible AI lifecycle consists of the following three phases, each with distinct responsibility considerations for the safety dimension:</p> 
  <ol> 
   <li><b>In the design and development phases</b>, thoroughly evaluate potential safety risks. Understand what you want your AI application to do, what you don’t want it to do, and what you want to prevent it from doing. You should build safety guardrails into your systems from the beginning and make sure that your development teams understand the capabilities and limits of your AI application.</li> 
   <li><b>In the deployment phase</b>, theory meets reality. During this phase, you should implement robust safety measures through multiple layers, from comprehensive user training to proactive monitoring and review processes. Every application, product, and feature must include clear safety protocols and user guidelines. You must think beyond the launch of an application and consider how to launch a holistic safety framework. This framework—which can contain steps such as red team testing—must protect your brand, users, and stakeholders.</li> 
   <li><b>In</b> <b>the operations phase</b>, it’s important to maintain vigilance. Safety, like security, isn’t something you set up once and then ignore. Safety requires continuous monitoring and adaptation. To catch potential safety issues early, you can implement real-time feedback mechanisms to conduct regular performance evaluations. You can also continuously monitor for shifts in how your application is used, or functions that could compromise safety. Because safety considerations and risks evolve as technology evolves, it’s crucial to understand that adjustments are necessary over time.</li> 
  </ol> 
  <p>For more information, see the <span class="LinkEnhancement"><a class="Link" href="https://d1.awsstatic.com/products/generative-ai/responsbile-ai/AWS-Responsible-Use-of-AI-Guide-Final.pdf" rel="noopener" target="_blank">Responsible use of AI guide</a></span>.</p> 
  <div class="RichTextHeading"> 
   <h2>Abuse detection</h2> 
   <p></p>
  </div> 
  <p>Foundation models in <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/bedrock" rel="noopener" target="_blank">Amazon Bedrock</a></span> are inherently designed with safety mechanisms to prevent harmful outputs. However, you can implement additional input safety systems in production environments to provide critical early detection capabilities to identify problematic content, users, or patterns.</p> 
  <blockquote>
   <p> <b>Note</b>: Amazon Bedrock might implement automated abuse detection mechanisms to identify potential violations of the <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/aup/" rel="noopener" target="_blank">AWS Acceptable Use Policy</a></span> (AUP) and Service Terms, including the <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/machine-learning/responsible-ai/policy/" rel="noopener" target="_blank">Responsible AI Policy</a></span> or a third-party model provider’s AUP.</p>
  </blockquote> 
  <p>See the <span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/bedrock/latest/userguide/abuse-detection.html" rel="noopener" target="_blank">Amazon Bedrock abuse detection document</a></span> for more information.</p> 
  <div class="RichTextHeading"> 
   <h2>AI abuse prevention tools and techniques</h2> 
   <p></p>
  </div> 
  <p>To maintain trust in your AI services, preventative action is key, while also efficiently planning and managing development resources. Introduce observability and safety guardrails early in development to support long-term scalability and help identify potential issues before they affect your users. To begin this process, thoroughly scope your AI use case with the following actions:</p> 
  <ul> 
   <li>Understand your users</li> 
   <li>Anticipate potential misuse scenarios</li> 
   <li>Define your risk tolerance</li> 
  </ul> 
  <p>This scope guides your development of a precise safety framework that addresses the specific risks of your AI implementation while you maintain expected performance. For this scope, you can use AWS specialized tools designed specifically to monitor and protect Amazon Bedrock applications.</p> 
  <div class="RichTextHeading"> 
   <h3>Using CloudWatch to monitor Amazon Bedrock</h3> 
   <p></p>
  </div> 
  <p><span class="LinkEnhancement"><a class="Link" href="https://docs.aws.amazon.com/bedrock/latest/userguide/model-invocation-logging.html" rel="noopener" target="_blank">Amazon CloudWatch</a></span> provides essential visibility into AI system behavior and performance. When you configure comprehensive logging, you can capture important information across user segments and interaction types, such as the following:</p> 
  <ul> 
   <li>Request volumes</li> 
   <li>Response latencies</li> 
   <li>Rejection rates</li> 
   <li>Content filtering triggers</li> 
  </ul> 
  <p>You can use this information to identify potential abuse patterns or unexpected behaviors before they affect operations. CloudWatch dashboards visualize metrics according to monitoring priorities, and automated alerts provide prompt notification when you exceed thresholds. This infrastructure transforms interaction data into actionable insights and supports continuous safety improvement.</p> 
  <blockquote>
   <p><b>Note</b>: By default, Amazon Bedrock logging is turned off. You must turn on logging for your application. To configure this, contact your account manager.</p>
  </blockquote> 
  <div class="RichTextHeading"> 
   <h3>Using Amazon Bedrock Guardrails to customize safeguards</h3> 
   <p></p>
  </div> 
  <p><span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/bedrock/guardrails/" rel="noopener" target="_blank">Amazon Bedrock Guardrails</a></span> offers configurable protection mechanisms tailored to specific risk profiles and content policies. You can customize Bedrock Guardrails to match your application requirements, such as:</p> 
  <ul> 
   <li>Define domain-relevant undesirable topics</li> 
   <li>Configure appropriate content filtering thresholds</li> 
   <li>Configure sensitive information detection and redaction parameters aligned with data policies</li> 
  </ul> 
  <p>Additionally, you can configure controls that prioritize accuracy and prevent hallucinations while maintaining creative flexibility based on your application needs. When you thoughtfully configure Guardrails, you can balance performance and safety according to your specific use case requirements and risk factors.</p> 
  <div class="RichTextHeading"> 
   <h2>The abuse response process</h2> 
   <p></p>
  </div> 
  <p>As AI safety evolves and new risks emerge, abuse might still occur even if you implement safety mechanisms. If you <span class="LinkEnhancement"><a class="Link" href="https://repost.aws/knowledge-center/aws-abuse-report" rel="noopener" target="_blank">receive an abuse report </a></span>from the AWS Trust &amp; Safety team, then complete the following steps to help effectively address the issue:</p> 
  <ol class="rte2-style-ol" id="rte-2b8eb841-1806-11f1-ac0b-2f0e041973c6" start="1"> 
   <li><b>Acknowledge receipt</b>: Acknowledge the receipt of the abuse report within 24 hours. If your team is still conducting their investigation, then inform AWS that the investigation is ongoing. Provide the number of days expected to complete the investigation.</li> 
   <li><b>Investigate the issue</b>: Thoroughly investigate the issue, including examining the logs (if enabled), reviewing Amazon Bedrock inputs, and checking for unauthorized access. While AWS abuse reports include a small sample of prompt IDs for you to investigate, investigate usage of your Amazon Bedrock application. Check for patterns to see if there’s a systemic issue that’s leading to abuse.</li> 
   <li><b>Take appropriate action</b>: If appropriate, take action to implement fixes, update safeguards, address violating users, or redesign features. Consider if you need systemic or root-cause fixes, rather than addressing one abusive end user. An abuse incident by one user could indicate vulnerabilities in your safety mechanisms that can lead to continuous abuse.</li> 
   <li><b>Report back to AWS Trust &amp; Safety</b>: Following your investigation and implementation of fixes, provide an update to AWS Trust &amp; Safety on your findings and remediation steps. Be transparent about what happened and how you addressed the issue. If you think that no violation occurred, then provide context on how you came to this conclusion. Include examples of the prompts and your business use case where possible.</li> 
  </ol> 
  <div class="RichTextHeading"> 
   <h2>Conclusion</h2> 
   <p></p>
  </div> 
  <p>To learn more about safety and responsible AI development, explore AWS resources, including the <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/ai/responsible-ai/" rel="noopener" target="_blank">Responsible AI portal</a></span> and <span class="LinkEnhancement"><a class="Link" href="https://aws.amazon.com/blogs/machine-learning/category/post-types/best-practices/" rel="noopener" target="_blank">machine learning best practices documentation</a></span>. These resources provide additional tools and frameworks to build safe, effective AI systems that drive innovation and maintain safety standards.</p> 
  <p></p>
 </div> 
</div> 
<footer> 
 <div class="blog-author-box">
  <img alt="Victor Lungu" class="alignleft size-full" src="https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2026/03/16/Victor-Lungu-Author.jpg" style="margin-left: 12px; margin-right: 18px; margin-top: 12px; margin-bottom: 6px; width: 93.750px; height: 125px;" />
  <span class="lb-h4" style="line-height: 2.1em; padding-top: 12px; margin-top: 24px;">Victor Lungu</span>
  <br />Victor is a Trust &amp; Safety AI Abuse Specialist at AWS, based in Dublin. Victor works across a broad range of AI safety domains including content safety and emerging AI risks
 </div> 
</footer>
