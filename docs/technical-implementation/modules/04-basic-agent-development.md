# Module 2: Basic Agent Development

## Learning Objectives
By the end of this module, you will be able to:
- Build conversational agents with custom topics and actions
- Create autonomous agents with triggers and dynamic workflows
- Implement generative actions for natural conversation flow
- Design effective agent instructions and prompts
- Test and debug agent behavior effectively

## Prerequisites
- Completion of Module 1: Copilot Studio Fundamentals
- Basic understanding of Power Platform concepts
- Access to Copilot Studio with appropriate licenses

## Overview

This module focuses on developing both conversational and autonomous agents, moving beyond basic generative answers to create sophisticated AI assistants that can handle complex scenarios and take actions on behalf of users.

## Part 1: Conversational Agent Development

### Understanding Topics vs. Generative Actions

#### Traditional Topics (Manual Authoring)
- **Definition**: Pre-authored conversation flows with specific triggers and responses
- **Use Cases**: Predictable workflows, compliance-sensitive scenarios
- **Structure**: Trigger phrases → Node-based conversation flow → Actions
- **Control**: High control but requires manual maintenance

#### Generative Actions (AI-Driven)
- **Definition**: AI orchestrated conversations using large language models
- **Use Cases**: Natural, flexible conversations with dynamic responses
- **Structure**: Plugin actions → AI orchestration → Natural conversation
- **Control**: Less control but more natural and adaptive

## Lab 1: Building a Conversational Agent with Traditional Topics

### Scenario: IT Help Desk Agent
Create an agent that helps employees with common IT issues like password resets and software installations.

### Step 1: Create the Base Agent

1. **Navigate to Copilot Studio**
   - Click **"Create"** > **"New agent"**
   - Name: "IT Help Desk Assistant"
   - Description: "Assists employees with IT support requests"

2. **Configure Agent Settings**
   - Language: English (United States)
   - Solution: Default
   - Environment: Select your environment

### Step 2: Create Custom Topics

#### Topic 1: Password Reset
1. **Create New Topic**
   - Go to **"Topics"** tab
   - Click **"+ New topic"** > **"From blank"**
   - Name: "Password Reset Help"

2. **Add Trigger Phrases**
   - "I forgot my password"
   - "Reset my password"
   - "Password not working"
   - "Can't log in"
   - "Password reset"

3. **Design Conversation Flow**
   ```
   Trigger → Message Node → Question Node → Condition Node → Action Node
   ```

4. **Add Message Node**
   - Click **"+ Add node"** > **"Send a message"**
   - Message text: "I can help you reset your password. Let me gather some information first."

5. **Add Question Node**
   - Click **"+ Add node"** > **"Ask a question"**
   - Question: "What's your employee ID?"
   - Variable name: `EmployeeID`
   - Entity type: Text

6. **Add Condition Node**
   - Click **"+ Add node"** > **"Add a condition"**
   - Variable: `EmployeeID`
   - Condition: "is not blank"

7. **Add Action Nodes**
   - **If employee ID exists**: Send message with reset instructions
   - **If blank**: Ask for employee ID again

#### Topic 2: Software Installation Request
1. **Create Topic**
   - Name: "Software Installation"
   - Trigger phrases:
     - "Install software"
     - "Need new software"
     - "Software request"
     - "Install application"

2. **Conversation Flow**
   - Ask for software name
   - Ask for business justification
   - Confirm request details
   - Create service ticket

### Step 3: Test Traditional Topics
1. Click **"Test"** button
2. Try trigger phrases for each topic
3. Verify conversation flows work as expected
4. Test edge cases and error handling

## Lab 2: Building with Generative Actions

### Scenario: Employee Leave Request Assistant
Create an intelligent agent that can handle various leave request scenarios using AI-powered conversations.

### Step 1: Set Up Plugin Actions

1. **Navigate to Generative AI Settings**
   - Go to **"Generative AI"** tab
   - Turn off **"Generative answers"**
   - Enable **"Generative actions"**

