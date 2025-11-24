# Module 11: Customer Service Solutions
*Part 5: Industry Solutions - Service desk and support agents*

## Learning Objectives
By completing this module, you will be able to:
- Design and implement service desk agents using Microsoft Copilot Studio
- Integrate agents with legacy ticketing systems and modern cloud solutions
- Build customer support workflows with REST APIs and SharePoint knowledge bases
- Configure multi-channel support across Teams, web, and phone
- Implement feedback collection and service quality monitoring
- Deploy scalable customer service automation solutions

## Prerequisites
- Completion of Modules 1-6
- Basic understanding of service desk operations
- Familiarity with REST API concepts
- Access to Microsoft Copilot Studio and Power Platform
- Understanding of SharePoint Online and Teams

## Module Overview
This module focuses on building comprehensive customer service solutions using Microsoft Copilot Studio. You'll learn to create intelligent service desk agents that can handle customer inquiries, manage incidents, and provide support across multiple channels while maintaining integration with existing systems.

---

## Lab 1: Building a Service Desk Declarative Agent

### Lab Objectives
- Create a service desk agent connected to multiple data sources
- Configure REST API integration for legacy systems
- Set up SharePoint knowledge base integration
- Implement Service Now connector using Microsoft Graph

### Scenario
You're working for a company transitioning from a legacy service desk system to modern cloud solutions. The company needs a unified agent that can search across both old and new systems while providing seamless customer support.

### Step-by-Step Implementation

#### Step 1: Set Up the Development Environment

1. **Access Microsoft Teams Toolkit**
   - Open Visual Studio Code
   - Install Microsoft Teams Toolkit extension
   - Sign in with your Microsoft 365 developer account

2. **Create New Declarative Agent Project**
   ```bash
   # In VS Code terminal
   npm install -g @microsoft/teamsfx-cli
   teamsfx new --interactive
   ```
   - Select "Bot" → "Declarative Agent"
   - Choose "Service Desk Agent" template
   - Configure project name as "ServiceDeskAgent"

#### Step 2: Configure Agent Manifest

1. **Edit Agent Manifest (appPackage/declarativeAgent.json)**
   ```json
   {
     "version": "v1.0",
     "name": "Service Desk Agent",
     "description": "Intelligent service desk agent for IT support and customer service",
     "instructions": "You are a helpful service desk agent that can search across multiple data sources including legacy systems, Service Now knowledge base, and SharePoint procedures. Help users find solutions to their technical problems and manage service requests.",
     "conversation_starters": [
       "List all incidents related to Outlook",
       "How do I find my wireless interface MAC address?",
       "Help me solve incident #INC001234",
       "Show me the latest service desk procedures"
     ],
     "actions": [
       {
         "id": "serviceDeskAPI",
         "file": "serviceDeskPlugin.json"
       }
     ],
     "capabilities": [
       {
         "name": "GraphConnector",
         "id": "serviceNowKB",
         "description": "Service Now Knowledge Base Articles"
       },
       {
         "name": "OneDriveAndSharePoint", 
         "id": "serviceDeskTeamSite",
         "description": "Service desk procedures and documentation"
       }
     ]
   }
   ```

#### Step 3: Create REST API Plugin

