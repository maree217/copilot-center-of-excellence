# Module 7: SharePoint Integration

## Learning Objectives
By the end of this module, you will be able to:
- Integrate Copilot Studio agents with SharePoint sites and content
- Create SharePoint Embedded copilot experiences
- Build agents that work with SharePoint lists and document libraries
- Implement "Click to Copilot" functionality for SharePoint sites
- Deploy copilots in SharePoint Framework (SPFx) web parts
- Create intelligent document processing workflows with SharePoint

## Prerequisites
- Completion of Modules 1-6
- Understanding of SharePoint Online concepts and architecture
- Basic knowledge of SharePoint Framework (SPFx) development
- Familiarity with SharePoint permissions and site management

## Overview

SharePoint integration represents one of the most powerful scenarios for Copilot Studio agents, enabling organizations to transform their content management and collaboration experiences. This module covers comprehensive SharePoint integration patterns, from simple knowledge source connections to sophisticated embedded copilot experiences.

## SharePoint Integration Architecture

### Integration Patterns

```
SharePoint Content → Copilot Studio Agent → User Experience
       ↓                    ↓                    ↓
  Knowledge Sources    Actions & Flows    Multiple Channels
       ↓                    ↓                    ↓
  Document Libraries   List Operations     Teams, Web, SPFx
```

### SharePoint as Knowledge Source vs. Action Target

#### Knowledge Source Integration
- **Purpose**: Provides context and information for agent responses
- **Data Flow**: SharePoint → Copilot Studio → User
- **Use Cases**: Document search, policy questions, knowledge base
- **Authentication**: User's SharePoint permissions apply

#### Action Target Integration
- **Purpose**: Enables agent to create/modify SharePoint content
- **Data Flow**: User → Copilot Studio → SharePoint
- **Use Cases**: List item creation, document upload, workflow initiation
- **Authentication**: Service account or user delegation required

## Lab 1: SharePoint Knowledge Source Integration

### Scenario: Company Policy Assistant
Build an agent that can answer questions about company policies stored in SharePoint document libraries.

### Step 1: Prepare SharePoint Environment

1. **Create Document Library Structure**
   ```
   Corporate Policies Site
   ├── HR Policies/
   │   ├── Employee Handbook.pdf
   │   ├── Leave Policy.docx
   │   └── Remote Work Guidelines.pdf
   ├── IT Policies/
   │   ├── Security Policy.pdf
   │   ├── Device Usage Policy.docx
   │   └── Data Protection Guidelines.pdf
   └── Finance Policies/
       ├── Expense Policy.pdf
       ├── Procurement Guidelines.docx
       └── Budget Process.pdf
   ```

2. **Configure Site Permissions**
   - Ensure appropriate user groups have read access
   - Configure authentication for service accounts if needed
   - Set up proper permission inheritance

3. **Optimize Content for Search**
   - Add metadata columns to document libraries
   - Use consistent naming conventions
   - Add rich descriptions to documents

### Step 2: Configure SharePoint Knowledge Source

1. **Add SharePoint as Knowledge Source**
   - In Copilot Studio, go to **"Knowledge"** tab
   - Click **"+ Add knowledge"**
   - Select **"SharePoint"**
   - Choose **"Use authentication"** for secure access

2. **Configure Site Connection**
   - Site URL: `https://yourcompany.sharepoint.com/sites/corporatepolicies`
   - Authentication method: **"User authentication"**
   - Content types to include: **"All document libraries"**
   - Specify folders if needed: `/HR Policies`, `/IT Policies`, `/Finance Policies`

3. **Advanced Configuration**
   ```json
   {
     "sharepointConfig": {
       "siteUrl": "https://yourcompany.sharepoint.com/sites/corporatepolicies",
       "includeSubsites": true,
       "contentTypes": ["Document", "Folder"],
       "fileTypes": [".pdf", ".docx", ".xlsx", ".pptx"],
       "excludePaths": ["/Archive", "/Templates"],
       "metadataFields": ["Department", "PolicyType", "ReviewDate", "Version"],
       "maxDocumentSize": "10MB",
       "refreshFrequency": "daily"
     }
   }
   ```

### Step 3: Create Policy Assistant Agent

