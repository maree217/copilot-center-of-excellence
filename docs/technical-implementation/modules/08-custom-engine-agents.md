# Module 6: Custom Engine Agents

## Learning Objectives
By the end of this module, you will be able to:
- Understand the difference between declarative and custom engine agents
- Build custom engine agents using the Teams AI Library
- Implement Azure AI Search integration for enhanced knowledge retrieval
- Create sophisticated AI-powered applications with custom logic
- Deploy and manage custom engine agents in Microsoft Teams
- Integrate with Azure AI services for advanced capabilities

## Prerequisites
- Completion of Modules 1-5
- Basic understanding of Node.js, Python, or C# development
- Familiarity with Microsoft Teams development concepts
- Access to Azure subscription for AI services
- Understanding of REST APIs and web development

## Overview

Custom Engine Agents represent the most advanced form of AI agents, providing complete control over the AI processing pipeline. Unlike declarative agents that rely on Copilot Studio's built-in orchestration, custom engine agents use the Teams AI Library to create sophisticated applications with custom logic, advanced AI capabilities, and deep integration with Azure AI services.

## Understanding Custom Engine Agents

### Declarative vs. Custom Engine Agents

#### Declarative Agents
- **Built on**: Microsoft 365 Copilot platform
- **Configuration**: JSON manifest and instructions
- **AI Processing**: Handled by Microsoft's infrastructure
- **Deployment**: Through Copilot Studio or Teams Toolkit
- **Customization**: Limited to instructions and data sources
- **Best For**: Standard business scenarios, quick development