1. **Design API Specification (serviceDeskPlugin.json)**
   ```json
   {
     "openapi": "3.0.0",
     "info": {
       "title": "Service Desk API",
       "version": "1.0.0",
       "description": "Legacy service desk system API"
     },
     "servers": [
       {
         "url": "https://your-api-endpoint.com/api"
       }
     ],
     "paths": {
       "/incidents": {
         "get": {
           "operationId": "listIncidents",
           "summary": "List service desk incidents",
           "parameters": [
             {
               "name": "category",
               "in": "query",
               "description": "Filter incidents by category",
               "schema": { "type": "string" }
             },
             {
               "name": "assigned_to",
               "in": "query", 
               "description": "Filter by assigned technician",
               "schema": { "type": "string" }
             },
             {
               "name": "status",
               "in": "query",
               "description": "Filter by incident status",
               "schema": { "type": "string" }
             }
           ],
           "responses": {
             "200": {
               "description": "List of incidents",
               "content": {
                 "application/json": {
                   "schema": {
                     "type": "object",
                     "properties": {
                       "incidents": {
                         "type": "array",
                         "items": {
                           "type": "object",
                           "properties": {
                             "id": { "type": "string" },
                             "title": { "type": "string" },
                             "description": { "type": "string" },
                             "priority": { "type": "string" },
                             "status": { "type": "string" },
                             "assigned_to": { "type": "string" },
                             "created_date": { "type": "string" },
                             "category": { "type": "string" }
                           }
                         }
                       }
                     }
                   }
                 }
               }
             }
           }
         }
       },
       "/incidents/{incident_id}": {
         "get": {
           "operationId": "getIncident",
           "summary": "Get specific incident details",
           "parameters": [
             {
               "name": "incident_id",
               "in": "path",
               "required": true,
               "schema": { "type": "string" }
             }
           ],
           "responses": {
             "200": {
               "description": "Incident details with adaptive card formatting",
               "content": {
                 "application/json": {
                   "schema": {
                     "type": "object",
                     "properties": {
                       "incident": {
                         "type": "object",
                         "properties": {
                           "id": { "type": "string" },
                           "title": { "type": "string" },
                           "description": { "type": "string" },
                           "priority": { "type": "string" },
                           "status": { "type": "string" },
                           "assigned_to": { "type": "string" },
                           "created_date": { "type": "string" },
                           "updated_date": { "type": "string" },
                           "category": { "type": "string" },
                           "resolution_notes": { "type": "string" }
                         }
                       }
                     }
                   }
                 }
               }
             }
           }
         }
       }
     },
     "components": {
       "securitySchemes": {
         "oauth2": {
           "type": "oauth2",
           "flows": {
             "authorizationCode": {
               "authorizationUrl": "https://login.microsoftonline.com/common/oauth2/v2.0/authorize",
               "tokenUrl": "https://login.microsoftonline.com/common/oauth2/v2.0/token",
               "scopes": {
                 "api://your-api-id/access_as_user": "Access API as user"
               }
             }
           }
         }
       }
     },
     "security": [
       {
         "oauth2": ["api://your-api-id/access_as_user"]
       }
     ]
   }
   ```

#### Step 4: Configure Authentication

1. **Set up Azure AD App Registration**
   - Navigate to Azure Portal → App Registrations
   - Create new registration: "Service Desk API"
   - Configure redirect URLs for your API
   - Note Application ID and create client secret

2. **Configure API Permissions**
   - Add Microsoft Graph permissions:
     - User.Read
     - Sites.Read.All (for SharePoint integration)
   - Add custom API permissions for your service desk system

#### Step 5: Create Sample API Backend

1. **Build Simple Node.js API (for demonstration)**
   ```javascript
   // server.js
   const express = require('express');
   const cors = require('cors');
   const app = express();

   app.use(cors());
   app.use(express.json());

   // Sample incident data
   const incidents = [
     {
       id: "INC001234",
       title: "Outlook not receiving emails",
       description: "User unable to receive emails in Outlook application",
       priority: "High",
       status: "Open",
       assigned_to: "John Smith",
       created_date: "2024-01-15T10:30:00Z",
       category: "Email"
     },
     {
       id: "INC001235", 
       title: "WiFi connectivity issues",
       description: "User experiencing intermittent WiFi disconnections",
       priority: "Medium",
       status: "In Progress", 
       assigned_to: "Jane Doe",
       created_date: "2024-01-15T11:45:00Z",
       category: "Network"
     }
   ];

   // Get incidents with filtering
   app.get('/api/incidents', (req, res) => {
     let filtered = incidents;
     
     if (req.query.category) {
       filtered = filtered.filter(inc => 
         inc.category.toLowerCase().includes(req.query.category.toLowerCase())
       );
     }
     
     if (req.query.assigned_to) {
       filtered = filtered.filter(inc => 
         inc.assigned_to.toLowerCase().includes(req.query.assigned_to.toLowerCase())
       );
     }
     
     if (req.query.status) {
       filtered = filtered.filter(inc => 
         inc.status.toLowerCase().includes(req.query.status.toLowerCase())
       );
     }

     res.json({ incidents: filtered });
   });

   // Get specific incident
   app.get('/api/incidents/:id', (req, res) => {
     const incident = incidents.find(inc => inc.id === req.params.id);
     if (!incident) {
       return res.status(404).json({ error: 'Incident not found' });
     }
     
     res.json({ incident });
   });

   const PORT = process.env.PORT || 3001;
   app.listen(PORT, () => {
     console.log(`Service Desk API running on port ${PORT}`);
   });
   ```

