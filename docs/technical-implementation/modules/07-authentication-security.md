# Module 5: Authentication & Security

## Learning Objectives
By the end of this module, you will be able to:
- Implement Single Sign-On (SSO) for Copilot Studio agents
- Configure Entra ID authentication and authorization
- Secure API plugins with proper authentication patterns
- Implement role-based access control and security policies
- Design secure data handling and privacy protection
- Monitor and audit security events and access patterns

## Prerequisites
- Completion of Modules 1-4
- Understanding of Azure Active Directory/Entra ID concepts
- Basic knowledge of OAuth 2.0 and authentication protocols
- Familiarity with security principles and best practices

## Overview

Security is fundamental to enterprise AI implementations. This module covers comprehensive authentication and security patterns for Copilot Studio, ensuring your agents operate securely while maintaining user experience and organizational compliance requirements.

## Understanding Authentication in Copilot Studio

### Authentication Architecture

```
User → Copilot Studio Agent → Authentication Provider (Entra ID)
                          ↓
                    Secure API/Resource Access
                          ↓  
                    Knowledge Sources & Actions
```

### Authentication Types

#### 1. No Authentication
- **Use Case**: Public information, general knowledge agents
- **Risk Level**: Low for public data, high for internal use
- **Implementation**: Default configuration

#### 2. Only for manual (On Demand)
- **Use Case**: Selective authentication for specific operations
- **Implementation**: User prompted when authentication required
- **Control**: Fine-grained per operation

#### 3. For any topic
- **Use Case**: All interactions require authentication
- **Security**: Highest level of control
- **User Experience**: Single sign-on seamless experience

## Lab 1: Implementing Single Sign-On (SSO)

### Scenario: Secure Employee Portal Agent
Build an agent that accesses sensitive HR information with SSO authentication.

### Step 1: Configure Authentication in Copilot Studio

1. **Navigate to Authentication Settings**
   - Open your Copilot Studio agent
   - Go to **"Settings"** tab
   - Click **"Security"** section

2. **Enable Authentication**
   - Toggle **"Require users to sign in"** to ON
   - Select authentication type: **"For any topic"**
   - Choose identity provider: **"Azure Active Directory v2"**