1. **Configure Agent Settings**
   - Name: "Corporate Policy Assistant"
   - Description: "Helps employees find and understand company policies"
   - Instructions:
   ```markdown
   You are a Corporate Policy Assistant that helps employees understand company policies and procedures.

   KNOWLEDGE SOURCES:
   - HR Policies: Employee handbook, leave policies, remote work guidelines
   - IT Policies: Security requirements, device usage, data protection
   - Finance Policies: Expense procedures, procurement guidelines, budgets

   CAPABILITIES:
   - Answer policy-related questions with specific citations
   - Provide step-by-step guidance for procedures
   - Reference relevant policy sections and documents
   - Suggest related policies when appropriate

   RESPONSE GUIDELINES:
   - Always cite the specific policy document and section
   - Provide direct quotes from policies when relevant
   - If policy is unclear or outdated, recommend contacting relevant department
   - Use clear, professional language
   - Structure responses with headings and bullet points for readability

   EXAMPLES:
   User: "What's the remote work policy?"
   Response: Provide policy details with document citations

   User: "How do I request time off?"
   Response: Step-by-step process from leave policy document
   ```

2. **Test Knowledge Integration**
   - Ask: "What's the company's remote work policy?"
   - Ask: "How do I submit an expense report?"
   - Ask: "What are the password requirements?"
   - Ask: "Can I use personal devices for work?"

### Step 4: Enhance with SharePoint Actions

1. **Add SharePoint List Integration**
   - Create "Policy Feedback" list in SharePoint
   - Add columns: Employee Name, Policy Area, Feedback, Date
   - Configure list permissions

2. **Create Feedback Collection Action**
   ```json
   {
     "action": "SharePoint Create Item",
     "list": "Policy Feedback",
     "siteUrl": "https://yourcompany.sharepoint.com/sites/corporatepolicies",
     "fields": {
       "Title": "Policy Feedback from {{userEmail}}",
       "Employee": "{{userDisplayName}}",
       "PolicyArea": "{{policyArea}}",
       "Feedback": "{{feedbackText}}",
       "DateSubmitted": "{{currentDate}}"
     }
   }
   ```

3. **Update Agent Instructions**
   ```markdown
   ADDITIONAL CAPABILITIES:
   - Collect feedback on policies using 'Submit Policy Feedback' action
   - Track common questions for policy improvement

   FEEDBACK COLLECTION:
   When users express confusion or suggest improvements:
   - Use 'Submit Policy Feedback' action to record their input
   - Confirm feedback submission
   - Provide contact information for follow-up if needed
   ```

## Lab 2: SharePoint Embedded Copilot Experience

### Scenario: Project Document Assistant
Create an embedded copilot that appears directly in SharePoint project sites to assist with document management.

### Step 1: Design SharePoint Framework Solution

1. **Create SPFx Project**
   ```bash
   # Install SPFx generator (if not already installed)
   npm install -g @microsoft/generator-sharepoint

   # Create new SPFx project
   yo @microsoft/sharepoint

   # Project configuration:
   # - Solution name: sharepoint-copilot-webpart
   # - Target: SharePoint Online only
   # - Framework: React
   # - WebPart name: CopilotAssistant
   ```

2. **Configure Web Part Manifest**
   ```json
   {
     "$schema": "https://developer.microsoft.com/json-schemas/spfx/client-side-web-part-manifest.schema.json",
     "id": "copilot-assistant-webpart-id",
     "alias": "CopilotAssistantWebPart",
     "componentType": "WebPart",
     "version": "*",
     "manifestVersion": 2,
     "requiresCustomScript": false,
     "supportedHosts": ["SharePointWebPart"],
     "preconfiguredEntries": [{
       "groupId": "5c03119e-3074-46fd-976b-c60198311f70",
       "group": { "default": "Advanced" },
       "title": { "default": "Project Copilot Assistant" },
       "description": { "default": "AI assistant for project document management" },
       "officeFabricIconFontName": "Robot",
       "properties": {
         "copilotEndpoint": "",
         "siteContext": true,
         "enableDocumentAnalysis": true
       }
     }]
   }
   ```

### Step 2: Build React Copilot Interface