#### Step 6: Deploy and Test the Agent

1. **Deploy to Teams**
   ```bash
   teamsfx provision
   teamsfx deploy
   ```

2. **Test Agent Functionality**
   - Open Microsoft Teams
   - Navigate to Apps → Your deployed agent
   - Test conversation starters:
     - "List all incidents related to Outlook"
     - "Show me incident INC001234"
     - "List incidents assigned to John Smith"

### Expected Results
- Service desk agent successfully deployed to Teams
- Agent can query legacy system via REST API
- Results displayed in user-friendly format
- Authentication working properly

---

## Lab 2: SharePoint Knowledge Base Integration

### Lab Objectives
- Configure SharePoint as knowledge source
- Set up document libraries for service procedures
- Implement content synchronization
- Test knowledge retrieval capabilities

### Scenario
Your service desk needs access to procedures and documentation stored in SharePoint. You'll configure the agent to search through these knowledge sources and provide relevant information to support requests.

### Step-by-Step Implementation

#### Step 1: Prepare SharePoint Knowledge Base

1. **Create SharePoint Team Site**
   - Navigate to SharePoint Admin Center
   - Create new team site: "Service Desk Knowledge Base"
   - Configure site permissions for service desk team

2. **Set Up Document Libraries**
   ```
   Knowledge Base Structure:
   /Procedures/
   ├── Hardware/
   │   ├── Laptop Setup Guide.docx
   │   ├── Printer Installation.docx
   │   └── Network Configuration.docx
   ├── Software/
   │   ├── Office 365 Troubleshooting.docx
   │   ├── VPN Setup Guide.docx
   │   └── Email Configuration.docx
   └── Policies/
       ├── Password Policy.docx
       ├── Security Guidelines.docx
       └── Incident Response.docx
   ```

3. **Upload Sample Documentation**
   Create sample documents with common service desk scenarios:
   
   **Hardware/Network Configuration.docx:**
   ```
   # Network Configuration Guide

   ## Finding Wireless Interface MAC Address

   ### Windows 11:
   1. Press Windows + R
   2. Type "cmd" and press Enter
   3. Type: ipconfig /all
   4. Look for "Wireless LAN adapter Wi-Fi"
   5. Find "Physical Address" - this is your MAC address

   ### Windows 10:
   1. Right-click Start button
   2. Select "Network Connections"
   3. Click "WiFi" on the left panel
   4. Click "Hardware properties"
   5. MAC address listed as "Physical address"

   ### macOS:
   1. Hold Option key and click WiFi icon
   2. MAC address shown as "Address"
   ```

#### Step 2: Configure SharePoint Integration in Agent

1. **Update Agent Manifest**
   ```json
   {
     "capabilities": [
       {
         "name": "OneDriveAndSharePoint",
         "id": "serviceDeskTeamSite",
         "description": "Service desk procedures and documentation",
         "sharepoint_site_urls": [
           "https://yourtenant.sharepoint.com/sites/servicedeskknowledgebase"
         ]
       }
     ]
   }
   ```

2. **Configure Site Permissions**
   - Ensure agent has read access to SharePoint site
   - Configure managed identity or service principal
   - Grant Sites.Read.All permission in Azure AD

#### Step 3: Test Knowledge Retrieval

1. **Test Knowledge Queries**
   - "How do I find my wireless interface MAC address?"
   - "What's the procedure for setting up a laptop?"
   - "Show me the password policy"

2. **Verify Response Quality**
   - Check if agent returns relevant documents
   - Ensure proper citation of sources
   - Validate response accuracy

### Expected Results
- Agent successfully connects to SharePoint knowledge base
- Relevant documents retrieved for user queries
- Proper source citation in responses
- Fast search and retrieval performance

---

## Lab 3: Multi-Channel Customer Support

### Lab Objectives
- Deploy agent across multiple channels (Teams, Web, Phone)
- Configure channel-specific behaviors
- Set up escalation workflows
- Implement handoff to human agents

### Scenario
Your organization needs to provide consistent customer support across Teams, web chat, and phone channels while maintaining context and conversation history.

### Step-by-Step Implementation

#### Step 1: Configure Web Channel