#### Custom Engine Agents
- **Built on**: Teams AI Library framework
- **Development**: Full programming environment (Node.js, Python, C#)
- **AI Processing**: Custom implementation with Azure AI
- **Deployment**: Custom hosting and infrastructure
- **Customization**: Complete control over AI pipeline
- **Best For**: Complex scenarios, specialized logic, advanced AI features

### Teams AI Library Architecture

```
Teams Interface → Custom Engine Agent → Azure AI Services
                        ↓
                  Custom Business Logic
                        ↓
                  External APIs & Data Sources
```

### Key Components

1. **Teams AI Library**: Framework for building conversational AI apps
2. **Azure OpenAI Service**: Large language model integration
3. **Azure AI Search**: Enhanced search and retrieval capabilities
4. **Custom Actions**: Programmable functions and integrations
5. **State Management**: Conversation and user state handling
6. **Authentication**: Custom security and identity management

## Lab 1: Building Your First Custom Engine Agent

### Scenario: Advanced Project Management Assistant
Create a sophisticated agent that manages projects with natural language understanding, task automation, and intelligent recommendations.

### Step 1: Environment Setup

1. **Install Prerequisites**
   ```bash
   # Install Node.js (version 18+)
   # Install Teams Toolkit for Visual Studio Code
   
   # Create new project
   mkdir project-assistant-agent
   cd project-assistant-agent
   npm init -y
   
   # Install Teams AI Library
   npm install @microsoft/teams-ai
   npm install @azure/openai
   npm install @azure/search-documents
   ```

2. **Install Development Tools**
   ```bash
   # Install development dependencies
   npm install --save-dev typescript @types/node nodemon
   npm install express restify botbuilder
   
   # Create TypeScript configuration
   npx tsc --init
   ```

3. **Configure Azure Resources**
   - Create Azure OpenAI service instance
   - Set up Azure AI Search service
   - Configure storage account for state management
   - Note connection strings and endpoints

### Step 2: Initialize the Teams AI Application

1. **Create Basic App Structure**
   ```typescript
   // src/index.ts
   import { TeamsActivityHandler, TurnContext, ActivityTypes } from 'botbuilder';
   import { 
     Application, 
     ActionPlanner, 
     OpenAIModel, 
     PromptManager 
   } from '@microsoft/teams-ai';
   import { OpenAIClient, AzureKeyCredential } from '@azure/openai';

   // Configure OpenAI client
   const openAIClient = new OpenAIClient(
     process.env.AZURE_OPENAI_ENDPOINT!,
     new AzureKeyCredential(process.env.AZURE_OPENAI_API_KEY!)
   );

   // Create the main application
   const app = new Application({
     ai: {
       planner: new ActionPlanner({
         model: new OpenAIModel({
           client: openAIClient,
           deploymentName: 'gpt-4', // or your deployment name
           apiVersion: '2024-02-01'
         }),
         prompts: new PromptManager('./src/prompts'),
         defaultPrompt: 'chat'
       })
     }
   });

   // Handle message activities
   app.messageActivity(async (context: TurnContext, state: any) => {
     console.log(`Received message: ${context.activity.text}`);
   });

   export default app;
   ```

2. **Create Application Configuration**
   ```typescript
   // src/config.ts
   export interface AppConfig {
     azureOpenAI: {
       endpoint: string;
       apiKey: string;
       deploymentName: string;
       apiVersion: string;
     };
     azureSearch: {
       endpoint: string;
       apiKey: string;
       indexName: string;
     };
     botSettings: {
       appId: string;
       appPassword: string;
     };
   }

   export const config: AppConfig = {
     azureOpenAI: {
       endpoint: process.env.AZURE_OPENAI_ENDPOINT!,
       apiKey: process.env.AZURE_OPENAI_API_KEY!,
       deploymentName: process.env.AZURE_OPENAI_DEPLOYMENT_NAME!,
       apiVersion: '2024-02-01'
     },
     azureSearch: {
       endpoint: process.env.AZURE_SEARCH_ENDPOINT!,
       apiKey: process.env.AZURE_SEARCH_API_KEY!,
       indexName: 'project-knowledge-index'
     },
     botSettings: {
       appId: process.env.BOT_ID!,
       appPassword: process.env.BOT_PASSWORD!
     }
   };
   ```

### Step 3: Create AI Prompts and Instructions

1. **Define System Prompt**
   ```yaml
   # src/prompts/chat/skprompt.txt
   You are an advanced Project Management Assistant that helps teams manage projects efficiently.

   CAPABILITIES:
   - Create and manage project tasks
   - Track project progress and deadlines  
   - Generate status reports and insights
   - Provide intelligent recommendations
   - Search project knowledge base
   - Schedule meetings and reminders

   AVAILABLE ACTIONS:
   - createTask: Create new project tasks
   - updateTaskStatus: Update task progress
   - searchProjects: Find project information
   - generateReport: Create project status reports
   - scheduleReminder: Set up task reminders
   - analyzeRisks: Identify project risks

   CONTEXT:
   Current user: {{$userProfile.displayName}}
   User role: {{$userProfile.jobTitle}}
   Current projects: {{$userProjects}}

   INSTRUCTIONS:
   - Always be helpful and professional
   - Use context from previous conversations
   - Provide actionable recommendations
   - Ask clarifying questions when needed
   - Reference specific project data when available

   User message: {{$input}}
   ```

2. **Create Configuration File**
   ```json
   // src/prompts/chat/config.json
   {
     "schema": "1.0",
     "type": "completion",
     "description": "Project management chat prompt",
     "completion": {
       "max_tokens": 1000,
       "temperature": 0.7,
       "top_p": 0.9,
       "presence_penalty": 0.1,
       "frequency_penalty": 0.1
     }
   }
   ```

### Step 4: Implement Custom Actions

1. **Create Task Management Actions**
   ```typescript
   // src/actions/taskActions.ts
   import { TurnContext } from 'botbuilder';
   import { PredictedDoCommand, ApplicationTurnState } from '@microsoft/teams-ai';

   interface TaskData {
     title: string;
     description: string;
     assignee: string;
     dueDate: string;
     priority: 'low' | 'medium' | 'high';
     projectId: string;
   }

   export class TaskActions {
     /**
      * Create a new project task
      */
     static async createTask(
       context: TurnContext, 
       state: ApplicationTurnState, 
       data: TaskData
     ): Promise<string> {
       try {
         // Validate input data
         if (!data.title || !data.assignee) {
           return "I need at least a task title and assignee to create a task.";
         }

         // Generate unique task ID
         const taskId = `TASK-${Date.now()}`;
         
         // Create task object
         const task = {
           id: taskId,
           title: data.title,
           description: data.description || '',
           assignee: data.assignee,
           dueDate: data.dueDate ? new Date(data.dueDate) : null,
           priority: data.priority || 'medium',
           projectId: data.projectId,
           status: 'todo',
           createdBy: context.activity.from.name,
           createdAt: new Date(),
           updatedAt: new Date()
         };

         // Save to database/storage
         await saveTaskToStorage(task);
         
         // Send notification to assignee
         await notifyTaskAssignee(task);

         return `✅ Task "${task.title}" created successfully with ID ${taskId}. Assigned to ${task.assignee}.`;
         
       } catch (error) {
         console.error('Error creating task:', error);
         return "Sorry, I encountered an error while creating the task. Please try again.";
       }
     }

     /**
      * Update task status
      */
     static async updateTaskStatus(
       context: TurnContext,
       state: ApplicationTurnState,
       data: { taskId: string; status: string; notes?: string }
     ): Promise<string> {
       try {
         // Retrieve existing task
         const existingTask = await getTaskFromStorage(data.taskId);
         if (!existingTask) {
           return `Task with ID ${data.taskId} not found.`;
         }

         // Update task status
         existingTask.status = data.status;
         existingTask.updatedAt = new Date();
         existingTask.updatedBy = context.activity.from.name;
         
         if (data.notes) {
           existingTask.notes = data.notes;
         }

         // Save updated task
         await saveTaskToStorage(existingTask);

         // Log status change
         await logTaskStatusChange(existingTask, data.status);

         return `✅ Task "${existingTask.title}" status updated to ${data.status}.`;
         
       } catch (error) {
         console.error('Error updating task:', error);
         return "Sorry, I encountered an error while updating the task status.";
       }
     }

     /**
      * Search project tasks
      */
     static async searchTasks(
       context: TurnContext,
       state: ApplicationTurnState,
       data: { query: string; projectId?: string; status?: string }
     ): Promise<string> {
       try {
         const searchResults = await searchTasksInStorage({
           query: data.query,
           projectId: data.projectId,
           status: data.status
         });

         if (searchResults.length === 0) {
           return "No tasks found matching your search criteria.";
         }

         let response = `Found ${searchResults.length} task(s):\n\n`;
         
         searchResults.forEach((task, index) => {
           response += `${index + 1}. **${task.title}**\n`;
           response += `   - ID: ${task.id}\n`;
           response += `   - Status: ${task.status}\n`;
           response += `   - Assignee: ${task.assignee}\n`;
           response += `   - Due: ${task.dueDate ? task.dueDate.toDateString() : 'Not set'}\n\n`;
         });

         return response;
         
       } catch (error) {
         console.error('Error searching tasks:', error);
         return "Sorry, I encountered an error while searching for tasks.";
       }
     }
   }

   // Helper functions for data persistence
   async function saveTaskToStorage(task: any): Promise<void> {
     // Implementation depends on your storage solution
     // Could be Azure Table Storage, Cosmos DB, SQL Database, etc.
     console.log('Saving task to storage:', task);
   }

   async function getTaskFromStorage(taskId: string): Promise<any> {
     // Retrieve task from storage
     console.log('Retrieving task from storage:', taskId);
     return null; // Placeholder
   }

   async function searchTasksInStorage(criteria: any): Promise<any[]> {
     // Search tasks based on criteria
     console.log('Searching tasks:', criteria);
     return []; // Placeholder
   }

   async function notifyTaskAssignee(task: any): Promise<void> {
     // Send notification to task assignee
     console.log('Notifying assignee:', task.assignee);
   }

   async function logTaskStatusChange(task: any, newStatus: string): Promise<void> {
     // Log status change for audit trail
     console.log('Logging status change:', task.id, newStatus);
   }
   ```

2. **Register Actions with the Application**
   ```typescript
   // src/index.ts (updated)
   import { TaskActions } from './actions/taskActions';

   // Register custom actions
   app.ai.action('createTask', async (context, state, data) => {
     return await TaskActions.createTask(context, state, data);
   });

   app.ai.action('updateTaskStatus', async (context, state, data) => {
     return await TaskActions.updateTaskStatus(context, state, data);
   });

   app.ai.action('searchTasks', async (context, state, data) => {
     return await TaskActions.searchTasks(context, state, data);
   });
   ```

### Step 5: Integrate Azure AI Search

1. **Set up Azure AI Search Integration**
   ```typescript
   // src/services/searchService.ts
   import { SearchClient, AzureKeyCredential } from '@azure/search-documents';
   import { config } from '../config';

   export class ProjectSearchService {
     private searchClient: SearchClient;

     constructor() {
       this.searchClient = new SearchClient(
         config.azureSearch.endpoint,
         config.azureSearch.indexName,
         new AzureKeyCredential(config.azureSearch.apiKey)
       );
     }

     /**
      * Search project knowledge base
      */
     async searchProjectKnowledge(query: string, filters?: any): Promise<any[]> {
       try {
         const searchOptions = {
           searchText: query,
           top: 10,
           includeTotalCount: true,
           facets: ['category', 'projectType', 'status'],
           highlightFields: ['title', 'content', 'description'],
           select: ['id', 'title', 'content', 'category', 'projectType', 'lastModified']
         };

         if (filters) {
           searchOptions['filter'] = this.buildFilterExpression(filters);
         }

         const searchResults = await this.searchClient.search(searchOptions.searchText, searchOptions);
         const results = [];

         for await (const result of searchResults.results) {
           results.push({
             document: result.document,
             score: result.score,
             highlights: result.highlights
           });
         }

         return results;
         
       } catch (error) {
         console.error('Error searching project knowledge:', error);
         throw error;
       }
     }

     /**
      * Get semantic search results
      */
     async getSemanticSearchResults(query: string, projectContext?: string): Promise<any[]> {
       try {
         const searchOptions = {
           searchText: query,
           queryType: 'semantic' as const,
           semanticConfigurationName: 'project-semantic-config',
           captions: 'extractive',
           answers: 'extractive',
           top: 5
         };

         if (projectContext) {
           searchOptions['filter'] = `projectId eq '${projectContext}'`;
         }

         const results = await this.searchClient.search(searchOptions.searchText, searchOptions);
         const semanticResults = [];

         for await (const result of results.results) {
           semanticResults.push({
             document: result.document,
             score: result.score,
             captions: result.captions,
             highlights: result.highlights
           });
         }

         return semanticResults;
         
       } catch (error) {
         console.error('Error with semantic search:', error);
         throw error;
       }
     }

     /**
      * Index new project document
      */
     async indexProjectDocument(document: any): Promise<void> {
       try {
         const indexingResult = await this.searchClient.uploadDocuments([document]);
         console.log('Document indexed successfully:', indexingResult);
         
       } catch (error) {
         console.error('Error indexing document:', error);
         throw error;
       }
     }

     private buildFilterExpression(filters: any): string {
       const filterParts = [];
       
       if (filters.category) {
         filterParts.push(`category eq '${filters.category}'`);
       }
       
       if (filters.projectType) {
         filterParts.push(`projectType eq '${filters.projectType}'`);
       }
       
       if (filters.dateRange) {
         filterParts.push(`lastModified ge ${filters.dateRange.start} and lastModified le ${filters.dateRange.end}`);
       }
       
       return filterParts.join(' and ');
     }
   }
   ```

2. **Add Search Action**
   ```typescript
   // src/actions/searchActions.ts
   import { TurnContext } from 'botbuilder';
   import { ApplicationTurnState } from '@microsoft/teams-ai';
   import { ProjectSearchService } from '../services/searchService';

   export class SearchActions {
     private static searchService = new ProjectSearchService();

     /**
      * Search project knowledge base
      */
     static async searchProjectKnowledge(
       context: TurnContext,
       state: ApplicationTurnState,
       data: { query: string; projectId?: string; category?: string }
     ): Promise<string> {
       try {
         const filters = {
           category: data.category,
           projectId: data.projectId
         };

         const searchResults = await this.searchService.searchProjectKnowledge(
           data.query, 
           filters
         );

         if (searchResults.length === 0) {
           return `No documents found for "${data.query}". Try using different search terms.`;
         }

         let response = `Found ${searchResults.length} relevant document(s):\n\n`;
         
         searchResults.slice(0, 5).forEach((result, index) => {
           const doc = result.document;
           response += `${index + 1}. **${doc.title}**\n`;
           response += `   Category: ${doc.category}\n`;
           response += `   Relevance: ${(result.score * 100).toFixed(1)}%\n`;
           
           if (result.highlights && result.highlights.content) {
             response += `   Preview: ${result.highlights.content[0]}...\n`;
           }
           
           response += `\n`;
         });

         return response;
         
       } catch (error) {
         console.error('Error in search action:', error);
         return "Sorry, I encountered an error while searching. Please try again.";
       }
     }

     /**
      * Get AI-powered insights from search results
      */
     static async getProjectInsights(
       context: TurnContext,
       state: ApplicationTurnState,
       data: { projectId: string; insightType?: string }
     ): Promise<string> {
       try {
         // Get project data using semantic search
         const projectQuery = `project insights analysis ${data.insightType || 'overview'}`;
         const semanticResults = await this.searchService.getSemanticSearchResults(
           projectQuery, 
           data.projectId
         );

         if (semanticResults.length === 0) {
           return "Not enough project data available for generating insights.";
         }

         // Combine search results for AI analysis
         const projectContext = semanticResults
           .map(result => result.document.content)
           .join('\n\n');

         // This would typically call your AI model to generate insights
         // For now, returning a structured response based on search results
         let insights = `## Project Insights for ${data.projectId}\n\n`;
         insights += `Based on analysis of ${semanticResults.length} project documents:\n\n`;
         
         // Add insight categories
         insights += `### Key Findings:\n`;
         semanticResults.slice(0, 3).forEach((result, index) => {
           if (result.captions && result.captions.length > 0) {
             insights += `- ${result.captions[0].text}\n`;
           }
         });

         insights += `\n### Recommendations:\n`;
         insights += `- Review high-priority tasks and deadlines\n`;
         insights += `- Consider resource allocation optimization\n`;
         insights += `- Monitor project risks and dependencies\n`;

         return insights;
         
       } catch (error) {
         console.error('Error generating project insights:', error);
         return "Sorry, I couldn't generate project insights at this time.";
       }
     }
   }
   ```

## Lab 2: Advanced AI Integration

### Scenario: Intelligent Document Analysis Agent
Build an agent that can analyze documents, extract insights, and answer questions about content using advanced AI capabilities.

### Step 1: Document Processing Pipeline

1. **Implement Document Analysis Service**
   ```typescript
   // src/services/documentAnalysisService.ts
   import { DocumentAnalysisClient, AzureKeyCredential } from '@azure/ai-form-recognizer';
   import { OpenAIClient } from '@azure/openai';

   export class DocumentAnalysisService {
     private documentClient: DocumentAnalysisClient;
     private openAIClient: OpenAIClient;

     constructor() {
       this.documentClient = new DocumentAnalysisClient(
         process.env.DOCUMENT_INTELLIGENCE_ENDPOINT!,
         new AzureKeyCredential(process.env.DOCUMENT_INTELLIGENCE_KEY!)
       );
       
       this.openAIClient = new OpenAIClient(
         process.env.AZURE_OPENAI_ENDPOINT!,
         new AzureKeyCredential(process.env.AZURE_OPENAI_API_KEY!)
       );
     }

     /**
      * Analyze document content and structure
      */
     async analyzeDocument(documentUrl: string, analysisType: string = 'layout'): Promise<any> {
       try {
         const poller = await this.documentClient.beginAnalyzeDocumentFromUrl(
           analysisType,
           documentUrl
         );
         
         const result = await poller.pollUntilDone();
         
         return {
           content: this.extractTextContent(result),
           tables: this.extractTables(result),
           keyValuePairs: this.extractKeyValuePairs(result),
           entities: this.extractEntities(result),
           structure: this.analyzeDocumentStructure(result)
         };
         
       } catch (error) {
         console.error('Error analyzing document:', error);
         throw error;
       }
     }

     /**
      * Generate AI-powered document summary
      */
     async generateDocumentSummary(documentContent: string, summaryType: 'brief' | 'detailed' = 'brief'): Promise<string> {
       try {
         const prompt = this.buildSummaryPrompt(documentContent, summaryType);
         
         const completion = await this.openAIClient.getCompletions(
           'gpt-4', // deployment name
           [prompt],
           {
             maxTokens: summaryType === 'brief' ? 150 : 500,
             temperature: 0.3,
             topP: 0.8
           }
         );

         return completion.choices[0]?.text?.trim() || 'Unable to generate summary';
         
       } catch (error) {
         console.error('Error generating summary:', error);
         throw error;
       }
     }

     /**
      * Extract key insights from document
      */
     async extractDocumentInsights(documentContent: string, focusArea?: string): Promise<any> {
       try {
         const insightPrompt = `
         Analyze the following document and extract key insights:
         
         ${focusArea ? `Focus on: ${focusArea}` : ''}
         
         Document Content:
         ${documentContent}
         
         Please provide:
         1. Key themes and topics
         2. Important facts and figures
         3. Action items or recommendations
         4. Risks or concerns mentioned
         5. Opportunities identified
         
         Format the response as structured JSON.
         `;

         const completion = await this.openAIClient.getCompletions(
           'gpt-4',
           [insightPrompt],
           {
             maxTokens: 800,
             temperature: 0.2
           }
         );

         const insightText = completion.choices[0]?.text?.trim();
         
         try {
           return JSON.parse(insightText || '{}');
         } catch {
           return { rawInsights: insightText };
         }
         
       } catch (error) {
         console.error('Error extracting insights:', error);
         throw error;
       }
     }

     private extractTextContent(result: any): string {
       return result.content || '';
     }

     private extractTables(result: any): any[] {
       return result.tables?.map((table: any) => ({
         rowCount: table.rowCount,
         columnCount: table.columnCount,
         cells: table.cells?.map((cell: any) => ({
           content: cell.content,
           rowIndex: cell.rowIndex,
           columnIndex: cell.columnIndex
         }))
       })) || [];
     }

     private extractKeyValuePairs(result: any): any[] {
       return result.keyValuePairs?.map((pair: any) => ({
         key: pair.key?.content,
         value: pair.value?.content,
         confidence: pair.confidence
       })) || [];
     }

     private extractEntities(result: any): any[] {
       return result.entities?.map((entity: any) => ({
         category: entity.category,
         subCategory: entity.subCategory,
         content: entity.content,
         confidence: entity.confidence
       })) || [];
     }

     private analyzeDocumentStructure(result: any): any {
       return {
         pageCount: result.pages?.length || 0,
         hasImages: result.pages?.some((page: any) => page.images?.length > 0) || false,
         hasTables: result.tables?.length > 0 || false,
         sections: result.sections?.length || 0
       };
     }

     private buildSummaryPrompt(content: string, type: string): string {
       const length = type === 'brief' ? '2-3 sentences' : '1-2 paragraphs';
       
       return `
       Please provide a ${type} summary of the following document in ${length}:
       
       ${content}
       
       Summary:
       `;
     }
   }
   ```

2. **Add Document Analysis Actions**
   ```typescript
   // src/actions/documentActions.ts
   import { TurnContext } from 'botbuilder';
   import { ApplicationTurnState } from '@microsoft/teams-ai';
   import { DocumentAnalysisService } from '../services/documentAnalysisService';

   export class DocumentActions {
     private static documentService = new DocumentAnalysisService();

     /**
      * Analyze uploaded document
      */
     static async analyzeDocument(
       context: TurnContext,
       state: ApplicationTurnState,
       data: { documentUrl: string; analysisType?: string; summaryType?: string }
     ): Promise<string> {
       try {
         // Analyze document structure and content
         const analysis = await this.documentService.analyzeDocument(
           data.documentUrl,
           data.analysisType || 'layout'
         );

         // Generate summary
         const summary = await this.documentService.generateDocumentSummary(
           analysis.content,
           (data.summaryType as 'brief' | 'detailed') || 'brief'
         );

         // Extract insights
         const insights = await this.documentService.extractDocumentInsights(
           analysis.content
         );

         let response = `## Document Analysis Complete\n\n`;
         
         response += `### Summary:\n${summary}\n\n`;
         
         response += `### Document Details:\n`;
         response += `- Pages: ${analysis.structure.pageCount}\n`;
         response += `- Tables: ${analysis.tables.length}\n`;
         response += `- Key-Value Pairs: ${analysis.keyValuePairs.length}\n`;
         response += `- Entities Found: ${analysis.entities.length}\n\n`;

         if (analysis.keyValuePairs.length > 0) {
           response += `### Key Information:\n`;
           analysis.keyValuePairs.slice(0, 5).forEach((pair: any) => {
             response += `- **${pair.key}:** ${pair.value}\n`;
           });
           response += `\n`;
         }

         if (insights.rawInsights) {
           response += `### Key Insights:\n${insights.rawInsights}\n`;
         }

         return response;
         
       } catch (error) {
         console.error('Error analyzing document:', error);
         return "Sorry, I encountered an error while analyzing the document. Please ensure the document URL is accessible and try again.";
       }
     }

     /**
      * Answer questions about document content
      */
     static async answerDocumentQuestion(
       context: TurnContext,
       state: ApplicationTurnState,
       data: { question: string; documentContext: string }
     ): Promise<string> {
       try {
         // This would use retrieval-augmented generation (RAG) pattern
         // to answer questions based on document content
         
         const questionPrompt = `
         Based on the following document content, please answer the user's question accurately and concisely.
         
         Document Content:
         ${data.documentContext}
         
         User Question: ${data.question}
         
         Answer:
         `;

         // Use Azure OpenAI to generate answer
         const openAIClient = new (require('@azure/openai').OpenAIClient)(
           process.env.AZURE_OPENAI_ENDPOINT!,
           new (require('@azure/openai').AzureKeyCredential)(process.env.AZURE_OPENAI_API_KEY!)
         );

         const completion = await openAIClient.getCompletions(
           'gpt-4',
           [questionPrompt],
           {
             maxTokens: 300,
             temperature: 0.1,
             topP: 0.9
           }
         );

         const answer = completion.choices[0]?.text?.trim();
         
         if (!answer) {
           return "I couldn't find a clear answer to your question in the document content.";
         }

         return answer;
         
       } catch (error) {
         console.error('Error answering document question:', error);
         return "Sorry, I encountered an error while processing your question about the document.";
       }
     }
   }
   ```

### Step 2: Advanced Conversation Management

1. **Implement Conversation State Management**
   ```typescript
   // src/services/conversationStateService.ts
   import { TurnContext, ConversationState, UserState, MemoryStorage } from 'botbuilder';

   export interface ConversationData {
     currentProject?: string;
     documentContext?: string;
     userPreferences?: {
       summaryLength: 'brief' | 'detailed';
       analysisDepth: 'basic' | 'advanced';
       notificationSettings: any;
     };
     conversationHistory?: Array<{
       timestamp: Date;
       userMessage: string;
       botResponse: string;
       context?: any;
     }>;
   }

   export class ConversationStateService {
     private conversationState: ConversationState;
     private userState: UserState;

     constructor() {
       const memoryStorage = new MemoryStorage();
       this.conversationState = new ConversationState(memoryStorage);
       this.userState = new UserState(memoryStorage);
     }

     /**
      * Get conversation data for current session
      */
     async getConversationData(context: TurnContext): Promise<ConversationData> {
       const conversationStateAccessor = this.conversationState.createProperty('conversationData');
       return await conversationStateAccessor.get(context, () => ({}));
     }

     /**
      * Update conversation data
      */
     async updateConversationData(context: TurnContext, data: Partial<ConversationData>): Promise<void> {
       const conversationStateAccessor = this.conversationState.createProperty('conversationData');
       const currentData = await conversationStateAccessor.get(context, () => ({}));
       
       Object.assign(currentData, data);
       await conversationStateAccessor.set(context, currentData);
       await this.conversationState.saveChanges(context);
     }

     /**
      * Add message to conversation history
      */
     async addToHistory(
       context: TurnContext, 
       userMessage: string, 
       botResponse: string, 
       additionalContext?: any
     ): Promise<void> {
       const data = await this.getConversationData(context);
       
       if (!data.conversationHistory) {
         data.conversationHistory = [];
       }

       data.conversationHistory.push({
         timestamp: new Date(),
         userMessage,
         botResponse,
         context: additionalContext
       });

       // Keep only last 20 messages to manage memory
       if (data.conversationHistory.length > 20) {
         data.conversationHistory = data.conversationHistory.slice(-20);
       }

       await this.updateConversationData(context, data);
     }

     /**
      * Get conversation context for AI model
      */
     async getConversationContext(context: TurnContext): Promise<string> {
       const data = await this.getConversationData(context);
       
       if (!data.conversationHistory || data.conversationHistory.length === 0) {
         return '';
       }

       let contextString = 'Recent conversation history:\n';
       const recentMessages = data.conversationHistory.slice(-5); // Last 5 exchanges
       
       recentMessages.forEach((message, index) => {
         contextString += `User: ${message.userMessage}\n`;
         contextString += `Assistant: ${message.botResponse}\n\n`;
       });

       return contextString;
     }
   }
   ```

2. **Enhanced Message Handling**
   ```typescript
   // src/handlers/messageHandler.ts
   import { TurnContext } from 'botbuilder';
   import { ApplicationTurnState } from '@microsoft/teams-ai';
   import { ConversationStateService } from '../services/conversationStateService';

   export class MessageHandler {
     private static stateService = new ConversationStateService();

     /**
      * Process incoming message with context awareness
      */
     static async handleMessage(
       context: TurnContext,
       state: ApplicationTurnState
     ): Promise<void> {
       try {
         const userMessage = context.activity.text;
         
         // Get conversation context
         const conversationContext = await this.stateService.getConversationContext(context);
         const conversationData = await this.stateService.getConversationData(context);

         // Enhance the state with conversation context
         state.temp = state.temp || {};
         state.temp.conversationContext = conversationContext;
         state.temp.currentProject = conversationData.currentProject;
         state.temp.documentContext = conversationData.documentContext;
         state.temp.userPreferences = conversationData.userPreferences;

         // The AI planner will use this enriched state to make better decisions
         console.log('Message processed with context:', {
           hasHistory: !!conversationContext,
           currentProject: conversationData.currentProject,
           hasDocumentContext: !!conversationData.documentContext
         });
         
       } catch (error) {
         console.error('Error handling message:', error);
       }
     }

     /**
      * Post-process AI response before sending
      */
     static async postProcessResponse(
       context: TurnContext,
       response: string,
       additionalContext?: any
     ): Promise<string> {
       try {
         // Add conversation to history
         await this.stateService.addToHistory(
           context,
           context.activity.text,
           response,
           additionalContext
         );

         // Format response for better readability
         return this.formatResponse(response);
         
       } catch (error) {
         console.error('Error post-processing response:', error);
         return response;
       }
     }

     private static formatResponse(response: string): string {
       // Add markdown formatting, clean up text, add structure
       return response
         .replace(/\*\*(.*?)\*\*/g, '**$1**') // Ensure bold formatting
         .replace(/\n\n\n+/g, '\n\n') // Clean up excessive line breaks
         .trim();
     }
   }
   ```

## Lab 3: Deployment and Management

### Scenario: Production-Ready Deployment
Deploy your custom engine agent to Azure with proper monitoring, scaling, and management capabilities.

### Step 1: Azure App Service Deployment

1. **Create Deployment Configuration**
   ```yaml
   # azure-pipelines.yml
   trigger:
   - main

   variables:
     azureServiceConnection: 'azure-service-connection'
     webAppName: 'project-assistant-agent'
     environmentName: 'production'
     resourceGroupName: 'rg-copilot-agents'

   stages:
   - stage: Build
     displayName: Build stage
     jobs:
     - job: Build
       displayName: Build
       pool:
         vmImage: ubuntu-latest
       
       steps:
       - task: NodeTool@0
         inputs:
           versionSpec: '18.x'
         displayName: 'Install Node.js'
       
       - script: |
           npm install
           npm run build
           npm run test
         displayName: 'npm install, build and test'
       
       - task: ArchiveFiles@2
         displayName: 'Archive files'
         inputs:
           rootFolderOrFile: '$(System.DefaultWorkingDirectory)'
           includeRootFolder: false
           archiveType: zip
           archiveFile: $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip
           replaceExistingArchive: true
       
       - upload: $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip
         artifact: drop

   - stage: Deploy
     displayName: Deploy stage
     dependsOn: Build
     condition: succeeded()
     
     jobs:
     - deployment: Deploy
       displayName: Deploy
       environment: $(environmentName)
       pool:
         vmImage: ubuntu-latest
       strategy:
         runOnce:
           deploy:
             steps:
             - task: AzureWebApp@1
               displayName: 'Azure Web App Deploy'
               inputs:
                 azureSubscription: $(azureServiceConnection)
                 appType: webAppLinux
                 appName: $(webAppName)
                 runtimeStack: 'NODE|18-lts'
                 package: $(Pipeline.Workspace)/drop/$(Build.BuildId).zip
                 startUpCommand: 'npm start'
   ```

2. **Configure Environment Variables**
   ```bash
   # Set up App Service configuration
   az webapp config appsettings set --resource-group rg-copilot-agents --name project-assistant-agent --settings \
     AZURE_OPENAI_ENDPOINT="https://your-openai.openai.azure.com/" \
     AZURE_OPENAI_API_KEY="your-openai-key" \
     AZURE_OPENAI_DEPLOYMENT_NAME="gpt-4" \
     AZURE_SEARCH_ENDPOINT="https://your-search.search.windows.net" \
     AZURE_SEARCH_API_KEY="your-search-key" \
     BOT_ID="your-bot-app-id" \
     BOT_PASSWORD="your-bot-app-password" \
     NODE_ENV="production"
   ```

### Step 2: Monitoring and Analytics

1. **Implement Application Insights Integration**
   ```typescript
   // src/services/telemetryService.ts
   import { TelemetryClient } from 'applicationinsights';
   import * as appInsights from 'applicationinsights';

   export class TelemetryService {
     private static client: TelemetryClient;

     static initialize(instrumentationKey: string): void {
       appInsights
         .setup(instrumentationKey)
         .setAutoDependencyCorrelation(true)
         .setAutoCollectRequests(true)
         .setAutoCollectPerformance(true)
         .setAutoCollectExceptions(true)
         .setAutoCollectDependencies(true)
         .setUseDiskRetryCaching(true)
         .start();

       this.client = appInsights.defaultClient;
     }

     /**
      * Track custom events
      */
     static trackEvent(name: string, properties?: any, metrics?: any): void {
       if (this.client) {
         this.client.trackEvent({
           name,
           properties,
           measurements: metrics
         });
       }
     }

     /**
      * Track user interactions
      */
     static trackUserInteraction(
       userId: string,
       action: string,
       properties?: any
     ): void {
       this.trackEvent('UserInteraction', {
         userId,
         action,
         timestamp: new Date().toISOString(),
         ...properties
       });
     }

     /**
      * Track AI model performance
      */
     static trackAIModelPerformance(
       model: string,
       operation: string,
       duration: number,
       success: boolean,
       properties?: any
     ): void {
       this.trackEvent('AIModelPerformance', {
         model,
         operation,
         success: success.toString(),
         ...properties
       }, {
         duration
       });
     }

     /**
      * Track business metrics
      */
     static trackBusinessMetric(
       metricName: string,
       value: number,
       properties?: any
     ): void {
       this.trackEvent('BusinessMetric', properties, {
         [metricName]: value
       });
     }

     /**
      * Track errors with context
      */
     static trackError(
       error: Error,
       properties?: any,
       severityLevel?: any
     ): void {
       if (this.client) {
         this.client.trackException({
           exception: error,
           properties,
           severityLevel
         });
       }
     }
   }
   ```

2. **Create Custom Dashboards**
   ```json
   {
     "dashboard": {
       "name": "Custom Engine Agent Analytics",
       "widgets": [
         {
           "title": "User Interactions",
           "type": "chart",
           "query": "customEvents | where name == 'UserInteraction' | summarize count() by bin(timestamp, 1h)",
           "visualization": "timechart"
         },
         {
           "title": "AI Model Performance",
           "type": "chart", 
           "query": "customEvents | where name == 'AIModelPerformance' | summarize avg(customMeasurements.duration) by tostring(customDimensions.model)",
           "visualization": "barchart"
         },
         {
           "title": "Error Rate",
           "type": "chart",
           "query": "exceptions | summarize count() by bin(timestamp, 1h)",
           "visualization": "timechart"
         },
         {
           "title": "Top User Actions",
           "type": "table",
           "query": "customEvents | where name == 'UserInteraction' | summarize count() by tostring(customDimensions.action) | top 10 by count_"
         }
       ]
     }
   }
   ```

### Step 3: Performance Optimization

1. **Implement Caching Strategy**
   ```typescript
   // src/services/cacheService.ts
   import { createClient, RedisClientType } from 'redis';

   export class CacheService {
     private client: RedisClientType;
     private defaultTTL = 3600; // 1 hour

     constructor() {
       this.client = createClient({
         url: process.env.REDIS_CONNECTION_STRING
       });
       
       this.client.on('error', (err) => {
         console.error('Redis Client Error:', err);
       });
       
       this.client.connect();
     }

     /**
      * Cache AI model responses
      */
     async cacheAIResponse(
       cacheKey: string,
       response: string,
       ttl: number = this.defaultTTL
     ): Promise<void> {
       try {
         await this.client.setEx(cacheKey, ttl, response);
       } catch (error) {
         console.error('Error caching AI response:', error);
       }
     }

     /**
      * Get cached AI response
      */
     async getCachedAIResponse(cacheKey: string): Promise<string | null> {
       try {
         return await this.client.get(cacheKey);
       } catch (error) {
         console.error('Error getting cached response:', error);
         return null;
       }
     }

     /**
      * Cache search results
      */
     async cacheSearchResults(
       query: string,
       results: any[],
       ttl: number = this.defaultTTL
     ): Promise<void> {
       try {
         const cacheKey = `search:${this.hashQuery(query)}`;
         await this.client.setEx(cacheKey, ttl, JSON.stringify(results));
       } catch (error) {
         console.error('Error caching search results:', error);
       }
     }

     /**
      * Get cached search results
      */
     async getCachedSearchResults(query: string): Promise<any[] | null> {
       try {
         const cacheKey = `search:${this.hashQuery(query)}`;
         const cached = await this.client.get(cacheKey);
         return cached ? JSON.parse(cached) : null;
       } catch (error) {
         console.error('Error getting cached search results:', error);
         return null;
       }
     }

     private hashQuery(query: string): string {
       // Simple hash function for cache keys
       return Buffer.from(query).toString('base64');
     }
   }
   ```

2. **Optimize AI Model Calls**
   ```typescript
   // src/services/optimizedAIService.ts
   import { TelemetryService } from './telemetryService';
   import { CacheService } from './cacheService';
   import { OpenAIClient } from '@azure/openai';

   export class OptimizedAIService {
     private openAIClient: OpenAIClient;
     private cacheService: CacheService;

     constructor() {
       this.openAIClient = new OpenAIClient(
         process.env.AZURE_OPENAI_ENDPOINT!,
         new (require('@azure/openai').AzureKeyCredential)(process.env.AZURE_OPENAI_API_KEY!)
       );
       this.cacheService = new CacheService();
     }

     /**
      * Get AI completion with caching and optimization
      */
     async getOptimizedCompletion(
       prompt: string,
       options: any = {},
       useCache: boolean = true
     ): Promise<string> {
       const startTime = Date.now();
       let cacheHit = false;

       try {
         // Check cache first
         if (useCache) {
           const cacheKey = this.buildCacheKey(prompt, options);
           const cached = await this.cacheService.getCachedAIResponse(cacheKey);
           
           if (cached) {
             cacheHit = true;
             TelemetryService.trackEvent('AIModelCacheHit', {
               prompt: prompt.substring(0, 100),
               model: options.model || 'gpt-4'
             });
             return cached;
           }
         }

         // Make AI call
         const completion = await this.openAIClient.getCompletions(
           options.model || 'gpt-4',
           [prompt],
           {
             maxTokens: options.maxTokens || 500,
             temperature: options.temperature || 0.7,
             topP: options.topP || 0.9,
             ...options
           }
         );

         const response = completion.choices[0]?.text?.trim() || '';

         // Cache the response
         if (useCache && response) {
           const cacheKey = this.buildCacheKey(prompt, options);
           await this.cacheService.cacheAIResponse(cacheKey, response, 3600);
         }

         // Track performance
         const duration = Date.now() - startTime;
         TelemetryService.trackAIModelPerformance(
           options.model || 'gpt-4',
           'completion',
           duration,
           true,
           {
             cacheHit: false,
             promptLength: prompt.length,
             responseLength: response.length
           }
         );

         return response;

       } catch (error) {
         const duration = Date.now() - startTime;
         TelemetryService.trackAIModelPerformance(
           options.model || 'gpt-4',
           'completion',
           duration,
           false,
           { error: error.message }
         );
         
         throw error;
       }
     }

     private buildCacheKey(prompt: string, options: any): string {
       const keyData = {
         prompt: prompt,
         maxTokens: options.maxTokens || 500,
         temperature: options.temperature || 0.7,
         model: options.model || 'gpt-4'
       };
       
       return Buffer.from(JSON.stringify(keyData)).toString('base64');
     }
   }
   ```

## Best Practices for Custom Engine Agents

### 1. Architecture Design

#### Modular Component Structure
```
src/
├── actions/           # Custom action implementations
├── services/          # Business logic services  
├── handlers/          # Message and event handlers
├── models/            # Data models and interfaces
├── utils/             # Helper utilities
├── config/            # Configuration management
├── middleware/        # Custom middleware
└── tests/             # Unit and integration tests
```

#### Separation of Concerns
```markdown
DESIGN PRINCIPLES:
- Actions: Handle specific user operations
- Services: Implement business logic
- Handlers: Manage conversation flow
- Models: Define data structures
- Utils: Provide reusable functionality
```

### 2. Error Handling and Resilience

#### Comprehensive Error Management
```typescript
export class ErrorHandler {
  static async handleActionError(
    context: TurnContext,
    error: Error,
    actionName: string
  ): Promise<string> {
    // Log error with context
    TelemetryService.trackError(error, {
      actionName,
      userId: context.activity.from.id,
      conversationId: context.activity.conversation.id
    });

    // Provide user-friendly response based on error type
    if (error.message.includes('timeout')) {
      return "The operation is taking longer than expected. Please try again.";
    }
    
    if (error.message.includes('unauthorized')) {
      return "I don't have permission to access that information. Please contact your administrator.";
    }
    
    return "I encountered an error while processing your request. Our team has been notified.";
  }
}
```

### 3. Security Best Practices

#### Input Validation and Sanitization
```typescript
export class SecurityValidator {
  static validateInput(input: string): boolean {
    // Check for malicious patterns
    const dangerousPatterns = [
      /<script/i,
      /javascript:/i,
      /on\w+\s*=/i,
      /eval\s*\(/i
    ];
    
    return !dangerousPatterns.some(pattern => pattern.test(input));
  }

  static sanitizeOutput(output: string): string {
    return output
      .replace(/</g, '&lt;')
      .replace(/>/g, '&gt;')
      .replace(/"/g, '&quot;')
      .replace(/'/g, '&#x27;');
  }
}
```

### 4. Performance Optimization

#### Batch Processing
```typescript
export class BatchProcessor {
  static async processBatch<T, R>(
    items: T[],
    processor: (item: T) => Promise<R>,
    batchSize: number = 5
  ): Promise<R[]> {
    const results: R[] = [];
    
    for (let i = 0; i < items.length; i += batchSize) {
      const batch = items.slice(i, i + batchSize);
      const batchResults = await Promise.all(
        batch.map(item => processor(item))
      );
      results.push(...batchResults);
    }
    
    return results;
  }
}
```

## Knowledge Check

### Practical Development Exercises

1. **Build Custom Action Library**
   - Create 5 different custom actions
   - Implement error handling and validation
   - Add comprehensive logging
   - Write unit tests for each action

2. **Azure AI Integration Project**
   - Integrate 3 different Azure AI services
   - Implement caching for AI calls
   - Add performance monitoring
   - Create fallback mechanisms

3. **Production Deployment**
   - Set up CI/CD pipeline
   - Configure monitoring and alerting
   - Implement security scanning
   - Load test the application

### Assessment Questions

1. What are the key differences between declarative and custom engine agents?
2. How do you implement proper error handling in custom engine agents?
3. What are the best practices for integrating Azure AI services?
4. How do you optimize performance for custom engine agents?
5. What security considerations apply to custom engine agent development?

## Summary

In this module, you learned:
- The architecture and capabilities of custom engine agents
- How to build sophisticated agents using the Teams AI Library
- Integration patterns with Azure AI services and search capabilities
- Advanced conversation management and state handling
- Production deployment, monitoring, and optimization strategies
- Best practices for security, performance, and maintainability

### Key Advantages of Custom Engine Agents
- **Complete Control**: Full control over AI processing pipeline
- **Advanced Capabilities**: Integration with cutting-edge AI services
- **Custom Logic**: Implementation of complex business rules
- **Scalability**: Designed for enterprise-scale deployments
- **Extensibility**: Unlimited customization possibilities

### Next Steps
In Module 7, we'll explore SharePoint Integration, learning how to create agents that work seamlessly with SharePoint data, embedded scenarios, and content management workflows.

## Additional Resources

- [Teams AI Library Documentation](https://learn.microsoft.com/en-us/microsoftteams/platform/bots/how-to/teams-conversational-ai/teams-ai-library)
- [Azure OpenAI Service](https://learn.microsoft.com/en-us/azure/ai-services/openai/)
- [Azure AI Search](https://learn.microsoft.com/en-us/azure/search/)
- [Teams Toolkit](https://learn.microsoft.com/en-us/microsoftteams/platform/toolkit/teams-toolkit-fundamentals)
- [Bot Framework](https://dev.botframework.com/)

---

*Continue to Module 7: SharePoint Integration to learn about creating agents that work seamlessly with SharePoint content and collaboration scenarios.*