3. **Configure Entra ID Integration**
   - Click **"Configure"** next to Azure Active Directory v2
   - Application (client) ID: [Copy from Azure app registration]
   - Client secret: [From Azure app registration] 
   - Tenant ID: [Your organization's tenant ID]

### Step 2: Create Azure App Registration

1. **Navigate to Azure Portal**
   - Go to portal.azure.com
   - Navigate to **"Azure Active Directory"** > **"App registrations"**
   - Click **"+ New registration"**

2. **Register the Application**
   - Name: "Copilot Studio Employee Portal"
   - Account types: **"Accounts in this organizational directory only"**
   - Redirect URI: Platform **"Web"**, URL from Copilot Studio auth settings
   - Click **"Register"**

3. **Configure API Permissions**
   - Go to **"API permissions"**
   - Click **"+ Add a permission"**
   - Select **"Microsoft Graph"**
   - Add delegated permissions:
     - User.Read (basic profile)
     - User.ReadBasic.All (read other users)
     - Directory.Read.All (organizational info)
     - Calendars.ReadWrite (calendar access)
   - Click **"Grant admin consent"**

4. **Create Client Secret**
   - Go to **"Certificates & secrets"**
   - Click **"+ New client secret"**
   - Description: "Copilot Studio Integration"
   - Expires: 12 months (recommended)
   - Copy and save the secret value

### Step 3: Configure Authenticated Knowledge Sources

1. **Add SharePoint Knowledge Source with Authentication**
   - Go to **"Knowledge"** tab
   - Click **"+ Add knowledge"**
   - Select **"SharePoint"**
   - Choose **"Use authentication"**
   - Site URL: Your internal SharePoint site
   - Authentication will use user's credentials

2. **Test Authenticated Access**
   - Save and publish agent
   - Test in a browser (incognito mode)
   - Verify authentication prompt appears
   - Confirm access to protected content

### Step 4: Implement User Context in Actions

1. **Create User-Aware Power Automate Flow**
   ```json
   {
     "trigger": "When a flow is run from copilot",
     "actions": [
       {
         "name": "Get_user_profile",
         "type": "office365users",
         "inputs": {
           "userPrincipalName": "@{triggerBody()['userEmail']}"
         }
       },
       {
         "name": "Get_user_manager",
         "type": "office365users", 
         "inputs": {
           "userPrincipalName": "@{body('Get_user_profile')?['manager']?['mail']}"
         }
       },
       {
         "name": "Return_personalized_data",
         "type": "Respond to a Copilot",
         "inputs": {
           "user": "@{body('Get_user_profile')?['displayName']}",
           "department": "@{body('Get_user_profile')?['department']}",
           "manager": "@{body('Get_user_manager')?['displayName']}",
           "permissions": "@{variables('UserPermissions')}"
         }
       }
     ]
   }
   ```

2. **Add Flow to Agent**
   - Name: "Get User Context"
   - Description: "Retrieve authenticated user information and permissions"

### Step 5: Update Agent Instructions for Security

```markdown
SECURE EMPLOYEE PORTAL AGENT

You are an authenticated agent that provides personalized employee services.

SECURITY REQUIREMENTS:
- All users must authenticate before any interaction
- Access user context from authentication tokens
- Respect organizational permissions and role boundaries
- Never share information between different users' sessions

USER CONTEXT AVAILABLE:
- User email and display name
- Department and job title  
- Manager information
- Group memberships and roles

PERSONALIZATION:
- Address users by name when appropriate
- Filter information based on user's department/role
- Provide relevant services based on user permissions
- Escalate requests to appropriate managers when needed

ACTIONS AVAILABLE:
- 'Get User Context': Retrieve authenticated user information
- 'Check Permissions': Verify user access rights
- 'Access HR Data': Retrieve user-specific HR information

PRIVACY PROTECTION:
- Only access data the user is authorized to see
- Log access attempts for audit purposes
- Never cache sensitive personal information
- Respect data retention and privacy policies

RESPONSE FORMAT:
- Use personalized greetings
- Provide context-appropriate information
- Include relevant next steps and contacts
- Maintain professional, helpful tone
```

## Lab 2: Securing API Plugins with Entra ID

### Scenario: Secure API Integration
Protect custom API plugins with Entra ID authentication and proper authorization.

### Step 1: Create Protected API

For demonstration, we'll create a simple Azure Function with Entra ID protection:

```csharp
[FunctionName("GetEmployeeData")]
[Authorize] // Requires authentication
public static async Task<IActionResult> GetEmployeeData(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = "employees/{id}")] 
    HttpRequest req,
    string id,
    ILogger log)
{
    // Verify user is authenticated
    var principal = req.HttpContext.User;
    if (!principal.Identity.IsAuthenticated)
    {
        return new UnauthorizedResult();
    }

    // Check user permissions
    var userEmail = principal.FindFirst("preferred_username")?.Value;
    var userRoles = principal.FindAll("roles").Select(c => c.Value).ToArray();
    
    // Role-based access control
    if (!userRoles.Contains("HR.Read") && !userRoles.Contains("HR.Admin"))
    {
        return new ForbiddenResult();
    }

    // Return employee data based on permissions
    var employeeData = await GetEmployeeDataFromDatabase(id, userEmail, userRoles);
    return new OkObjectResult(employeeData);
}
```

### Step 2: Configure API Authentication

1. **Enable Entra ID for Azure Function**
   - In Azure Portal, go to your Function App
   - Navigate to **"Authentication"** settings
   - Click **"Add identity provider"**
   - Select **"Microsoft"**
   - Configure settings:
     - App registration type: Create new
     - Name: EmployeeDataAPI
     - Supported account types: Current tenant only
     - Restrict access: Require authentication

2. **Configure Token Validation**
   ```json
   {
     "authSettings": {
       "enabled": true,
       "defaultProvider": "AzureActiveDirectory",
       "tokenStoreEnabled": true,
       "allowedAudiences": [
         "api://your-api-app-id"
       ],
       "issuer": "https://sts.windows.net/your-tenant-id/",
       "clientId": "your-api-app-id"
     }
   }
   ```

### Step 3: Create Authenticated Custom Connector

1. **Build Custom Connector with Authentication**
   - In Power Platform, create new custom connector
   - Name: "Secure Employee API"
   - Host: your-function-app.azurewebsites.net
   - Base URL: /api

2. **Configure OAuth 2.0 Authentication**
   - Security type: **"OAuth 2.0"**
   - Identity provider: **"Azure Active Directory"**
   - Client ID: [From Azure app registration]
   - Client secret: [From Azure app registration]
   - Authorization URL: `https://login.microsoftonline.com/your-tenant-id/oauth2/v2.0/authorize`
   - Token URL: `https://login.microsoftonline.com/your-tenant-id/oauth2/v2.0/token`
   - Refresh URL: `https://login.microsoftonline.com/your-tenant-id/oauth2/v2.0/token`
   - Scope: `api://your-api-app-id/.default`

3. **Define Operations with Security Context**
   ```yaml
   operations:
     GetEmployeeData:
       summary: Get employee information
       security:
         - oauth2: ["api://your-api-app-id/.default"]
       parameters:
         - name: employeeId
           in: path
           required: true
           type: string
       responses:
         200:
           description: Employee data retrieved successfully
         401:
           description: Unauthorized - authentication required
         403:
           description: Forbidden - insufficient permissions
   ```

### Step 4: Implement Role-Based Access Control

1. **Configure App Roles in Azure**
   ```json
   {
     "appRoles": [
       {
         "id": "hr-reader-role-id",
         "allowedMemberTypes": ["User"],
         "displayName": "HR Data Reader",
         "description": "Can read basic HR information",
         "value": "HR.Read",
         "isEnabled": true
       },
       {
         "id": "hr-admin-role-id", 
         "allowedMemberTypes": ["User"],
         "displayName": "HR Administrator",
         "description": "Can read and modify HR information",
         "value": "HR.Admin",
         "isEnabled": true
       }
     ]
   }
   ```

2. **Assign Users to Roles**
   - In Azure Portal, go to **"Enterprise applications"**
   - Find your application
   - Go to **"Users and groups"**
   - Assign users to appropriate roles

3. **Validate Roles in API**
   ```csharp
   private static bool HasRequiredRole(ClaimsPrincipal user, params string[] requiredRoles)
   {
       var userRoles = user.FindAll("roles").Select(c => c.Value);
       return requiredRoles.Any(role => userRoles.Contains(role));
   }

   // Usage in API method
   if (!HasRequiredRole(principal, "HR.Read", "HR.Admin"))
   {
       return new ForbiddenResult();
   }
   ```

## Lab 3: Advanced Security Patterns

### Scenario: Multi-Tenant Secure Agent
Build an agent that works across multiple organizational units with tenant isolation.

### Step 1: Implement Tenant Isolation

1. **Design Tenant-Aware Architecture**
   ```json
   {
     "tenantConfiguration": {
       "identificationMethod": "domain",
       "tenants": [
         {
           "id": "tenant-a",
           "domain": "tenanta.company.com",
           "dataSource": "sharepoint-tenant-a",
           "permissions": ["read", "write"]
         },
         {
           "id": "tenant-b", 
           "domain": "tenantb.company.com",
           "dataSource": "sharepoint-tenant-b", 
           "permissions": ["read"]
         }
       ]
     }
   }
   ```

2. **Create Tenant Resolution Flow**
   ```json
   {
     "name": "Resolve_user_tenant",
     "actions": [
       {
         "name": "Extract_user_domain",
         "type": "Compose",
         "inputs": "@split(triggerBody()['userEmail'], '@')[1]"
       },
       {
         "name": "Lookup_tenant_config",
         "type": "Switch", 
         "expression": "@outputs('Extract_user_domain')",
         "cases": {
           "TenantA": {
             "case": "tenanta.company.com",
             "actions": ["Set_tenant_A_context"]
           },
           "TenantB": {
             "case": "tenantb.company.com", 
             "actions": ["Set_tenant_B_context"]
           }
         },
         "default": {
           "actions": ["Reject_unauthorized_tenant"]
         }
       }
     ]
   }
   ```

### Step 2: Data Loss Prevention (DLP)

1. **Configure DLP Policies**
   ```json
   {
     "dlpPolicies": {
       "preventDataLeakage": {
         "enabled": true,
         "rules": [
           {
             "name": "SSN_Detection",
             "pattern": "\\b\\d{3}-\\d{2}-\\d{4}\\b",
             "action": "block",
             "message": "Social Security Numbers cannot be shared"
           },
           {
             "name": "Credit_Card_Detection",
             "pattern": "\\b\\d{4}[\\s-]?\\d{4}[\\s-]?\\d{4}[\\s-]?\\d{4}\\b",
             "action": "redact",
             "replacement": "[REDACTED]"
           },
           {
             "name": "Internal_Email_Detection",
             "pattern": "@company\\.com\\b",
             "action": "warn",
             "allowOverride": true
           }
         ]
       }
     }
   }
   ```

2. **Implement Content Filtering**
   ```csharp
   public class ContentFilter
   {
       public static FilterResult ProcessContent(string content, User user)
       {
           var result = new FilterResult { OriginalContent = content };
           
           // Check for sensitive patterns
           foreach (var policy in DLPPolicies.GetPoliciesForUser(user))
           {
               var matches = policy.Pattern.Matches(content);
               if (matches.Any())
               {
                   switch (policy.Action)
                   {
                       case "block":
                           result.IsBlocked = true;
                           result.Reason = policy.Message;
                           break;
                       case "redact":
                           result.FilteredContent = policy.Pattern.Replace(
                               content, policy.Replacement);
                           break;
                       case "warn":
                           result.Warnings.Add(policy.Message);
                           break;
                   }
               }
           }
           
           return result;
       }
   }
   ```

### Step 3: Audit and Compliance

1. **Implement Comprehensive Logging**
   ```json
   {
     "auditSchema": {
       "timestamp": "2024-01-15T10:30:00Z",
       "userId": "user@company.com",
       "sessionId": "session-12345",
       "action": "query_employee_data",
       "resource": "sharepoint://hr-documents/employees",
       "outcome": "success",
       "ipAddress": "192.168.1.100",
       "userAgent": "Mozilla/5.0...",
       "tenantId": "tenant-a",
       "dataClassification": "confidential",
       "accessMethod": "copilot_studio",
       "permissionsUsed": ["HR.Read"],
       "dataAccessed": ["employee_name", "job_title", "department"]
     }
   }
   ```

2. **Create Audit Dashboard**
   ```json
   {
     "auditDashboard": {
       "metrics": [
         {
           "name": "Total Queries",
           "query": "COUNT(*) FROM audit_log WHERE date >= DATE_SUB(NOW(), INTERVAL 24 HOUR)"
         },
         {
           "name": "Failed Authentication Attempts", 
           "query": "COUNT(*) FROM audit_log WHERE outcome = 'auth_failed'"
         },
         {
           "name": "Sensitive Data Access",
           "query": "COUNT(*) FROM audit_log WHERE data_classification = 'confidential'"
         },
         {
           "name": "Top Users by Activity",
           "query": "SELECT user_id, COUNT(*) as activity FROM audit_log GROUP BY user_id ORDER BY activity DESC LIMIT 10"
         }
       ],
       "alerts": [
         {
           "condition": "failed_auth_attempts > 5 in 1 hour",
           "action": "notify_security_team"
         },
         {
           "condition": "sensitive_data_access > 100 in 1 hour for single user",
           "action": "flag_for_review"
         }
       ]
     }
   }
   ```

## Security Monitoring and Alerting

### 1. Real-Time Threat Detection

#### Anomaly Detection
```json
{
  "anomalyDetection": {
    "patterns": [
      {
        "name": "Unusual_Access_Volume",
        "condition": "user_queries > 3 * average_daily_queries",
        "severity": "medium",
        "action": "investigate"
      },
      {
        "name": "Off_Hours_Access",
        "condition": "access_time NOT BETWEEN '08:00' AND '18:00'",
        "severity": "low", 
        "action": "log_only"
      },
      {
        "name": "Suspicious_Geographic_Access",
        "condition": "ip_location != user_registered_location",
        "severity": "high",
        "action": "block_and_alert"
      }
    ]
  }
}
```

#### Automated Response System
```json
{
  "incidentResponse": {
    "triggers": [
      {
        "event": "multiple_failed_auth",
        "threshold": 5,
        "timeWindow": "5 minutes",
        "response": [
          "temporary_account_lock",
          "notify_user_manager", 
          "escalate_to_security"
        ]
      },
      {
        "event": "data_exfiltration_attempt",
        "indicators": ["large_data_download", "unusual_export_patterns"],
        "response": [
          "immediate_session_termination",
          "freeze_user_account",
          "emergency_security_alert"
        ]
      }
    ]
  }
}
```

### 2. Compliance Reporting

#### Automated Compliance Reports
```json
{
  "complianceReporting": {
    "reports": [
      {
        "name": "GDPR_Data_Access_Report",
        "schedule": "weekly",
        "content": [
          "personal_data_accessed",
          "user_consent_status",
          "data_retention_compliance",
          "deletion_requests_processed"
        ]
      },
      {
        "name": "SOX_Financial_Data_Access",
        "schedule": "monthly", 
        "content": [
          "financial_data_queries",
          "segregation_of_duties_compliance",
          "privileged_access_usage",
          "change_management_tracking"
        ]
      }
    ],
    "distribution": [
      "compliance_team@company.com",
      "security_team@company.com",
      "audit_committee@company.com"
    ]
  }
}
```

## Best Practices for Security Implementation

### 1. Defense in Depth

#### Multiple Security Layers
```markdown
SECURITY LAYER STRATEGY:
1. Network Security: Firewall, WAF, DDoS protection
2. Identity & Access: Strong authentication, MFA, conditional access
3. Application Security: Input validation, output encoding, secure coding
4. Data Security: Encryption at rest and in transit, field-level encryption
5. Monitoring: Real-time detection, logging, incident response
```

#### Security Configuration Checklist
```markdown
SECURITY CONFIGURATION CHECKLIST:
□ Authentication enabled for all sensitive operations
□ Role-based access control implemented
□ API endpoints secured with proper authentication
□ Data encryption configured for all sensitive data
□ Audit logging enabled for all user interactions
□ Regular security assessments scheduled
□ Incident response procedures documented
□ User access reviews performed quarterly
□ Security awareness training completed
□ Backup and recovery procedures tested
```

### 2. Secure Development Lifecycle

#### Security Testing Integration
```markdown
SECURITY TESTING PHASES:
1. Design Phase: Threat modeling, security requirements
2. Development: Static code analysis, secure coding practices
3. Testing: Dynamic security testing, penetration testing
4. Deployment: Security configuration validation
5. Operations: Continuous monitoring, security updates
```

#### Vulnerability Management
```markdown
VULNERABILITY MANAGEMENT PROCESS:
1. Regular security scans and assessments
2. Vulnerability classification and prioritization
3. Patch management and remediation tracking
4. Security metrics and KPI monitoring
5. Third-party security assessments
```

## Troubleshooting Security Issues

### Common Authentication Problems

#### 1. SSO Configuration Issues
**Symptoms:**
- Authentication failures
- Redirect loop errors
- Token validation failures

**Solutions:**
```markdown
TROUBLESHOOTING STEPS:
1. Verify Azure app registration configuration
2. Check redirect URIs match exactly
3. Validate client secrets are current
4. Confirm API permissions are granted
5. Test authentication flow step by step
```

#### 2. Permission Denied Errors
**Symptoms:**
- 403 Forbidden responses
- Limited functionality for authenticated users
- Role-based access not working

**Solutions:**
```markdown
PERMISSION TROUBLESHOOTING:
1. Verify user role assignments in Azure AD
2. Check API permissions in app registration
3. Validate role claims in JWT tokens
4. Test with different user roles
5. Review conditional access policies
```

#### 3. Token Expiration Issues
**Symptoms:**
- Intermittent authentication failures
- Session timeout errors
- Re-authentication prompts

**Solutions:**
```markdown
TOKEN MANAGEMENT:
1. Configure appropriate token lifetimes
2. Implement token refresh mechanisms
3. Handle token expiration gracefully
4. Use refresh tokens for long sessions
5. Monitor token usage patterns
```

## Knowledge Check

### Practical Security Assessment

1. **Security Architecture Review**
   - Design secure authentication flow
   - Implement role-based access control
   - Configure audit logging and monitoring
   - Document security procedures

2. **Penetration Testing Simulation**
   - Test for common vulnerabilities
   - Validate authentication bypasses
   - Check for data leakage scenarios
   - Assess monitoring and alerting

3. **Compliance Validation**
   - Create GDPR compliance checklist
   - Implement data retention policies
   - Configure automated compliance reporting
   - Document privacy controls

### Assessment Questions

1. What are the different authentication modes in Copilot Studio?
2. How do you implement role-based access control for API plugins?
3. What security considerations apply to multi-tenant scenarios?
4. How do you configure audit logging and compliance reporting?
5. What are the best practices for secure agent deployment?

## Summary

In this module, you learned:
- Comprehensive authentication patterns for Copilot Studio agents
- Single Sign-On implementation with Entra ID integration
- Securing API plugins with proper authentication and authorization
- Advanced security patterns including multi-tenancy and data loss prevention
- Security monitoring, alerting, and compliance reporting
- Best practices for enterprise security implementation

### Key Security Principles
- **Defense in Depth**: Multiple layers of security controls
- **Least Privilege**: Minimum necessary access rights
- **Zero Trust**: Verify everything, trust nothing
- **Continuous Monitoring**: Real-time threat detection and response
- **Compliance by Design**: Built-in regulatory compliance

### Next Steps
In Module 6, we'll explore Custom Engine Agents using the Teams AI Library, learning how to build sophisticated agents with advanced AI capabilities and custom logic beyond the standard Copilot Studio framework.

## Additional Resources

- [Copilot Studio Authentication Documentation](https://learn.microsoft.com/en-us/microsoft-copilot-studio/configuration-authentication-azure-ad)
- [Azure AD Authentication Flows](https://learn.microsoft.com/en-us/azure/active-directory/develop/authentication-flows-app-scenarios)
- [Power Platform Security Best Practices](https://learn.microsoft.com/en-us/power-platform/admin/security/overview)
- [Microsoft Security Development Lifecycle](https://www.microsoft.com/en-us/securityengineering/sdl)
- [Azure Security Baseline](https://learn.microsoft.com/en-us/security/benchmark/azure/)

---

*Continue to Module 6: Custom Engine Agents to learn about building advanced agents with the Teams AI Library and custom AI capabilities.*