1. **Enable Web Channel in Copilot Studio**
   - Open Microsoft Copilot Studio
   - Navigate to your service desk agent
   - Go to Channels → Web
   - Enable web channel

2. **Customize Web Chat Widget**
   ```javascript
   <script>
     window.WebChat = {
       secret: 'YOUR_WEB_CHAT_SECRET',
       token: null,
       directLineOptions: {
         domain: 'https://directline.botframework.com/v3/directline'
       },
       styleOptions: {
         accent: '#0078d4',
         backgroundColor: 'White',
         cardEmphasisBackgroundColor: '#F3F2F1',
         paddingRegular: 20,
         paddingWide: 20,
         bubbleBackground: '#E5E5E5',
         bubbleFromUserBackground: '#0078d4',
         bubbleFromUserTextColor: 'White'
       }
     };
   </script>
   <script src="https://cdn.botframework.com/botframework-webchat/latest/webchat.js"></script>
   ```

3. **Embed in Support Website**
   ```html
   <!DOCTYPE html>
   <html>
   <head>
     <title>Customer Support</title>
   </head>
   <body>
     <div id="support-header">
       <h1>How can we help you today?</h1>
     </div>
     
     <div id="webchat" role="main" style="height: 600px;">
       <!-- Web chat will be embedded here -->
     </div>

     <script>
       window.WebChat.renderWebChat({
         directLine: window.WebChat.createDirectLine({
           secret: window.WebChat.secret
         }),
         userID: 'user_' + Math.random(),
         username: 'Customer',
         locale: 'en-US'
       }, document.getElementById('webchat'));
     </script>
   </body>
   </html>
   ```

#### Step 2: Configure Phone Integration

1. **Set Up Azure Communication Services**
   - Create Azure Communication Services resource
   - Configure phone numbers
   - Set up SIP trunk connections

2. **Enable Voice Channel**
   ```json
   {
     "channels": {
       "voice": {
         "enabled": true,
         "phoneNumber": "+1234567890",
         "speechServices": {
           "speechToText": "azure-speech-service",
           "textToSpeech": "azure-speech-service"
         },
         "callHandling": {
           "greeting": "Hello, you've reached our service desk. How can I help you today?",
           "noInputMessage": "I didn't hear anything. Could you please repeat your question?",
           "transferMessage": "Let me connect you with a human agent.",
           "endCallMessage": "Thank you for contacting our service desk. Have a great day!"
         }
       }
     }
   }
   ```

3. **Configure Speech Recognition Settings**
   ```javascript
   // Configure speech settings for better recognition
   const speechConfig = {
     recognitionLanguage: "en-US",
     outputFormat: "Simple",
     profanityOption: "Masked",
     requestWordTimestamps: true,
     enableAutomaticPunctuation: true
   };
   ```

#### Step 3: Implement Escalation Logic

1. **Create Escalation Triggers**
   ```json
   {
     "escalationRules": [
       {
         "condition": "sentiment < 0.3",
         "action": "escalate_to_human",
         "message": "I understand this is frustrating. Let me connect you with a human agent who can provide more personalized assistance."
       },
       {
         "condition": "conversation_turns > 10",
         "action": "offer_escalation",
         "message": "Would you like me to connect you with a human agent for more detailed assistance?"
       },
       {
         "condition": "intent_confidence < 0.5",
         "action": "clarify_or_escalate",
         "message": "I'm not sure I understand your request correctly. Would you like to speak with a human agent?"
       }
     ]
   }
   ```

2. **Configure Power Automate Escalation Flow**
   - Create flow triggered by escalation event
   - Route to appropriate support queue
   - Transfer conversation context
   - Notify human agents

### Expected Results
- Agent deployed across Teams, web, and phone channels
- Consistent behavior across all channels
- Working escalation to human agents
- Proper context preservation during handoffs

---

## Lab 4: Customer Feedback Collection System

### Lab Objectives
- Build automated feedback collection system
- Create feedback analysis dashboard
- Implement continuous improvement workflows
- Monitor service quality metrics

### Scenario
You need to collect customer feedback after service interactions to measure satisfaction and identify improvement opportunities, similar to the Copilot feedback bot implementation.

### Step-by-Step Implementation

#### Step 1: Create Feedback Collection Bot