1. **Create Chat Component**
   ```tsx
   // src/webparts/copilotAssistant/components/CopilotChat.tsx
   import * as React from 'react';
   import { useState, useEffect, useRef } from 'react';
   import { 
     Stack, 
     TextField, 
     PrimaryButton, 
     MessageBar, 
     MessageBarType,
     Spinner,
     Text
   } from '@fluentui/react';
   import { WebPartContext } from '@microsoft/sp-webpart-base';

   interface ICopilotChatProps {
     context: WebPartContext;
     copilotEndpoint: string;
   }

   interface IChatMessage {
     id: string;
     text: string;
     sender: 'user' | 'copilot';
     timestamp: Date;
     isLoading?: boolean;
   }

   export const CopilotChat: React.FC<ICopilotChatProps> = ({
     context,
     copilotEndpoint
   }) => {
     const [messages, setMessages] = useState<IChatMessage[]>([
       {
         id: '1',
         text: 'Hello! I\'m your Project Document Assistant. I can help you find documents, answer questions about project content, and assist with document management tasks.',
         sender: 'copilot',
         timestamp: new Date()
       }
     ]);
     const [inputValue, setInputValue] = useState('');
     const [isLoading, setIsLoading] = useState(false);
     const messagesEndRef = useRef<HTMLDivElement>(null);

     const scrollToBottom = () => {
       messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
     };

     useEffect(() => {
       scrollToBottom();
     }, [messages]);

     const sendMessage = async () => {
       if (!inputValue.trim() || isLoading) return;

       const userMessage: IChatMessage = {
         id: Date.now().toString(),
         text: inputValue,
         sender: 'user',
         timestamp: new Date()
       };

       setMessages(prev => [...prev, userMessage]);
       setInputValue('');
       setIsLoading(true);

       try {
         const response = await callCopilotAPI(inputValue, context);
         
         const copilotMessage: IChatMessage = {
           id: (Date.now() + 1).toString(),
           text: response,
           sender: 'copilot',
           timestamp: new Date()
         };

         setMessages(prev => [...prev, copilotMessage]);
       } catch (error) {
         const errorMessage: IChatMessage = {
           id: (Date.now() + 1).toString(),
           text: 'Sorry, I encountered an error. Please try again.',
           sender: 'copilot',
           timestamp: new Date()
         };
         setMessages(prev => [...prev, errorMessage]);
       } finally {
         setIsLoading(false);
       }
     };

     const callCopilotAPI = async (message: string, spContext: WebPartContext): Promise<string> => {
       const userInfo = {
         email: spContext.pageContext.user.email,
         displayName: spContext.pageContext.user.displayName,
         loginName: spContext.pageContext.user.loginName
       };

       const siteInfo = {
         siteUrl: spContext.pageContext.web.absoluteUrl,
         webTitle: spContext.pageContext.web.title,
         listId: spContext.pageContext.list?.id?.toString(),
         listTitle: spContext.pageContext.list?.title
       };

       const requestBody = {
         message: message,
         user: userInfo,
         context: {
           site: siteInfo,
           timestamp: new Date().toISOString(),
           source: 'sharepoint-spfx'
         }
       };

       const response = await fetch(`${copilotEndpoint}/api/chat`, {
         method: 'POST',
         headers: {
           'Content-Type': 'application/json',
           'Authorization': `Bearer ${await spContext.aadTokenProviderFactory.getTokenProvider().getToken('api://your-copilot-app-id')}`
         },
         body: JSON.stringify(requestBody)
       });

       if (!response.ok) {
         throw new Error(`HTTP error! status: ${response.status}`);
       }

       const data = await response.json();
       return data.response || 'No response received';
     };

     const handleKeyPress = (event: React.KeyboardEvent) => {
       if (event.key === 'Enter' && !event.shiftKey) {
         event.preventDefault();
         sendMessage();
       }
     };

     return (
       <Stack tokens={{ childrenGap: 10 }} styles={{ root: { height: '400px', padding: '10px' } }}>
         <Stack.Item grow styles={{ root: { overflowY: 'auto', border: '1px solid #edebe9', padding: '10px' } }}>
           {messages.map((message) => (
             <Stack
               key={message.id}
               horizontal={message.sender === 'user'}
               horizontalAlign={message.sender === 'user' ? 'end' : 'start'}
               tokens={{ childrenGap: 5 }}
               styles={{ root: { marginBottom: '10px' } }}
             >
               <Stack
                 styles={{
                   root: {
                     backgroundColor: message.sender === 'user' ? '#0078d4' : '#f3f2f1',
                     color: message.sender === 'user' ? 'white' : 'black',
                     padding: '8px 12px',
                     borderRadius: '8px',
                     maxWidth: '80%'
                   }
                 }}
               >
                 <Text variant="medium">{message.text}</Text>
                 <Text variant="tiny" styles={{ root: { opacity: 0.7 } }}>
                   {message.timestamp.toLocaleTimeString()}
                 </Text>
               </Stack>
             </Stack>
           ))}
           {isLoading && (
             <Stack horizontal horizontalAlign="start" tokens={{ childrenGap: 10 }}>
               <Spinner size="small" />
               <Text>Assistant is thinking...</Text>
             </Stack>
           )}
           <div ref={messagesEndRef} />
         </Stack.Item>

         <Stack horizontal tokens={{ childrenGap: 10 }}>
           <TextField
             placeholder="Ask me about project documents..."
             value={inputValue}
             onChange={(_, newValue) => setInputValue(newValue || '')}
             onKeyPress={handleKeyPress}
             disabled={isLoading}
             multiline
             rows={2}
             styles={{ root: { flexGrow: 1 } }}
           />
           <PrimaryButton
             text="Send"
             onClick={sendMessage}
             disabled={!inputValue.trim() || isLoading}
             styles={{ root: { alignSelf: 'flex-end' } }}
           />
         </Stack>
       </Stack>
     );
   };
   ```