2. **Create Power Automate Flow for Leave Requests**
   - In Power Automate, create new flow
   - Trigger: **"When a flow is run from copilot"**
   - Parameters:
     - `StartDate` (Date)
     - `EndDate` (Date)  
     - `EmployeeID` (Text)
     - `LeaveType` (Text)
     - `Reason` (Text)

3. **Add Flow Actions**
   - **Parse JSON**: Structure the leave request data
   - **Create item**: Add to SharePoint leave requests list
   - **Send email**: Notify manager
   - **Respond to copilot**: Return confirmation

### Step 2: Add Plugin Actions to Agent

1. **Add Plugin Action**
   - In Copilot Studio, go to **"Actions"** tab
   - Click **"+ Add action"**
   - Choose **"Flows"**
   - Select your leave request flow

2. **Configure Action Properties**
   - **Name**: "Create Leave Request"
   - **Description**: "Creates a new employee leave request with specified dates and reason"
   - Input parameters:
     - Start date for leave
     - End date for leave
     - Type of leave (vacation, sick, personal)
     - Reason for leave
   - Output: Confirmation status and request ID

### Step 3: Add Second Plugin Action for Leave Balance

1. **Create Balance Check Flow**
   - Trigger: **"When a flow is run from copilot"**
   - Parameter: `EmployeeID` (Text)
   - Action: Query HR system or SharePoint list
   - Response: Available leave balances by type

2. **Add to Copilot Studio**
   - Name: "Check Leave Balance"
   - Description: "Retrieves current leave balance for an employee"

### Step 4: Configure Agent Instructions

```markdown
You are an AI assistant that helps employees with leave requests and balance inquiries.

CAPABILITIES:
- Check employee leave balances
- Create new leave requests
- Answer questions about leave policies

INSTRUCTIONS:
1. When employees ask about leave balances, use the "Check Leave Balance" action
2. When employees want to request time off, use the "Create Leave Request" action
3. Always confirm details before submitting requests
4. Be helpful and professional in all interactions

LEAVE TYPES:
- Vacation: Planned time off for personal activities
- Sick: Medical leave for illness or medical appointments  
- Personal: Other personal matters requiring time off
- Emergency: Unexpected urgent situations

VALIDATION RULES:
- Leave requests must be for future dates
- Minimum notice is 48 hours for non-emergency leave
- Maximum single request is 30 days
```

### Step 5: Test Generative Actions

1. **Test Natural Conversations**
   - "What's my vacation balance?"
   - "I need to take time off next week"
   - "Create a leave request from March 15-17 for vacation"
   - "How much sick leave do I have left?"

2. **Observe AI Behavior**
   - AI automatically selects correct plugin actions
   - Extracts parameters from natural language
   - Asks for missing information naturally
   - Handles corrections and modifications

## Part 2: Autonomous Agent Development

### Key Concepts for Autonomous Agents

#### Triggers
- **External Events**: SharePoint list items, emails, calendar events
- **Power Platform Connectors**: Direct mapping to connector triggers
- **Automatic Execution**: No user interaction required to start

#### Orchestration
- **Generative Orchestration**: Must be enabled for autonomous agents
- **AI Decision Making**: Agent decides which actions to take based on instructions
- **Dynamic Workflows**: Adapts behavior based on context and data

## Lab 3: Creating an Autonomous Employee Onboarding Agent

### Scenario
Build an autonomous agent that automatically processes new employee entries in a SharePoint list and:
- Schedules required training sessions
- Procures appropriate hardware based on department
- Creates service tickets for IT setup

### Step 1: Prepare Data Sources

#### Create SharePoint Lists

1. **New Employees List**
   - Columns:
     - Title (Person name)
     - Email (Single line of text)
     - Department (Choice: Sales, IT, Marketing, HR)
     - StartDate (Date)
     - Status (Choice: Pending, Processing, Complete)