1. **Bot Framework Setup**
   ```csharp
   // FeedbackBot.cs
   using Microsoft.Bot.Builder;
   using Microsoft.Bot.Schema;
   using System.Threading;
   using System.Threading.Tasks;

   public class ServiceDeskFeedbackBot : ActivityHandler
   {
       protected override async Task OnMessageActivityAsync(
           ITurnContext<IMessageActivity> turnContext, 
           CancellationToken cancellationToken)
       {
           var userMessage = turnContext.Activity.Text;
           
           if (userMessage.ToLower().Contains("start feedback"))
           {
               await SendFeedbackSurvey(turnContext, cancellationToken);
           }
       }

       private async Task SendFeedbackSurvey(
           ITurnContext turnContext, 
           CancellationToken cancellationToken)
       {
           var feedbackCard = CreateFeedbackCard();
           var response = MessageFactory.Attachment(feedbackCard);
           await turnContext.SendActivityAsync(response, cancellationToken);
       }

       private Attachment CreateFeedbackCard()
       {
           var adaptiveCard = new AdaptiveCard("1.3")
           {
               Body = new List<AdaptiveElement>
               {
                   new AdaptiveTextBlock
                   {
                       Text = "Service Desk Feedback",
                       Size = AdaptiveTextSize.Large,
                       Weight = AdaptiveTextWeight.Bolder
                   },
                   new AdaptiveTextBlock
                   {
                       Text = "How would you rate your recent support experience?",
                       Wrap = true
                   },
                   new AdaptiveChoiceSetInput
                   {
                       Id = "rating",
                       Style = AdaptiveChoiceInputStyle.Expanded,
                       Choices = new List<AdaptiveChoice>
                       {
                           new AdaptiveChoice { Title = "😊 Excellent (5)", Value = "5" },
                           new AdaptiveChoice { Title = "🙂 Good (4)", Value = "4" },
                           new AdaptiveChoice { Title = "😐 Average (3)", Value = "3" },
                           new AdaptiveChoice { Title = "😞 Poor (2)", Value = "2" },
                           new AdaptiveChoice { Title = "😠 Very Poor (1)", Value = "1" }
                       }
                   },
                   new AdaptiveTextInput
                   {
                       Id = "timeSaved",
                       Placeholder = "How much time did we save you? (e.g., 30 minutes)",
                       Label = "Time Saved"
                   },
                   new AdaptiveTextInput
                   {
                       Id = "comments",
                       Placeholder = "Any additional feedback?",
                       IsMultiline = true,
                       Label = "Comments"
                   }
               },
               Actions = new List<AdaptiveAction>
               {
                   new AdaptiveSubmitAction
                   {
                       Title = "Submit Feedback",
                       Data = new { action = "submitFeedback" }
                   }
               }
           };

           return new Attachment
           {
               ContentType = AdaptiveCard.ContentType,
               Content = adaptiveCard
           };
       }
   }
   ```

#### Step 2: Implement Feedback Processing

1. **Create Feedback Data Model**
   ```csharp
   // Models/FeedbackModel.cs
   public class ServiceFeedback
   {
       public string Id { get; set; }
       public string UserId { get; set; }
       public string UserEmail { get; set; }
       public DateTime FeedbackDate { get; set; }
       public int Rating { get; set; }
       public string TimeSaved { get; set; }
       public string Comments { get; set; }
       public string TicketId { get; set; }
       public string Category { get; set; }
       public string SupportChannel { get; set; }
   }
   ```

2. **Store Feedback in SQL Database**
   ```sql
   CREATE TABLE ServiceFeedback (
       Id UNIQUEIDENTIFIER PRIMARY KEY DEFAULT NEWID(),
       UserId NVARCHAR(255),
       UserEmail NVARCHAR(255),
       FeedbackDate DATETIME2 DEFAULT GETUTCDATE(),
       Rating INT,
       TimeSaved NVARCHAR(100),
       Comments NVARCHAR(MAX),
       TicketId NVARCHAR(50),
       Category NVARCHAR(100),
       SupportChannel NVARCHAR(50)
   );
   ```

