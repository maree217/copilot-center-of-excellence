# Module 3: Connectors & Data Integration

## Learning Objectives
By the end of this module, you will be able to:
- Understand Power Platform connectors and their role in Copilot Studio
- Create and configure custom connectors for external systems
- Integrate various data sources using pre-built connectors
- Implement Dataverse as a knowledge source and action target
- Design secure data integration patterns
- Troubleshoot connector configuration and authentication issues

## Prerequisites
- Completion of Modules 1 and 2
- Understanding of REST APIs and HTTP methods
- Basic knowledge of authentication protocols (OAuth, API keys)
- Access to Power Platform environment with connector privileges

## Overview

Power Platform connectors are the bridge between your Copilot Studio agents and external systems. They enable agents to read data, perform actions, and integrate with hundreds of services including Microsoft services, third-party applications, and custom APIs.

## Understanding Power Platform Connectors

### Connector Architecture

```
Copilot Studio Agent → Power Platform Connector → External System/API
                     ↑
                Authentication & Configuration
```

### Connector Types

#### 1. Standard Connectors
- **Pre-built by Microsoft**: SharePoint, Teams, Outlook, SQL Server
- **Ready to Use**: No development required
- **Free/Premium**: Varies by connector and license
- **Examples**: 
  - SharePoint (create items, get lists)
  - Teams (send messages, create channels)
  - Outlook (send emails, read calendar)

#### 2. Premium Connectors
- **Advanced Functionality**: Complex systems and services
- **License Required**: Premium Power Platform license
- **Examples**:
  - Salesforce
  - ServiceNow  
  - SAP
  - Oracle Database

#### 3. Custom Connectors
- **Your APIs**: Connect to custom or unsupported systems
- **Full Control**: Define operations, authentication, and parameters
- **Development Required**: Create OpenAPI specification

## Lab 1: Using Pre-built Connectors

### Scenario: Employee Directory Agent
Build an agent that can search and update employee information using SharePoint as a data source.

### Step 1: Prepare SharePoint List

1. **Create Employee Directory List**
   - Navigate to SharePoint site
   - Create new list: "Employee Directory"
   - Add columns:
     - Title (Person name) - default
     - Department (Choice: IT, Sales, Marketing, HR, Finance)
     - JobTitle (Single line of text)
     - PhoneNumber (Single line of text)
     - Location (Choice: New York, London, Tokyo, Remote)
     - StartDate (Date)
     - Skills (Multiple lines of text)
     - Manager (Person or Group)

2. **Add Sample Data**
   - Add 10-15 employee records
   - Include variety of departments and locations
   - Ensure realistic job titles and skills

### Step 2: Create Agent with SharePoint Actions

1. **Create New Agent**
   - Name: "Employee Directory Assistant"
   - Description: "Helps find and manage employee information"

2. **Add SharePoint Connector Actions**
   
   #### Action 1: Search Employees
   - Go to **"Actions"** tab
   - Click **"+ Add action"** > **"Connectors"**
   - Search for "SharePoint"
   - Select **"Get items"**
   
   **Configure Action:**
   - Name: "Search Employees"
   - Description: "Search for employees by department, name, or job title"
   - Site address: Select your SharePoint site
   - List name: Employee Directory
   
   #### Action 2: Update Employee Information
   - Add another SharePoint connector
   - Select **"Update item"**
   
   **Configure Action:**
   - Name: "Update Employee"
   - Description: "Update employee information like phone number, location, or job title"
   - Site address: Same SharePoint site
   - List name: Employee Directory

### Step 3: Configure Generative Actions

1. **Enable Generative Actions**
   - Go to **"Generative AI"** tab
   - Enable **"Generative actions"**
   - Disable **"Generative answers"** for this demo