2. **Training Sessions List**
   - Columns:
     - TrainingName (Single line of text)
     - Department (Choice: All, Sales, IT, Marketing, HR)
     - DayOfWeek (Choice: Monday-Friday)
     - Duration (Number)
     - Cadence (Choice: Weekly, Monthly, One-time)

3. **Hardware Requests List**
   - Columns:
     - EmployeeName (Single line of text)
     - DeviceType (Single line of text)
     - Model (Single line of text)
     - Cost (Currency)
     - Status (Choice: Requested, Approved, Ordered)
     - AssetTag (Single line of text)

### Step 2: Create Knowledge Sources

1. **Upload Training Document**
   - Create Word document with training matrix
   - Include department-specific requirements
   - Upload to agent knowledge sources

2. **Add Business Devices Knowledge**
   - Point to Microsoft Surface products website
   - Or upload device catalog document

### Step 3: Set Up Actions

#### Action 1: Schedule Training Flow
1. **Create Power Automate Flow**
   - Trigger: **"When a flow is run from copilot"**
   - Parameters:
     - `EmployeeEmail` (Text)
     - `TrainingName` (Text)  
     - `StartDateTime` (Date/Time)

2. **Flow Actions**
   - Create Teams meeting
   - Send calendar invitation
   - Update training status

#### Action 2: Create Hardware Request
1. **Add SharePoint Action**
   - Action: **"Create item"**
   - List: Hardware Requests
   - Configure dynamic inputs:
     - Title: "Fill with the manufacturer and model of the device from the business devices knowledge source"
     - Cost: "Fill with device cost from knowledge source"
     - AssetTag: "Generate random 6-digit asset tag starting with ASSET-"
     - EmployeeName: "Fill with employee name from trigger"

### Step 4: Configure the Trigger

1. **Add Trigger**
   - Go to **"Triggers"** tab
   - Click **"+ Add trigger"**
   - Search for "SharePoint"
   - Select **"When an item is created"**

2. **Configure Trigger Settings**
   - Site address: Select your SharePoint site
   - List name: New Employees
   - Trigger condition: None (fires for all new items)

3. **Enable Generative Orchestration**
   - Toggle **"Generative orchestration"** to On
   - This is required for autonomous agents

### Step 5: Write Comprehensive Instructions

```markdown
AUTONOMOUS EMPLOYEE ONBOARDING AGENT

ROLE: You automate new employee onboarding tasks without user interaction.

TRIGGER: When a new employee is added to the SharePoint list, you will:

1. ANALYZE THE NEW EMPLOYEE DATA
   - Extract name, email, department, and start date
   - Use this information for all subsequent actions

2. SCHEDULE REQUIRED TRAINING
   - Use the 'IT Training' knowledge source to find required training for the employee's department
   - For each required training:
     - Create Teams meetings using the 'Schedule Training' action
     - Schedule on the appropriate weekday based on training requirements
     - Set duration according to training specifications
   - IMPORTANT: Only schedule training relevant to the employee's department

3. PROCURE HARDWARE
   - Use the 'Business Devices' knowledge source to select appropriate device
   - Department-specific requirements:
     - IT employees: High-specification laptops (minimum 16GB RAM)
     - Sales employees: Standard business laptops  
     - Marketing: Devices suitable for creative work
     - HR: Standard office devices
   - Create hardware request using the 'Create Hardware Request' action
   - MUST ONLY select devices from the knowledge source - do not hallucinate
   - Include detailed specifications in the description

4. ERROR HANDLING
   - If no suitable device is found, create request noting "Device selection pending manual review"
   - If training cannot be scheduled, log the issue but continue with other tasks

5. COMPLETION
   - Process all steps for each new employee
   - Provide summary of actions taken
   - Update employee status if possible

IMPORTANT NOTES:
- Reference knowledge sources using single quotes: 'IT Training', 'Business Devices'
- Be specific about device requirements based on department
- Generate asset tags automatically (format: ASSET-XXXXXX)
- Pay attention to all requirements - this is critical for success
```

