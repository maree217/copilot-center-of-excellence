# Module 1: Copilot Studio Fundamentals

## Learning Objectives
By the end of this module, you will be able to:
- Understand what Microsoft Copilot Studio is and its core capabilities
- Navigate the Copilot Studio interface
- Distinguish between conversational and autonomous agents
- Set up your first basic copilot using generative answers
- Understand the role of Copilot Studio in the broader Microsoft ecosystem

## Overview

Microsoft Copilot Studio is a low-code platform that enables you to build custom AI-powered copilots and extend Microsoft 365 Copilot. Originally known as Power Virtual Agents, it was rebranded at Microsoft Ignite to reflect its expanded capabilities in the AI era.

### What is Microsoft Copilot Studio?

At its core, Copilot Studio can do two main things:

1. **Build Custom Copilots**: Create standalone conversational AI agents (formerly chatbots) that can be deployed across multiple channels
2. **Extend First-Party Copilots**: Enhance Microsoft 365 Copilot and Dynamics 365 copilots with custom capabilities

### Key Architecture Components

Copilot Studio sits at the foundation level underneath various Microsoft copilots, leveraging:
- Azure AI Studio
- Bot Framework
- Power Platform Connectors (both custom and pre-built)
- Microsoft Graph
- Azure OpenAI Services

## Getting Started with Copilot Studio

### Prerequisites
- Microsoft 365 license with Copilot Studio access
- Power Platform environment
- Appropriate permissions to create agents

### Accessing Copilot Studio

1. **Navigate to Copilot Studio**
   - Go to `https://copilotstudio.microsoft.com`
   - Or access through PowerPlatform admin center
   - Sign in with your organizational credentials

2. **Environment Setup**
   - Select your Power Platform environment
   - Ensure you have maker permissions
   - Verify license allocation

## Core Concepts

### Types of Agents

#### Conversational Agents
- **Retrieval Agents**: Get information from grounded data sources
- **Task-Based Agents**: Automate actions and perform tasks
- **Interactive**: Require user interaction through chat interface
- **Analogy**: Like GPS in your car - you tell it where to go and interact with it

#### Autonomous Agents
- **Independent Operation**: Can operate without user interaction
- **Trigger-Based**: Activated by external events
- **Self-Directed**: Make decisions independently based on instructions
- **Analogy**: Like a self-driving car - once it knows the destination, it operates independently

### Building Blocks of Agents

1. **Triggers** (Autonomous agents only)
   - External events that activate the agent
   - Map directly to Power Platform connectors
   - Examples: SharePoint item created, email received, calendar event

2. **Knowledge Sources**
   - Public websites (via Bing search)
   - SharePoint sites and documents
   - Uploaded files
   - Azure AI Search indexes
   - Dataverse tables

3. **Actions**
   - Power Platform connector actions
   - Power Automate flows
   - HTTP requests
   - Custom API calls

4. **Instructions**
   - Natural language guidance for the agent
   - Define behavior and decision-making logic
   - Most critical component for success

## Lab 1: Creating Your First Copilot with Generative Answers

### Scenario
Create a support copilot that can answer questions about Microsoft products using microsoft.com as a knowledge source.

### Step-by-Step Instructions

#### Step 1: Create New Agent
1. In Copilot Studio, click **"Create"** > **"New agent"**
2. Choose **"Skip to configure"** to bypass the conversation setup
3. Enter agent details:
   - **Name**: "Microsoft Support Assistant"
   - **Description**: "Helps users with Microsoft product questions"
   - **Instructions**: "You are a helpful assistant that answers questions about Microsoft products and services"

#### Step 2: Add Knowledge Source
1. Scroll to **"Knowledge"** section
2. Click **"+ Add knowledge"**
3. Select **"Public websites"**
4. Enter URL: `microsoft.com`
5. Click **"Add"**
6. Wait for indexing to complete (may take several minutes)

#### Step 3: Configure Generative Answers
1. Navigate to **"Generative AI"** tab
2. Enable **"Generative answers"**
3. Verify microsoft.com is selected as a knowledge source
4. Configure settings:
   - **Moderation**: Medium
   - **Content Filter**: On

#### Step 4: Test Your Copilot
1. Click **"Test"** button
2. Try these sample prompts:
   - "How do I restart my Xbox?"
   - "What are the system requirements for Windows 11?"
   - "How do I recover a deleted file in OneDrive?"

#### Step 5: Review Responses
1. Observe how the copilot:
   - Searches across microsoft.com
   - Provides contextual answers
   - Includes citations and sources
   - Handles follow-up questions

