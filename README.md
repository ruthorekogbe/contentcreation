# contentcreation


Introduction
This system helps businesses
automate repetitive marketing tasks, allowing you to focus on what matters most. With our
advanced AI-powered workflows, you'll experience:
● 85% Time Saved on Manual Tasks
● 93% Accuracy Improvement in Content Creation
● 40% Average Cost Reduction for Marketing Operation
This guide will walk you through setting up and using our powerful n8n-based marketing
automation system.

System Overview
Our AI Marketing System consists of interconnected workflows that handle:
1. Content Generation: Creating blog posts, social media updates
2. Image Creation: Generating visuals from text prompts
3. Media Processing: Converting and formatting content
4. Distribution: Publishing to multiple platforms automatically
5. Logging & Analytics: Tracking performance in Google Sheets

Step-by-Step Setup Guide
1. Trigger Configuration
Setup Instructions:
1. Create a new workflow in n8n
2. Add a "Trigger" node (select either option):
○ Telegram Trigger: For command-based activation
○ When Executed by Another Workflow: For sequential processing
3. For Telegram setup:
○ Enter your Telegram bot token
○ Configure message pattern to recognize commands (e.g., "/create_update")
4. Test the trigger by sending a test command

2. Content Generation Setup
Setup Instructions:
1. Add "Tools Agent" node after the trigger
2. Select the appropriate agent type:
○ Blog Post Agent
○ Image Prompt Agent
3. Connect to GPT 4.1 Mini using Model dropdown
4. Configure memory settings to maintain context
5. Customize prompt templates for your brand voice
6. Set structured output format (JSON recommended)
3. Image Generation with PIAPI
Setup Instructions:
1. Add "HTTP Request" node after content generation
2. Configure as follows:
○ Method: POST
○ URL: [Your PIAPI endpoint]

Headers:
{ "Authorization": "Bearer YOUR_PIAPI_KEY", "Content-Type": "application/json"}

○
Body (JSON):
{ "prompt": "{{$node['Image Prompt Agent'].json.prompt}}", "width": 1024, "height": 1024,
"style": "YOUR_PREFERRED_STYLE"}

○
3. Test connection by generating a simple image
4. Media Processing Configuration
Setup Instructions:
1. For image workflows:
○ Add "Convert to Binary" node
○ Configure to accept image data from PIAPI
○ Map output fields appropriately
2. For file handling:
○ Add "Move Base64 String to File" node
○ Set appropriate file path and naming convention

○ Ensure proper file extension (.jpg/.png)

5. Distribution Setup
Setup Instructions:
1. Add platform-specific nodes:
○ Telegram: "Send Photo" node with bot token
○ LinkedIn: HTTP request with authentication
○ Blog: WordPress or CMS integration
2. Configure message formatting for each platform
3. Test each distribution channel individually
6. Storage & Logging
Setup Instructions:
1. Add "Google Drive" upload node:
○ Connect your Google account
○ Set appropriate folder destination
○ Configure file naming convention
2. Add "Google Sheets" logging node:
○ Link to your tracking spreadsheet
○ Map data fields to columns
○ Include timestamps and performance metrics

Workflow Connections Guide
Basic Content Workflow
Connect nodes in this sequence:
1. Trigger → Blog Post Agent
2. Blog Post Agent → Image Prompt Agent
3. Image Prompt Agent → PIAPI HTTP Request
4. PIAPI HTTP Request → Convert to Binary
5. Convert to Binary → Split node with connections to:
○ Send Photo (Telegram)
○ Send Blog (Content platforms)
○ Upload (Google Drive)
○ Image Log (Google Sheets)

Image Search Workflow
Connect nodes in this sequence:
1. Trigger → Image Search Agent
2. Image Search Agent → Not Found? (Conditional)
3. Not Found? → Split paths:
○ If found: Get Image → Download → Send
○ If not found: Generate new image with PIAPI

Advanced Media Workflow
For video/audio content:
1. Generate Content → Generate Media Prompt
2. Media Prompt → HTTP Request to media service
3. HTTP Request → Download → Process → Distribute

Using The System
Example Commands
● /create_blog [topic] - Generate a blog post about the specified topic
● /create_image [prompt] - Generate an image based on the prompt
● /search_image [description] - Find an image in your database
● /schedule_post [platform] [time] - Schedule content for posting
Best Practices
1. Start Simple: Begin with basic workflows before complex automation
2. Test Thoroughly: Verify each step before full implementation
3. Monitor Performance: Review logs regularly to identify improvements
4. Refine Prompts: Continuously improve your AI prompts for better results
5. Update Credentials: Refresh API keys and tokens regularly

Troubleshooting

Issue Solution

Workflow not triggering Check Telegram bot token and webhook

settings

Poor quality content Refine GPT prompts and increase context

length

Image generation fails Verify PIAPI key and endpoint URL
Distribution errors Check platform authentication credentials
Missing logs Confirm Google Sheets access permissions

Performance Metrics
Track these key metrics in your Google Sheets log:
● Content generation time
● Distribution success rate
● Engagement metrics by platform
● Resource usage (API calls, tokens)
● Cost savings vs. manual methods