2. **Write Agent Instructions**
```markdown
You are an Employee Directory Assistant that helps users find and manage employee information.

CAPABILITIES:
- Search for employees by name, department, job title, or location
- Update employee information (phone, location, job title)
- Provide employee details and contact information

ACTIONS AVAILABLE:
- 'Search Employees': Use this to find employees based on user criteria
- 'Update Employee': Use this to modify employee information

SEARCH INSTRUCTIONS:
- When users ask for employees by department, use the Search Employees action
- When users ask for specific people, search by name
- Always provide relevant employee details in responses
- If multiple employees match, show summary information for each

UPDATE INSTRUCTIONS:  
- Only update employee information when explicitly requested
- Confirm changes before updating
- Required information for updates: Employee ID and field to update
- Supported updates: PhoneNumber, Location, JobTitle, Department

RESPONSE FORMAT:
- Present employee information clearly
- Include name, department, job title, location, and contact info
- Use professional, helpful tone
- Ask clarifying questions when search criteria is unclear

EXAMPLES:
User: "Find employees in IT department"
Response: Use Search Employees action, then present formatted results

User: "Update John Smith's phone number to 555-0123"  
Response: Use Update Employee action with ID and new phone number
```

### Step 4: Test the Integration

1. **Test Employee Search**
   - "Show me all employees in the IT department"
   - "Find John Smith"
   - "Who works in the London office?"
   - "List all managers"

2. **Test Employee Updates**
   - "Update Sarah Johnson's location to Remote"
   - "Change Mike Chen's job title to Senior Developer"
   - "Update the phone number for Alice Brown to 555-0199"

3. **Review Connector Activity**
   - Check SharePoint list for updates
   - Verify search results accuracy
   - Monitor connector usage in Power Platform admin center

## Lab 2: Creating Custom Connectors

### Scenario: Integration with External HR System
Create a custom connector to integrate with a REST API that manages employee leave balances and requests.

### Step 1: Design API Specification

For this lab, we'll create a connector for a hypothetical HR API with these endpoints:

```yaml
# OpenAPI 3.0 Specification
openapi: 3.0.0
info:
  title: HR Leave Management API
  version: 1.0.0
  description: API for managing employee leave requests and balances

servers:
  - url: https://api.company.com/hr/v1

paths:
  /employees/{employeeId}/leave-balance:
    get:
      summary: Get employee leave balance
      parameters:
        - name: employeeId
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Leave balance information
          content:
            application/json:
              schema:
                type: object
                properties:
                  employeeId:
                    type: string
                  vacationDays:
                    type: number
                  sickDays:
                    type: number
                  personalDays:
                    type: number
                    
  /leave-requests:
    post:
      summary: Create new leave request
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                employeeId:
                  type: string
                startDate:
                  type: string
                  format: date
                endDate:
                  type: string
                  format: date
                leaveType:
                  type: string
                  enum: [vacation, sick, personal]
                reason:
                  type: string
      responses:
        '201':
          description: Leave request created
          content:
            application/json:
              schema:
                type: object
                properties:
                  requestId:
                    type: string
                  status:
                    type: string
                  message:
                    type: string
```

### Step 2: Create Custom Connector

1. **Navigate to Power Platform**
   - Go to make.powerapps.com
   - Select your environment
   - Go to **"Dataverse"** > **"Custom connectors"**

2. **Create from Blank**
   - Click **"+ New custom connector"** > **"Create from blank"**
   - Connector name: "HR Leave Management"
   - Description: "Manages employee leave balances and requests"
   - Host: `api.company.com`
   - Base URL: `/hr/v1`

3. **Configure Security**
   - Authentication type: **"API Key"**
   - Parameter label: "API Key"
   - Parameter name: "x-api-key"
   - Parameter location: "Header"

### Step 3: Define Operations

#### Operation 1: Get Leave Balance
1. **Add New Action**
   - Click **"Definition"** tab
   - Click **"New action"**
   - Summary: "Get Employee Leave Balance"
   - Description: "Retrieves leave balance information for an employee"
   - Operation ID: "GetLeaveBalance"
   - Visibility: "important"

2. **Configure Request**
   - Verb: GET
   - URL: `/employees/{employeeId}/leave-balance`
   
   **Parameters:**
   - Name: employeeId
   - Type: string
   - Required: Yes
   - Location: path
   - Description: "Employee ID to look up"

3. **Configure Response**
   - Click **"Default"** response
   - Body schema:
   ```json
   {
     "type": "object",
     "properties": {
       "employeeId": {"type": "string"},
       "vacationDays": {"type": "number"},
       "sickDays": {"type": "number"},
       "personalDays": {"type": "number"}
     }
   }
   ```