3. **Process Feedback Submissions**
   ```csharp
   protected override async Task OnSubmitActionAsync(
       ITurnContext<IInvokeActivity> turnContext,
       CancellationToken cancellationToken)
   {
       var value = turnContext.Activity.Value as JObject;
       
       if (value?.GetValue("action")?.ToString() == "submitFeedback")
       {
           var feedback = new ServiceFeedback
           {
               UserId = turnContext.Activity.From.Id,
               UserEmail = turnContext.Activity.From.Name,
               Rating = int.Parse(value.GetValue("rating")?.ToString() ?? "0"),
               TimeSaved = value.GetValue("timeSaved")?.ToString(),
               Comments = value.GetValue("comments")?.ToString(),
               SupportChannel = turnContext.Activity.ChannelId,
               Category = DetermineFeedbackCategory(value.GetValue("comments")?.ToString())
           };

           await SaveFeedbackAsync(feedback);
           
           await turnContext.SendActivityAsync(
               MessageFactory.Text("Thank you for your feedback! We use this information to improve our service."),
               cancellationToken);
       }
   }
   ```

#### Step 3: Create Feedback Analytics Dashboard

1. **Power BI Report Configuration**
   ```json
   {
     "dashboardConfig": {
       "dataSource": "sql-server",
       "refreshSchedule": "hourly",
       "visualizations": [
         {
           "type": "gauge",
           "title": "Overall Satisfaction",
           "measure": "AVG(Rating)",
           "target": 4.0
         },
         {
           "type": "line-chart",
           "title": "Satisfaction Trend",
           "xAxis": "FeedbackDate",
           "yAxis": "Rating"
         },
         {
           "type": "bar-chart",
           "title": "Feedback by Channel",
           "category": "SupportChannel",
           "measure": "COUNT(*)"
         }
       ]
     }
   }
   ```

2. **Automated Feedback Triggers**
   ```javascript
   // Power Automate Flow for automated feedback requests
   {
     "definition": {
       "triggers": {
         "when_ticket_resolved": {
           "type": "SharePoint",
           "inputs": {
             "site": "service-desk-site",
             "list": "support-tickets",
             "filter": "Status eq 'Resolved'"
           }
         }
       },
       "actions": {
         "send_feedback_request": {
           "type": "Teams",
           "inputs": {
             "recipient": "@{triggerBody()['CustomerEmail']}",
             "message": "Your support ticket has been resolved. We'd love your feedback on the experience.",
             "attachments": [
               {
                 "contentType": "application/vnd.microsoft.card.adaptive",
                 "content": "@{variables('feedbackCard')}"
               }
             ]
           }
         }
       }
     }
   }
   ```

### Expected Results
- Automated feedback collection after service interactions
- Real-time feedback analytics dashboard
- Improved service quality metrics tracking
- Actionable insights for service improvement

---

## Real-World Implementation Examples

### Case Study 1: Multi-System Service Desk Integration
**Challenge:** A company with legacy service desk system transitioning to Service Now while maintaining SharePoint documentation needed unified search capabilities.

**Solution:**
- Built declarative agent with REST API integration to legacy system
- Configured Microsoft Graph connector for Service Now knowledge base
- Integrated SharePoint team site for procedures and documentation
- Implemented OAuth2 authentication for secure access

**Results:**
- 60% reduction in search time across systems
- Unified interface for service desk staff
- Seamless transition during migration period
- Improved customer satisfaction scores

### Case Study 2: Multi-Channel Customer Support
**Company:** Technology services provider requiring 24/7 support across multiple channels

**Implementation:**
- Deployed service desk agent to Teams, web chat, and phone
- Configured automatic escalation based on sentiment analysis
- Integrated with existing CRM for ticket creation
- Set up feedback collection for continuous improvement

**Outcomes:**
- 40% reduction in first-call resolution time
- Consistent support experience across channels
- Automated routing reduced human agent workload by 30%
- Improved customer satisfaction ratings

---

## Troubleshooting Guide

### Common Issues and Solutions

#### Issue 1: API Authentication Failures
**Symptoms:**
- "Unauthorized" errors when calling REST API
- Agent cannot retrieve incident data

**Solutions:**
1. **Verify Azure AD App Registration**
   ```bash
   # Check app registration in Azure CLI
   az ad app list --display-name "Service Desk API"
   ```

2. **Validate API Permissions**
   - Ensure API permissions are properly configured
   - Check if admin consent has been granted
   - Verify redirect URLs match your implementation

3. **Test API Endpoints Manually**
   ```bash
   curl -H "Authorization: Bearer YOUR_TOKEN" \
        https://your-api-endpoint.com/api/incidents
   ```