### Step 6: Test the Autonomous Agent

1. **Test Trigger**
   - Add new employee to SharePoint list
   - Wait up to 1 minute for trigger activation
   - Use **"Test"** button in triggers section

2. **Monitor Execution**
   - Watch conversation history for AI decision-making
   - Verify knowledge source queries
   - Check action executions
   - Review final outputs

3. **Verify Results**
   - Check if Teams meetings were created
   - Verify hardware requests were generated
   - Confirm all data is accurate

### Expected Behavior
- Agent processes new employee automatically
- Creates department-specific training sessions
- Generates appropriate hardware requests
- Completes workflow without user intervention

## Advanced Agent Development Techniques

### 1. Dynamic Action Selection

#### Using Context Variables
```markdown
When user mentions "password", use 'Reset Password' action
When user mentions "software" or "application", use 'Software Request' action  
When user mentions "leave" or "vacation", use 'Leave Request' action
```

#### Multiple Action Chains
```markdown
For new employee onboarding:
1. First, use 'Check Employee' action to validate
2. Then, use 'Schedule Training' action for each required session
3. Finally, use 'Create Hardware Request' action
```

### 2. Error Handling and Validation

#### Input Validation
```markdown
VALIDATION RULES:
- Employee ID must be 6 digits
- Dates must be in future
- Required fields cannot be empty

If validation fails:
- Ask user to provide correct information
- Give specific error message
- Provide examples of correct format
```

#### Graceful Degradation
```markdown
If external system is unavailable:
- Log the request for manual processing
- Notify user of the delay
- Provide alternative contact information
```

### 3. Conversation Context Management

#### Maintaining State
- Use variables to track conversation progress
- Reference previous responses appropriately
- Handle context switches naturally

#### Follow-up Questions
```markdown
After creating leave request:
- "Would you like me to send a copy to your manager?"
- "Do you need help with coverage planning?"
- "Should I set a reminder to follow up?"
```

## Testing and Debugging Strategies

### 1. Systematic Testing Approach

#### Test Categories
1. **Happy Path**: Normal, expected interactions
2. **Edge Cases**: Boundary conditions and unusual inputs
3. **Error Scenarios**: System failures and invalid data
4. **Integration**: Cross-system interactions

#### Test Scripts Example
```
Test 1: Basic Leave Request
Input: "I need 3 days off next week"
Expected: Agent asks for dates, type, reason
Verify: All parameters collected correctly

Test 2: Incomplete Information  
Input: "Create leave request"
Expected: Agent asks for missing details
Verify: Natural follow-up questions

Test 3: Date Validation
Input: "Leave request for yesterday"
Expected: Agent flags invalid date
Verify: Proper error handling
```

### 2. Using the Conversation Debugger

#### Trace Window Features
- **Action Selection**: See which plugin actions were considered
- **Parameter Extraction**: View how AI extracted information
- **Knowledge Queries**: Monitor knowledge source searches
- **Decision Logic**: Understand AI reasoning

#### Debugging Common Issues
1. **Wrong Action Selected**
   - Review action descriptions
   - Make descriptions more specific
   - Add disambiguation instructions

2. **Poor Parameter Extraction**
   - Provide parameter examples in descriptions
   - Add validation prompts
   - Use more specific entity types

3. **Knowledge Source Problems**
   - Verify content quality
   - Check indexing status
   - Review search relevance

### 3. Performance Optimization

#### Response Time Optimization
- Minimize knowledge source queries
- Use efficient action designs
- Implement caching where appropriate

#### Accuracy Improvements
- Refine agent instructions iteratively
- Use specific examples in prompts
- Implement validation checks

## Best Practices for Agent Development

### 1. Instruction Writing

#### Be Specific and Clear
```markdown
❌ "Help users with requests"
✅ "Process employee leave requests by collecting start date, end date, leave type, and reason, then create the request using the Create Leave Request action"
```