#### Operation 2: Create Leave Request
1. **Add Second Action**
   - Summary: "Create Leave Request"
   - Description: "Creates a new leave request for an employee"
   - Operation ID: "CreateLeaveRequest"
   - Verb: POST
   - URL: `/leave-requests`

2. **Configure Request Body**
   - Body schema:
   ```json
   {
     "type": "object",
     "properties": {
       "employeeId": {"type": "string", "description": "Employee ID"},
       "startDate": {"type": "string", "format": "date", "description": "Leave start date"},
       "endDate": {"type": "string", "format": "date", "description": "Leave end date"},
       "leaveType": {"type": "string", "enum": ["vacation", "sick", "personal"]},
       "reason": {"type": "string", "description": "Reason for leave"}
     },
     "required": ["employeeId", "startDate", "endDate", "leaveType"]
   }
   ```

3. **Configure Response**
   ```json
   {
     "type": "object", 
     "properties": {
       "requestId": {"type": "string"},
       "status": {"type": "string"},
       "message": {"type": "string"}
     }
   }
   ```

### Step 4: Test and Deploy Connector

1. **Test Connector**
   - Click **"Test"** tab
   - Create new connection with API key
   - Test both operations with sample data

2. **Create and Deploy**
   - Click **"Create connector"**
   - Wait for deployment to complete
   - Verify connector appears in connectors list

### Step 5: Use Custom Connector in Agent

1. **Add Custom Connector Actions**
   - In Copilot Studio, go to **"Actions"** tab
   - Click **"+ Add action"** > **"Connectors"**  
   - Find your "HR Leave Management" connector
   - Add both operations as actions

2. **Configure Action Settings**
   
   **Get Leave Balance Action:**
   - Name: "Check Leave Balance"
   - Description: "Check available leave balance for an employee by their ID"
   
   **Create Leave Request Action:**
   - Name: "Submit Leave Request"  
   - Description: "Create a new leave request with start date, end date, type, and reason"

3. **Update Agent Instructions**
```markdown
You are an HR Leave Assistant that helps employees check leave balances and submit requests.

ACTIONS AVAILABLE:
- 'Check Leave Balance': Use when employees ask about available leave days
- 'Submit Leave Request': Use when employees want to request time off

LEAVE BALANCE QUERIES:
- Ask for employee ID if not provided
- Use Check Leave Balance action
- Present results in friendly format showing vacation, sick, and personal days

LEAVE REQUEST PROCESS:
- Collect required information: employee ID, start date, end date, leave type, reason
- Validate dates are in the future
- Use Submit Leave Request action
- Confirm request was created successfully

LEAVE TYPES:
- vacation: Planned time off
- sick: Medical leave  
- personal: Personal matters

RESPONSE FORMAT:
- Be conversational and helpful
- Ask for missing information politely
- Confirm details before submitting requests
- Provide clear confirmation messages
```

## Working with Dataverse

### Overview of Dataverse Integration

Dataverse is the native data platform for Power Platform, providing:
- **Structured Data Storage**: Tables, relationships, and business logic
- **Security**: Role-based access control and field-level security  
- **Integration**: Native connector with optimized performance
- **Business Logic**: Calculated fields, business rules, and workflows

### Lab 3: Using Dataverse as Knowledge Source

#### Scenario: Product Catalog Assistant
Build an agent that uses Dataverse to store and retrieve product information.

### Step 1: Create Dataverse Tables

1. **Navigate to Dataverse**
   - Go to make.powerapps.com
   - Select environment
   - Go to **"Tables"**

2. **Create Products Table**
   - Click **"+ New table"**
   - Table name: "Product"
   - Primary name column: "Product Name"
   - Enable attachments, notes, activities

3. **Add Columns**
   - **Category** (Choice): Electronics, Clothing, Books, Home, Sports
   - **Price** (Currency)
   - **Description** (Multiple lines of text)
   - **Stock Quantity** (Whole number)
   - **SKU** (Single line of text)
   - **Is Active** (Yes/No)
   - **Launch Date** (Date only)