#### Issue 2: SharePoint Integration Not Working
**Symptoms:**
- Agent cannot find SharePoint documents
- "Access denied" errors

**Solutions:**
1. **Check SharePoint Permissions**
   - Verify agent has Sites.Read.All permission
   - Ensure SharePoint site is accessible
   - Check if site is properly configured in agent manifest

2. **Test SharePoint Connection**
   ```powershell
   # Test SharePoint access with PowerShell
   Connect-PnPOnline -Url "https://tenant.sharepoint.com/sites/sitename" -Interactive
   Get-PnPListItem -List "Documents"
   ```

#### Issue 3: Phone Integration Issues
**Symptoms:**
- Poor speech recognition
- Call drops or connection issues

**Solutions:**
1. **Optimize Speech Settings**
   - Configure appropriate language models
   - Adjust speech recognition parameters
   - Test with different phone line qualities

2. **Check Azure Communication Services**
   ```csharp
   // Test communication services connection
   var connectionString = "your-acs-connection-string";
   var client = new CallAutomationClient(connectionString);
   ```

---

## Best Practices

### Agent Design
- **Keep responses concise**: Service desk users need quick, actionable information
- **Provide context**: Always include relevant incident or ticket information
- **Enable easy escalation**: Make it simple to transfer to human agents
- **Maintain conversation history**: Preserve context across channel switches

### Integration Patterns
- **Use caching**: Implement caching for frequently accessed API data
- **Handle rate limits**: Implement retry logic with exponential backoff
- **Monitor performance**: Track API response times and success rates
- **Secure sensitive data**: Never expose customer data in logs

### User Experience
- **Progressive disclosure**: Start with basic information, offer details on request
- **Visual formatting**: Use adaptive cards for structured information display
- **Clear navigation**: Provide clear conversation starters and help options
- **Feedback loops**: Continuously collect and act on user feedback

### Deployment Strategy
- **Phased rollout**: Start with pilot group before full deployment
- **A/B testing**: Test different conversation flows and responses
- **Performance monitoring**: Track key metrics like resolution time and satisfaction
- **Regular updates**: Keep knowledge base current with latest procedures

---

## Knowledge Check

### Multiple Choice Questions

1. **Which authentication method is required for REST API integration in declarative agents?**
   a) Basic authentication
   b) API key authentication
   c) OAuth2 with Azure AD
   d) Certificate-based authentication

2. **What file format is used to define API operations in declarative agents?**
   a) XML schema
   b) JSON schema
   c) OpenAPI specification
   d) WSDL definition

3. **Which Microsoft Graph permission is required for SharePoint integration?**
   a) Sites.Read.All
   b) Files.Read.All
   c) SharePoint.Read
   d) Documents.Read.All

### Practical Exercises

1. **Create a custom API endpoint** that returns service desk metrics in JSON format
2. **Design an adaptive card** for displaying incident details with action buttons
3. **Configure a Power Automate flow** that creates SharePoint list items from service requests

### Assessment Criteria
- Working REST API integration with proper authentication
- Functional SharePoint knowledge base search
- Effective multi-channel deployment
- Implemented feedback collection system
- Comprehensive troubleshooting documentation

---

## Additional Resources

### Documentation
- [Microsoft Copilot Studio Documentation](https://docs.microsoft.com/copilot-studio)
- [Declarative Agents Overview](https://docs.microsoft.com/teams/platform/bots/how-to/declarative-agents)
- [OpenAPI Specification Guide](https://swagger.io/specification/)
- [Azure Communication Services](https://docs.microsoft.com/azure/communication-services/)

### Sample Code Repositories
- [Service Desk Agent Template](https://github.com/microsoft/teams-toolkit-samples)
- [REST API Integration Examples](https://github.com/microsoft/copilot-studio-samples)
- [Multi-Channel Bot Samples](https://github.com/microsoft/botframework-samples)

### Training Resources
- Microsoft Learn: Building Service Desk Solutions
- Power Platform: Copilot Studio Advanced Scenarios
- Azure: Communication Services Implementation

---

This completes Module 11: Customer Service Solutions. You now have the skills to build comprehensive service desk agents that integrate with multiple systems, provide multi-channel support, and continuously improve through feedback collection and analysis.