2. **Create Web Part Component**
   ```tsx
   // src/webparts/copilotAssistant/components/CopilotAssistant.tsx
   import * as React from 'react';
   import { useState } from 'react';
   import { 
     Pivot, 
     PivotItem, 
     Stack, 
     Text, 
     MessageBar, 
     MessageBarType 
   } from '@fluentui/react';
   import { CopilotChat } from './CopilotChat';
   import { DocumentInsights } from './DocumentInsights';
   import { QuickActions } from './QuickActions';
   import { ICopilotAssistantProps } from './ICopilotAssistantProps';

   export const CopilotAssistant: React.FC<ICopilotAssistantProps> = ({
     context,
     copilotEndpoint,
     enableDocumentAnalysis
   }) => {
     const [selectedTab, setSelectedTab] = useState('chat');

     if (!copilotEndpoint) {
       return (
         <MessageBar messageBarType={MessageBarType.warning}>
           Please configure the Copilot endpoint in the web part properties.
         </MessageBar>
       );
     }

     return (
       <Stack tokens={{ childrenGap: 10 }}>
         <Text variant="large" styles={{ root: { fontWeight: 'bold' } }}>
           Project Assistant
         </Text>
         
         <Pivot
           selectedKey={selectedTab}
           onLinkClick={(item) => setSelectedTab(item?.props.itemKey || 'chat')}
         >
           <PivotItem headerText="Chat" itemKey="chat">
             <CopilotChat
               context={context}
               copilotEndpoint={copilotEndpoint}
             />
           </PivotItem>

           <PivotItem headerText="Quick Actions" itemKey="actions">
             <QuickActions
               context={context}
               copilotEndpoint={copilotEndpoint}
             />
           </PivotItem>

           {enableDocumentAnalysis && (
             <PivotItem headerText="Document Insights" itemKey="insights">
               <DocumentInsights
                 context={context}
                 copilotEndpoint={copilotEndpoint}
               />
             </PivotItem>
           )}
         </Pivot>
       </Stack>
     );
   };
   ```

### Step 3: Implement "Click to Copilot" Functionality

1. **Create SharePoint Site Template with Copilot**
   ```typescript
   // scripts/deployTemplate.ts
   import { sp } from '@pnp/sp/presets/all';
   import { SiteDesign, SiteScript } from '@pnp/sp/site-designs';

   export async function deployCopilotSiteTemplate(): Promise<void> {
     const siteScript = await sp.siteScripts.add({
       Title: "Project Site with Copilot",
       Description: "Creates project site with embedded copilot assistant",
       Content: JSON.stringify({
         "$schema": "https://developer.microsoft.com/json-schemas/sp/site-design-script-actions.schema.json",
         "actions": [
           {
             "verb": "createSPList",
             "listName": "Project Tasks",
             "templateType": 171,
             "subactions": [
               {
                 "verb": "addSPField",
                 "fieldType": "Choice",
                 "displayName": "Priority",
                 "choices": ["High", "Medium", "Low"]
               },
               {
                 "verb": "addSPField",
                 "fieldType": "Person",
                 "displayName": "Assigned To"
               }
             ]
           },
           {
             "verb": "addNavLink",
             "url": "/sites/templates/SitePages/CopilotHelper.aspx",
             "displayName": "AI Assistant",
             "isWebRelative": true
           },
           {
             "verb": "installSolution",
             "id": "copilot-assistant-webpart.sppkg"
           }
         ]
       })
     });

     const siteDesign = await sp.siteDesigns.add({
       Title: "Project Site with AI Assistant",
       Description: "Complete project site with embedded copilot functionality",
       SiteScriptIds: [siteScript.data.Id],
       WebTemplate: "64", // Team Site template
       PreviewImageUrl: "https://contoso.sharepoint.com/sites/templates/assets/copilot-preview.png",
       PreviewImageAltText: "Project site with AI assistant"
     });

     console.log(`Site design created with ID: ${siteDesign.data.Id}`);
   }
   ```