#### Use Examples
```markdown
EXAMPLE INTERACTIONS:
User: "I need time off"
Agent: "I can help with your leave request. What dates do you need off?"

User: "March 15-17"  
Agent: "Got it, March 15-17. What type of leave is this - vacation, sick, or personal?"
```

#### Reference Actions Clearly
```markdown
Use single quotes when referencing:
- Actions: 'Create Leave Request', 'Check Balance'  
- Knowledge sources: 'HR Policies', 'Training Guide'
- Variables: 'EmployeeID', 'StartDate'
```

### 2. Knowledge Source Management

#### Content Quality
- Keep information current and accurate
- Use clear headings and structure
- Remove outdated or conflicting information

#### Access Control
- Implement appropriate security
- Verify user permissions
- Audit knowledge source access

### 3. Action Design

#### Single Responsibility
- Each action should have one clear purpose
- Avoid complex multi-step actions
- Use composition for complex workflows

#### Clear Descriptions
- Describe exactly what the action does
- Include parameter requirements
- Specify expected outputs

### 4. User Experience

#### Natural Conversation Flow
- Use conversational language
- Provide helpful prompts
- Handle interruptions gracefully

#### Error Recovery
- Give clear error messages
- Provide correction options
- Offer alternative paths

## Troubleshooting Common Issues

### Agent Not Responding
1. **Check Trigger Configuration**
   - Verify trigger is enabled
   - Test trigger conditions
   - Review Power Automate flow status

2. **Review Orchestration Settings**
   - Ensure generative orchestration is enabled
   - Check for conflicting settings
   - Verify licensing requirements

### Poor Conversation Quality
1. **Improve Instructions**
   - Add more specific guidance
   - Include conversation examples
   - Define clear boundaries

2. **Refine Action Descriptions**
   - Make descriptions more specific
   - Add parameter examples
   - Include usage scenarios

### Action Execution Failures
1. **Check Permissions**
   - Verify connector permissions
   - Review Power Automate flow permissions
   - Check SharePoint access rights

2. **Debug Data Flow**
   - Test actions independently
   - Verify parameter mapping
   - Check data validation

## Knowledge Check

### Practical Exercises

1. **Create a Multi-Topic Agent**
   - Build an agent with 3 different topics
   - Include question nodes and conditions
   - Test conversation flow thoroughly

2. **Build Generative Actions Agent**
   - Create 2 plugin actions
   - Write comprehensive instructions
   - Test natural language interactions

3. **Design Autonomous Workflow**
   - Create trigger-based agent
   - Include multiple actions
   - Test end-to-end automation

### Assessment Questions

1. When would you choose traditional topics over generative actions?
2. What are the key components required for an autonomous agent?
3. How do you handle errors in agent conversations?
4. What's the difference between plugin actions and Power Automate flows?
5. How do you test and debug complex agent behaviors?

## Summary

In this module, you learned:
- How to build conversational agents using both traditional topics and generative actions
- The process of creating autonomous agents with triggers and dynamic workflows
- Best practices for writing effective agent instructions
- Testing and debugging strategies for complex agent behaviors
- Performance optimization and error handling techniques

### Next Steps
In Module 3, we'll explore Power Platform integration, learning how to connect your agents to various data sources and external systems through connectors.

## Additional Resources

- [Topics in Copilot Studio](https://learn.microsoft.com/en-us/microsoft-copilot-studio/authoring-fundamentals)
- [Generative Actions Documentation](https://learn.microsoft.com/en-us/microsoft-copilot-studio/advanced-generative-actions)
- [Autonomous Agents Guide](https://learn.microsoft.com/en-us/microsoft-copilot-studio/advanced-autonomous-agent)
- [Power Automate Integration](https://learn.microsoft.com/en-us/microsoft-copilot-studio/advanced-flow)

---

*Continue to Module 3: Connectors & Data Integration to learn about connecting your agents to external systems and data sources.*