4. **Add Sample Data**
   - Create 20+ product records
   - Include variety across categories
   - Ensure some items are out of stock

### Step 2: Configure Dataverse Knowledge Source

1. **Add Knowledge Source**
   - In Copilot Studio agent
   - Go to **"Knowledge"** tab
   - Click **"+ Add knowledge"**
   - Select **"Dataverse"**

2. **Configure Connection**
   - Select environment
   - Choose "Product" table
   - Select columns to include:
     - Product Name
     - Category  
     - Price
     - Description
     - Stock Quantity
     - SKU

3. **Test Knowledge Retrieval**
   - Ask: "What electronics products do you have?"
   - Ask: "Show me products under $50"
   - Ask: "Tell me about product SKU ABC123"

### Step 3: Add Dataverse Actions

1. **Add Table Actions**
   - **Get Records**: Search and filter products
   - **Create Record**: Add new products
   - **Update Record**: Modify existing products

2. **Configure Actions**
   
   **Search Products Action:**
   - Filter: Category equals [user input]
   - Select columns: Name, Price, Description, Stock
   - Order by: Price ascending
   
   **Update Stock Action:**
   - Input: Product ID, New Quantity
   - Update: Stock Quantity field

### Step 4: Implement Inventory Management

```markdown
PRODUCT CATALOG ASSISTANT INSTRUCTIONS:

You help users find products and manage inventory using Dataverse.

PRODUCT SEARCH:
- Use knowledge source for general product questions
- Use 'Search Products' action for specific filtering
- Present results with name, price, stock level, and description
- Indicate if items are out of stock

INVENTORY MANAGEMENT:
- Use 'Update Stock' action when users report inventory changes
- Confirm stock updates with before/after quantities
- Alert when stock levels are low (less than 10 units)

PRODUCT INFORMATION:
- Always include key details: price, availability, category
- Suggest alternatives for out-of-stock items
- Provide SKU for ordering purposes

RESPONSE FORMAT:
- Use clear, structured responses
- Group similar products together
- Highlight important information like stock levels
- Ask clarifying questions for better search results
```

## Advanced Connector Patterns

### 1. Connector Chaining

Create workflows that use multiple connectors in sequence:

```markdown
WORKFLOW: New Customer Onboarding
1. Get customer data from Salesforce (Salesforce connector)
2. Create SharePoint folder (SharePoint connector)  
3. Send welcome email (Outlook connector)
4. Create Teams channel (Teams connector)
5. Add to marketing list (Marketing automation connector)
```

### 2. Error Handling in Connectors

#### Retry Logic
```markdown
If connector action fails:
- Retry up to 3 times
- Wait 5 seconds between retries
- Log error details
- Provide fallback response to user
```

#### Graceful Degradation
```markdown
If primary system unavailable:
- Switch to backup data source
- Notify user of limited functionality
- Log incident for admin review
```

### 3. Performance Optimization

#### Batch Operations
- Group multiple records into single API calls
- Use bulk operations when available
- Implement pagination for large datasets

#### Caching Strategies
- Cache frequently accessed data
- Implement smart refresh policies
- Use local storage for temporary data

## Security and Authentication

### Authentication Types

#### 1. OAuth 2.0
- **Use Case**: Microsoft services, Google, Facebook
- **Benefits**: Secure, user consent, token refresh
- **Implementation**: Built-in OAuth flow

#### 2. API Key
- **Use Case**: Simple APIs, internal systems
- **Benefits**: Easy setup, direct access
- **Security**: Store securely, rotate regularly

#### 3. Basic Authentication
- **Use Case**: Legacy systems, internal APIs
- **Security Risk**: Username/password in headers
- **Recommendation**: Use only with HTTPS

#### 4. Custom Authentication
- **Use Case**: Proprietary auth schemes
- **Implementation**: Custom headers, tokens
- **Complexity**: Requires custom code

### Security Best Practices

#### 1. Least Privilege Access
```markdown
CONNECTOR PERMISSIONS:
- Grant minimum required permissions
- Use service accounts where possible
- Regular permission audits
- Document access requirements
```

#### 2. Credential Management
- Store credentials securely in Key Vault
- Use managed identities when possible
- Implement credential rotation
- Monitor for credential exposure