2. **Implement Context-Aware Copilot**
   ```typescript
   // src/services/SharePointContextService.ts
   import { WebPartContext } from '@microsoft/sp-webpart-base';
   import { sp } from '@pnp/sp/presets/all';

   export class SharePointContextService {
     private context: WebPartContext;

     constructor(context: WebPartContext) {
       this.context = context;
       sp.setup(context);
     }

     async getCurrentPageContext(): Promise<any> {
       try {
         const web = await sp.web();
         const list = this.context.pageContext.list;
         const currentUser = await sp.web.currentUser();

         const pageContext = {
           webInfo: {
             title: web.Title,
             url: web.Url,
             description: web.Description,
             template: web.WebTemplate
           },
           listInfo: list ? {
             id: list.id?.toString(),
             title: list.title,
             baseTemplate: list.baseTemplate
           } : null,
           userInfo: {
             id: currentUser.Id,
             email: currentUser.Email,
             title: currentUser.Title,
             loginName: currentUser.LoginName
           },
           pageInfo: {
             url: this.context.pageContext.web.absoluteUrl,
             title: document.title,
             isModernPage: this.context.pageContext.legacyPageContext === null
           }
         };

         return pageContext;
       } catch (error) {
         console.error('Error getting SharePoint context:', error);
         return null;
       }
     }

     async getRecentDocuments(count: number = 10): Promise<any[]> {
       try {
         const documents = await sp.web.lists
           .getByTitle('Documents')
           .items
           .select('Id', 'Title', 'FileRef', 'Modified', 'Author/Title')
           .expand('Author')
           .orderBy('Modified', false)
           .top(count)();

         return documents.map(doc => ({
           id: doc.Id,
           title: doc.Title,
           url: doc.FileRef,
           modified: doc.Modified,
           author: doc.Author?.Title
         }));
       } catch (error) {
         console.error('Error getting recent documents:', error);
         return [];
       }
     }

     async getListItems(listTitle: string, fields?: string[]): Promise<any[]> {
       try {
         const selectFields = fields?.join(',') || 'Id,Title,Created,Modified';
         
         const items = await sp.web.lists
           .getByTitle(listTitle)
           .items
           .select(selectFields)
           .orderBy('Modified', false)();

         return items;
       } catch (error) {
         console.error(`Error getting items from list ${listTitle}:`, error);
         return [];
       }
     }
   }
   ```

## Lab 3: Advanced Document Processing Integration

### Scenario: Intelligent Document Workflow
Create a comprehensive document processing system that analyzes documents, extracts metadata, and provides intelligent recommendations.

### Step 1: Build Document Analysis Pipeline