### Expected Results
- The copilot should provide relevant answers with citations
- Follow-up questions should maintain context
- Sources should be properly attributed

## Lab 2: Understanding Multi-Channel Deployment

### Scenario
Deploy your copilot to Microsoft Teams for internal use.

### Step-by-Step Instructions

#### Step 1: Publish Your Agent
1. Click **"Publish"** in the top menu
2. Review deployment settings
3. Click **"Publish"** to make agent available

#### Step 2: Add to Teams
1. Go to **"Channels"** tab
2. Click **"Microsoft Teams"**
3. Configure Teams settings:
   - **App name**: Microsoft Support Assistant
   - **Short description**: Internal support copilot
   - **Icon**: Upload or use default

#### Step 3: Install in Teams
1. Click **"Open bot"** to launch Teams
2. Test functionality within Teams environment
3. Share with colleagues for testing

### Available Channels
- Microsoft Teams
- Web chat widget
- Mobile apps
- Facebook Messenger
- Slack
- Custom websites
- Power Apps

## Understanding Knowledge Sources

### Public Websites (Bing Search)
- **Use Case**: External customer support, general knowledge
- **How it Works**: Uses Bing to search specified websites
- **Limitations**: Public content only, rate limits apply

### SharePoint Sites
- **Use Case**: Internal knowledge bases, company documentation
- **Requirements**: Proper permissions and authentication
- **Benefits**: Secure, organization-specific content

### Uploaded Files
- **Supported Formats**: Word, PowerPoint, PDF, text files
- **Best Practices**: 
  - Keep files current and well-structured
  - Use clear headings and organization
  - Limit file size for better performance

### Azure AI Search
- **Use Case**: Advanced scenarios requiring custom indexing
- **Benefits**: More control over search and indexing
- **Requirements**: Azure subscription and AI Search service

## Best Practices for Getting Started

### 1. Start Simple
- Begin with basic Q&A scenarios
- Use single knowledge sources initially
- Test thoroughly before adding complexity

### 2. Knowledge Source Quality
- Ensure content is current and accurate
- Use well-structured documents
- Implement proper access controls

### 3. Clear Instructions
- Write specific, actionable instructions
- Use examples in your guidance
- Test different phrasings and approaches

### 4. Iterative Development
- Start with basic functionality
- Test with real users
- Gather feedback and iterate

### 5. Security Considerations
- Implement appropriate authentication
- Review knowledge source permissions
- Monitor for sensitive information exposure

## Troubleshooting Common Issues

### Agent Not Responding Correctly
1. **Check Knowledge Sources**
   - Verify indexing completed
   - Ensure content is accessible
   - Review source quality

2. **Review Instructions**
   - Make instructions more specific
   - Add examples of expected behavior
   - Test different prompt variations

### Poor Answer Quality
1. **Improve Knowledge Sources**
   - Add more relevant content
   - Structure documents better
   - Remove outdated information

2. **Adjust Generative AI Settings**
   - Modify moderation levels
   - Update content filters
   - Fine-tune response parameters

### Deployment Issues
1. **Check Permissions**
   - Verify publish rights
   - Review channel permissions
   - Confirm license allocation

2. **Environment Issues**
   - Check Power Platform environment
   - Verify connector availability
   - Review security settings

## Knowledge Check

### Quiz Questions
1. What are the two main capabilities of Microsoft Copilot Studio?
2. What's the key difference between conversational and autonomous agents?
3. Name three types of knowledge sources supported in Copilot Studio.
4. What component is unique to autonomous agents?
5. Which channels can you deploy custom copilots to?

### Practical Exercises
1. Create a copilot for your organization's FAQ using SharePoint as a knowledge source
2. Test the same copilot with different knowledge sources and compare results
3. Deploy your copilot to two different channels and document the differences

## Summary

In this module, you learned:
- The fundamental concepts of Microsoft Copilot Studio
- How to create your first copilot using generative answers
- The difference between conversational and autonomous agents
- Various knowledge sources and their use cases
- Basic deployment and troubleshooting techniques

### Next Steps
In Module 2, we'll dive deeper into agent development, learning how to create both conversational and autonomous agents with more complex scenarios and custom actions.

## Additional Resources

- [Copilot Studio Documentation](https://learn.microsoft.com/en-us/microsoft-copilot-studio/)
- [Knowledge Sources Overview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/nlu-gpt-knowledge-source)
- [Power Platform Learning Path](https://learn.microsoft.com/en-us/training/paths/work-power-platform/)
- [Community Samples](https://aka.ms/community/samples)

---

*This module is part of the Microsoft Copilot Studio Complete Implementation Guide. Continue to Module 2 for advanced agent development techniques.*