#### 3. Data Encryption
- Use HTTPS for all API calls
- Encrypt sensitive data at rest
- Implement field-level encryption for PII
- Validate SSL certificates

### 4. Audit and Monitoring

#### Connector Usage Tracking
```markdown
MONITORING METRICS:
- API call volume and frequency
- Error rates by connector
- Response times and performance
- Authentication failures
- Data access patterns
```

#### Security Monitoring
- Monitor for unusual access patterns
- Alert on authentication failures
- Track data access and modifications
- Log all connector activities

## Troubleshooting Connector Issues

### Common Problems and Solutions

#### 1. Authentication Failures
**Symptoms:**
- 401 Unauthorized errors
- "Invalid credentials" messages
- Connection test failures

**Solutions:**
- Verify credentials are correct
- Check token expiration
- Validate authentication method
- Review API permissions

#### 2. Timeout Issues
**Symptoms:**
- Request timeout errors
- Slow response times
- Partial data returned

**Solutions:**
- Increase timeout settings
- Implement retry logic
- Optimize query performance
- Use pagination for large datasets

#### 3. Rate Limiting
**Symptoms:**
- 429 Too Many Requests errors
- Throttling notifications
- Degraded performance

**Solutions:**
- Implement request spacing
- Use exponential backoff
- Cache frequently accessed data
- Consider premium API tiers

#### 4. Data Format Issues
**Symptoms:**
- Parsing errors
- Invalid data type errors
- Missing field errors

**Solutions:**
- Validate API response format
- Check field mappings
- Implement data transformation
- Handle null/empty values

### Debugging Techniques

#### 1. Connection Testing
```markdown
TESTING STEPS:
1. Test connection in Power Platform
2. Verify authentication works
3. Test each operation individually
4. Check response data format
5. Validate error handling
```

#### 2. Trace Analysis
- Enable detailed logging
- Review API call traces
- Analyze response codes
- Check request/response payloads

#### 3. Performance Monitoring
- Monitor response times
- Track success/failure rates
- Analyze usage patterns
- Identify bottlenecks

## Knowledge Check

### Practical Exercises

1. **Custom Connector Development**
   - Create connector for public API (weather, news, etc.)
   - Implement authentication
   - Test all operations
   - Use in Copilot Studio agent

2. **Dataverse Integration**
   - Design business table structure
   - Configure as knowledge source
   - Create CRUD operations
   - Build complete business scenario

3. **Multi-Connector Workflow**
   - Design process using 3+ connectors
   - Implement error handling
   - Test failure scenarios
   - Document security considerations

### Assessment Questions

1. What's the difference between Standard and Premium connectors?
2. How do you implement retry logic for connector failures?
3. What authentication methods are supported for custom connectors?
4. How do you optimize connector performance for large datasets?
5. What security considerations apply to connector integrations?

## Summary

In this module, you learned:
- How to use pre-built Power Platform connectors effectively
- Creating custom connectors for external system integration
- Leveraging Dataverse as both knowledge source and data store
- Implementing secure authentication and data access patterns
- Troubleshooting and optimizing connector performance
- Best practices for enterprise connector management

### Key Takeaways
- Connectors bridge Copilot Studio with external systems
- Custom connectors enable integration with any REST API
- Dataverse provides native, high-performance data integration
- Security and authentication are critical considerations
- Proper error handling ensures reliable user experiences

### Next Steps
In Module 4, we'll dive deep into Power Automate integration, learning how to create sophisticated workflows that extend your agents' capabilities with complex business logic and multi-step processes.

## Additional Resources

- [Power Platform Connectors Documentation](https://learn.microsoft.com/en-us/connectors/)
- [Custom Connector Development Guide](https://learn.microsoft.com/en-us/connectors/custom-connectors/)
- [Dataverse Documentation](https://learn.microsoft.com/en-us/power-apps/maker/data-platform/)
- [OpenAPI Specification Guide](https://spec.openapis.org/oas/v3.0.0)
- [Connector Security Best Practices](https://learn.microsoft.com/en-us/power-platform/admin/security/overview)

---

*Continue to Module 4: Power Automate Integration to learn about building sophisticated automated workflows for your agents.*