1. **Create Document Processing Flow**
   ```json
   {
     "flowName": "Intelligent Document Processing",
     "trigger": {
       "type": "SharePoint",
       "event": "When a file is created in a document library",
       "configuration": {
         "siteAddress": "https://contoso.sharepoint.com/sites/projects",
         "libraryName": "Project Documents"
       }
     },
     "actions": [
       {
         "name": "Extract_Document_Metadata",
         "type": "AI Builder",
         "operation": "Document Processing",
         "inputs": {
           "file": "@triggerBody()?['FileRef']"
         }
       },
       {
         "name": "Classify_Document_Type",
         "type": "Azure Cognitive Services",
         "operation": "Text Classification",
         "inputs": {
           "text": "@body('Extract_Document_Metadata')?['content']"
         }
       },
       {
         "name": "Generate_Document_Summary",
         "type": "Azure OpenAI",
         "inputs": {
           "prompt": "Summarize the following document: @{body('Extract_Document_Metadata')?['content']}"
         }
       },
       {
         "name": "Update_Document_Properties",
         "type": "SharePoint",
         "operation": "Update file properties",
         "inputs": {
           "site": "@triggerBody()?['SiteUrl']",
           "library": "@triggerBody()?['LibraryName']",
           "file": "@triggerBody()?['FileRef']",
           "properties": {
             "DocumentType": "@body('Classify_Document_Type')?['category']",
             "AIGeneratedSummary": "@body('Generate_Document_Summary')?['summary']",
             "ProcessedDate": "@utcnow()",
             "ConfidenceScore": "@body('Classify_Document_Type')?['confidence']"
           }
         }
       },
       {
         "name": "Trigger_Copilot_Notification",
         "type": "HTTP",
         "inputs": {
           "uri": "https://your-copilot-endpoint.com/api/document-processed",
           "method": "POST",
           "headers": {
             "Content-Type": "application/json",
             "Authorization": "Bearer @{variables('CopilotToken')}"
           },
           "body": {
             "documentUrl": "@triggerBody()?['FileRef']",
             "documentType": "@body('Classify_Document_Type')?['category']",
             "summary": "@body('Generate_Document_Summary')?['summary']",
             "siteUrl": "@triggerBody()?['SiteUrl']",
             "processedBy": "Intelligent Document Workflow"
           }
         }
       }
     ]
   }
   ```

2. **Create Document Analysis Actions for Copilot**
   ```typescript
   // Document analysis action for Copilot Studio
   export class DocumentAnalysisActions {
     static async analyzeDocument(
       context: TurnContext,
       state: ApplicationTurnState,
       data: { documentUrl: string; analysisType?: string }
     ): Promise<string> {
       try {
         // Call SharePoint API to get document
         const documentData = await this.getDocumentFromSharePoint(data.documentUrl);
         
         // Analyze document content
         const analysis = await this.performDocumentAnalysis(documentData, data.analysisType);
         
         let response = `## Document Analysis Results\n\n`;
         response += `**Document:** ${analysis.fileName}\n\n`;
         
         if (analysis.summary) {
           response += `### Summary\n${analysis.summary}\n\n`;
         }
         
         if (analysis.keyPoints && analysis.keyPoints.length > 0) {
           response += `### Key Points\n`;
           analysis.keyPoints.forEach((point: string, index: number) => {
             response += `${index + 1}. ${point}\n`;
           });
           response += `\n`;
         }
         
         if (analysis.metadata) {
           response += `### Document Details\n`;
           response += `- Type: ${analysis.metadata.documentType}\n`;
           response += `- Pages: ${analysis.metadata.pageCount}\n`;
           response += `- Word Count: ${analysis.metadata.wordCount}\n`;
           response += `- Language: ${analysis.metadata.language}\n\n`;
         }
         
         if (analysis.recommendations) {
           response += `### Recommendations\n${analysis.recommendations}\n`;
         }
         
         return response;
         
       } catch (error) {
         console.error('Error analyzing document:', error);
         return "Sorry, I couldn't analyze the document. Please make sure the document is accessible and try again.";
       }
     }

     static async searchDocuments(
       context: TurnContext,
       state: ApplicationTurnState,
       data: { query: string; siteUrl?: string; documentType?: string }
     ): Promise<string> {
       try {
         const searchResults = await this.searchSharePointDocuments({
           query: data.query,
           siteUrl: data.siteUrl,
           documentType: data.documentType
         });

         if (searchResults.length === 0) {
           return `No documents found for "${data.query}". Try using different search terms.`;
         }

         let response = `Found ${searchResults.length} document(s) matching "${data.query}":\n\n`;
         
         searchResults.slice(0, 10).forEach((doc: any, index: number) => {
           response += `${index + 1}. **[${doc.title}](${doc.url})**\n`;
           response += `   - Type: ${doc.documentType || 'Unknown'}\n`;
           response += `   - Modified: ${new Date(doc.modified).toLocaleDateString()}\n`;
           response += `   - Author: ${doc.author}\n`;
           
           if (doc.summary) {
             response += `   - Summary: ${doc.summary.substring(0, 100)}...\n`;
           }
           
           response += `\n`;
         });

         return response;
         
       } catch (error) {
         console.error('Error searching documents:', error);
         return "Sorry, I encountered an error while searching for documents. Please try again.";
       }
     }

     private static async getDocumentFromSharePoint(documentUrl: string): Promise<any> {
       // Implementation for SharePoint API call
       // Return document data including content, metadata, etc.
       return {
         url: documentUrl,
         content: '', // Document text content
         metadata: {}, // Document metadata
         fileName: ''
       };
     }

     private static async performDocumentAnalysis(documentData: any, analysisType?: string): Promise<any> {
       // Implementation for document analysis using AI services
       return {
         fileName: documentData.fileName,
         summary: '',
         keyPoints: [],
         metadata: {
           documentType: '',
           pageCount: 0,
           wordCount: 0,
           language: 'en'
         },
         recommendations: ''
       };
     }

     private static async searchSharePointDocuments(criteria: any): Promise<any[]> {
       // Implementation for SharePoint search
       return [];
     }
   }
   ```

### Step 2: Create Smart Document Templates

1. **Design Template System**
   ```typescript
   // Document template management system
   export class DocumentTemplateService {
     async createDocumentFromTemplate(
       templateId: string,
       parameters: any,
       targetLibrary: string
     ): Promise<string> {
       try {
         // Get template from SharePoint
         const template = await this.getTemplate(templateId);
         
         // Process template with AI
         const processedContent = await this.processTemplateWithAI(template, parameters);
         
         // Create document in SharePoint
         const documentUrl = await this.createSharePointDocument(
           targetLibrary,
           processedContent,
           parameters.fileName
         );
         
         return documentUrl;
         
       } catch (error) {
         console.error('Error creating document from template:', error);
         throw error;
       }
     }

     private async processTemplateWithAI(template: any, parameters: any): Promise<string> {
       const prompt = `
       Generate a professional document based on the following template and parameters:
       
       Template: ${template.content}
       
       Parameters:
       ${JSON.stringify(parameters, null, 2)}
       
       Instructions:
       - Replace placeholders with appropriate values
       - Maintain professional formatting
       - Ensure content is accurate and complete
       - Use appropriate business language
       `;

       // Call Azure OpenAI or other AI service
       const response = await this.callAIService(prompt);
       return response;
     }

     private async callAIService(prompt: string): Promise<string> {
       // Implementation for AI service call
       return '';
     }

     private async getTemplate(templateId: string): Promise<any> {
       // Get template from SharePoint template library
       return {
         id: templateId,
         content: '',
         metadata: {}
       };
     }

     private async createSharePointDocument(
       library: string,
       content: string,
       fileName: string
     ): Promise<string> {
       // Create document in SharePoint document library
       return `https://contoso.sharepoint.com/sites/projects/${library}/${fileName}`;
     }
   }
   ```

2. **Add Template Actions to Copilot**
   ```markdown
   DOCUMENT TEMPLATE CAPABILITIES:

   You can help users create documents from templates using the following actions:

   AVAILABLE TEMPLATES:
   - Project Charter Template
   - Meeting Minutes Template  
   - Technical Specification Template
   - Budget Request Template
   - Status Report Template

   TEMPLATE CREATION PROCESS:
   1. Ask user which template they want to use
   2. Collect required parameters for the template
   3. Use 'Create Document From Template' action
   4. Confirm document creation and provide link

   EXAMPLE INTERACTION:
   User: "I need to create a project charter"
   Assistant: "I can help you create a project charter. I'll need some information:
   - Project name
   - Project sponsor
   - Key objectives
   - Timeline
   - Budget estimate
   
   Please provide these details and I'll generate the document for you."
   ```

## Best Practices for SharePoint Integration

### 1. Performance Optimization

#### Content Indexing Strategy
```markdown
SHAREPOINT CONTENT OPTIMIZATION:
- Use metadata columns for better searchability
- Implement content approval workflows
- Organize content with consistent folder structures
- Use content types for document classification
- Regular cleanup of outdated content
```

#### Caching and Data Management
```typescript
export class SharePointCacheService {
  private cache = new Map<string, { data: any; expiry: Date }>();
  private defaultCacheDuration = 30 * 60 * 1000; // 30 minutes

  async getCachedData<T>(
    key: string,
    fetchFunction: () => Promise<T>,
    cacheDuration?: number
  ): Promise<T> {
    const cached = this.cache.get(key);
    const now = new Date();

    if (cached && cached.expiry > now) {
      return cached.data as T;
    }

    const data = await fetchFunction();
    const expiry = new Date(now.getTime() + (cacheDuration || this.defaultCacheDuration));
    
    this.cache.set(key, { data, expiry });
    return data;
  }
}
```

### 2. Security and Permissions

#### Permission-Aware Knowledge Sources
```markdown
SECURITY CONSIDERATIONS:
- Always use user authentication for SharePoint knowledge sources
- Respect SharePoint permissions in agent responses
- Implement proper error handling for access denied scenarios
- Regular auditing of agent access patterns
- Use service accounts with minimal required permissions
```

#### Data Privacy and Compliance
```typescript
export class SharePointComplianceService {
  async validateContentAccess(
    userContext: any,
    contentUrl: string
  ): Promise<boolean> {
    try {
      // Check if user has access to the content
      const hasAccess = await this.checkUserPermissions(userContext, contentUrl);
      
      // Log access attempt for audit
      await this.logAccessAttempt(userContext, contentUrl, hasAccess);
      
      return hasAccess;
      
    } catch (error) {
      console.error('Error validating content access:', error);
      return false;
    }
  }

  private async checkUserPermissions(userContext: any, contentUrl: string): Promise<boolean> {
    // Implementation for permission checking
    return true;
  }

  private async logAccessAttempt(userContext: any, contentUrl: string, success: boolean): Promise<void> {
    // Implementation for audit logging
  }
}
```

### 3. User Experience Optimization

#### Context-Aware Responses
```markdown
SHAREPOINT CONTEXT INTEGRATION:
- Use current site/page context in agent responses
- Provide direct links to relevant SharePoint content
- Suggest related documents and resources
- Integrate with SharePoint navigation and structure
```

#### Mobile and Responsive Design
```css
/* SPFx Web Part Mobile Optimization */
.copilot-container {
  max-width: 100%;
  padding: 16px;
}

@media (max-width: 768px) {
  .copilot-chat {
    height: 300px;
    font-size: 14px;
  }
  
  .copilot-input {
    flex-direction: column;
    gap: 8px;
  }
}
```

## Knowledge Check

### Practical Integration Projects

1. **Complete SharePoint Knowledge Base**
   - Set up multi-site knowledge source integration
   - Configure authentication and permissions
   - Test with different user roles
   - Implement feedback collection system

2. **Embedded Copilot Development**
   - Build SPFx web part with full chat interface
   - Implement context-aware functionality
   - Deploy to SharePoint app catalog
   - Test across different SharePoint environments

3. **Document Processing Workflow**
   - Create intelligent document processing pipeline
   - Integrate AI analysis and classification
   - Implement automated metadata extraction
   - Build document recommendation system

### Assessment Questions

1. What are the key differences between SharePoint as a knowledge source vs. action target?
2. How do you implement proper authentication for SharePoint-integrated agents?
3. What are the best practices for optimizing SharePoint content for AI agents?
4. How do you build context-aware copilot experiences in SharePoint?
5. What security considerations apply to SharePoint-integrated agents?

## Summary

In this module, you learned:
- Comprehensive SharePoint integration patterns for Copilot Studio
- How to create embedded copilot experiences using SharePoint Framework
- Advanced document processing and analysis workflows
- Best practices for security, performance, and user experience
- Real-world implementation strategies for SharePoint-based AI solutions

### Key Integration Benefits
- **Contextual Intelligence**: Agents understand SharePoint site and content context
- **Seamless User Experience**: Native integration within familiar SharePoint environment
- **Automated Workflows**: Intelligent document processing and content management
- **Enterprise Security**: Respect for SharePoint permissions and governance
- **Scalable Architecture**: Support for large-scale SharePoint deployments

### Next Steps
In Module 8, we'll explore Enterprise Security & Governance, focusing on the Copilot Control System and comprehensive security frameworks for enterprise AI deployments.

## Additional Resources

- [SharePoint Framework Documentation](https://learn.microsoft.com/en-us/sharepoint/dev/spfx/sharepoint-framework-overview)
- [SharePoint REST API Guide](https://learn.microsoft.com/en-us/sharepoint/dev/sp-add-ins/working-with-lists-and-list-items-with-rest)
- [Copilot Studio SharePoint Integration](https://learn.microsoft.com/en-us/microsoft-copilot-studio/nlu-gpt-knowledge-source-sharepoint)
- [PnP PowerShell for SharePoint](https://pnp.github.io/powershell/)
- [SharePoint Embedded Documentation](https://learn.microsoft.com/en-us/sharepoint/dev/embedded/)

---

*Continue to Module 8: Enterprise Security & Governance to learn about implementing comprehensive security and governance frameworks for enterprise Copilot deployments.*