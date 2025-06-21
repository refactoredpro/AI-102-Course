# Day 4: Azure AI Foundry Deep Dive - Lab Guide

## Today's Mission
Master the primary platform for Azure AI agent development and explore its capabilities for building intelligent agents without extensive coding.

## Learning Objectives
By the end of this lab, you will be able to:
- Navigate the Azure AI Foundry portal with confidence
- Create and manage AI agent projects
- Explore and customize pre-built agent templates
- Understand resource management and monitoring
- Deploy your first low-code agent

## Prerequisites
- Completion of Days 1-3 labs
- Active Azure subscription with credits
- Modern web browser
- Basic understanding of AI concepts

## Time Required
- Total: 2 hours
- Portal exploration: 30 minutes
- Project setup: 30 minutes
- Template exploration: 30 minutes
- First agent creation: 30 minutes

---

## Part 1: Azure AI Foundry Portal Navigation (30 minutes)

### Getting Started with AI Foundry

**1. Accessing Azure AI Foundry**
```
Navigate to: https://ai.azure.com
Sign in with your Azure credentials
```

### Portal Layout Overview
```
┌─────────────────────────────────────────────────────┐
│                 Azure AI Foundry                     │
├─────────────────────────────────────────────────────┤
│  Navigation Bar                                      │
│  ┌─────────┬──────────┬─────────┬────────────┐    │
│  │  Home   │ Projects │ Models  │ Resources   │    │
│  └─────────┴──────────┴─────────┴────────────┘    │
├─────────────────────────────────────────────────────┤
│  Main Workspace                                      │
│  ┌────────────────┐  ┌────────────────────────┐    │
│  │ Quick Actions  │  │ Recent Projects        │    │
│  │ • New Project  │  │ • Project Alpha        │    │
│  │ • Browse Models│  │ • Customer Service Bot │    │
│  │ • Templates    │  │ • Data Analyst Agent   │    │
│  └────────────────┘  └────────────────────────┘    │
└─────────────────────────────────────────────────────┘
```

### Activity 1: Portal Exploration Checklist
Navigate to each section and document:

| Section | Purpose | Key Features Found |
|---------|---------|-------------------|
| Home | Dashboard and quick access | |
| Projects | Manage AI projects | |
| Models | Browse available models | |
| Resources | Azure resource management | |
| Learn | Documentation and tutorials | |

### Understanding the Interface

**Key Areas to Explore:**

1. **Project Management**
   - Project creation workflow
   - Resource allocation
   - Team collaboration features
   - Version control integration

2. **Model Catalog**
   - Available foundation models
   - Model capabilities and limits
   - Deployment options
   - Pricing information

3. **Development Tools**
   - Code editors
   - Prompt engineering playground
   - Testing interfaces
   - Debugging tools

### Activity 2: Feature Discovery
Find and document the location of:
- [ ] Prompt flow designer
- [ ] Model deployment options
- [ ] API key management
- [ ] Usage metrics dashboard
- [ ] Cost tracking tools
- [ ] Sample datasets
- [ ] Community templates

---

## Part 2: Project Structure and Organization (30 minutes)

### Creating Your First AI Foundry Project

**Step 1: Project Creation**
```
1. Click "New Project"
2. Project Details:
   - Name: "ai-agent-lab-[yourname]"
   - Description: "Learning AI agent development"
   - Resource Group: Use existing or create new
   - Location: Choose nearest region
```

**Step 2: Project Configuration**
```
Project Settings:
├── Compute Resources
│   ├── CPU Instances
│   └── GPU Instances (if needed)
├── Storage
│   ├── Datasets
│   └── Model artifacts
├── Networking
│   ├── Private endpoints
│   └── Security settings
└── Access Control
    ├── User permissions
    └── Service principals
```

### Project Organization Best Practices

**Folder Structure:**
```
ai-agent-lab/
├── agents/
│   ├── customer-service/
│   ├── data-analyst/
│   └── task-automation/
├── prompts/
│   ├── templates/
│   └── custom/
├── data/
│   ├── training/
│   └── evaluation/
├── deployments/
│   ├── dev/
│   ├── staging/
│   └── production/
└── documentation/
    ├── architecture/
    └── api-specs/
```

### Activity 3: Project Setup
Complete the following in your new project:

1. **Create Folder Structure**
   - Set up the recommended folder hierarchy
   - Add README files to each folder
   - Configure .gitignore if using version control

2. **Configure Resources**
   - Select appropriate compute size
   - Set up storage accounts
   - Configure networking if needed

3. **Set Access Permissions**
   - Add team members (if applicable)
   - Configure role-based access
   - Set up service principals for automation

### Understanding Project Components

**Core Components:**
- **Flows**: Visual workflow designers
- **Deployments**: Model and agent endpoints
- **Datasets**: Training and test data
- **Experiments**: A/B testing and evaluation
- **Pipelines**: Automated workflows

---

## Part 3: Pre-built Agent Templates (30 minutes)

### Exploring Available Templates

Azure AI Foundry provides several pre-built templates:

1. **Customer Service Agent**
   - Multi-turn conversation handling
   - Sentiment analysis
   - Ticket creation integration
   - Knowledge base connectivity

2. **Data Analysis Agent**
   - Natural language to SQL
   - Data visualization
   - Report generation
   - Insight extraction

3. **Document Processing Agent**
   - PDF extraction
   - Form understanding
   - Summary generation
   - Multi-language support

4. **Task Automation Agent**
   - Email management
   - Calendar scheduling
   - Workflow triggers
   - API integrations

### Activity 4: Template Analysis

For each template type, complete:

| Template | Use Cases | Key Components | Customization Options |
|----------|-----------|----------------|----------------------|
| Customer Service | Help desk, FAQ | NLU, Response Gen | Tone, Knowledge Base |
| Data Analysis | BI, Reporting | Query Builder, Viz | Data Sources, Charts |
| Document Processing | Invoice, Forms | OCR, Extraction | Fields, Languages |
| Task Automation | Admin, Workflow | Triggers, Actions | Integrations, Rules |

### Deep Dive: Customer Service Template

**Architecture:**
```
User Input → Intent Recognition → Context Management → Response Generation
     ↓              ↓                    ↓                    ↓
  Text/Voice    Azure LUIS         Cosmos DB          Azure OpenAI
```

**Customization Points:**
1. **Personality and Tone**
   ```json
   {
     "personality": {
       "tone": "professional",
       "formality": "medium",
       "empathy": "high",
       "humor": "minimal"
     }
   }
   ```

2. **Knowledge Integration**
   - Connect to existing knowledge bases
   - Add custom FAQs
   - Link to documentation
   - Include product catalogs

3. **Workflow Integration**
   - CRM systems
   - Ticketing platforms
   - Email systems
   - Chat platforms

### Activity 5: Template Customization Exercise

1. **Select the Customer Service Template**
2. **Modify the Following:**
   - Change greeting message
   - Add three custom intents
   - Configure escalation rules
   - Set business hours
   - Add a knowledge source

3. **Test Your Customizations:**
   - Run sample conversations
   - Test edge cases
   - Verify escalation works
   - Check response quality

---

## Part 4: Resource Management and Monitoring (30 minutes)

### Understanding Resource Allocation

**Compute Resources:**
```
Development Environment:
├── CPU: 2 cores, 8GB RAM
├── Storage: 50GB
└── Cost: ~$50/month

Production Environment:
├── CPU: 8 cores, 32GB RAM
├── GPU: Optional for training
├── Storage: 500GB
└── Cost: ~$500/month
```

### Monitoring Dashboard Overview

**Key Metrics to Track:**
1. **Performance Metrics**
   - Response time
   - Throughput (requests/second)
   - Error rates
   - Availability

2. **Usage Metrics**
   - API calls
   - Token consumption
   - Storage usage
   - Compute hours

3. **Quality Metrics**
   - User satisfaction
   - Task completion rate
   - Escalation rate
   - Response accuracy

### Activity 6: Setting Up Monitoring

1. **Create a Monitoring Dashboard**
   ```
   Dashboard Elements:
   - Real-time API usage
   - Response time graph
   - Error log stream
   - Cost tracker
   ```

2. **Configure Alerts**
   | Alert Type | Threshold | Action |
   |------------|-----------|--------|
   | High Error Rate | >5% | Email notification |
   | Slow Response | >2 seconds | Scale up compute |
   | Budget Alert | 80% of limit | Review and optimize |
   | Low Availability | <99% | Investigate immediately |

3. **Set Up Logging**
   - Enable diagnostic logging
   - Configure log retention (30 days)
   - Set up log analytics workspace
   - Create log queries for common issues

### Cost Management Strategies

**Optimization Techniques:**
1. **Right-sizing Resources**
   - Start small, scale as needed
   - Use auto-scaling policies
   - Schedule dev environment shutdowns

2. **Efficient Model Usage**
   - Cache common responses
   - Use appropriate model sizes
   - Batch process when possible

3. **Storage Optimization**
   - Archive old logs
   - Compress large datasets
   - Use lifecycle policies

### Activity 7: Cost Analysis Exercise

Create a cost projection for your agent:

| Component | Dev Cost/Month | Prod Cost/Month | Optimization Strategy |
|-----------|----------------|-----------------|----------------------|
| Compute | | | |
| Storage | | | |
| API Calls | | | |
| Bandwidth | | | |
| **Total** | | | |

---

## Part 5: Building Your First Agent (30 minutes)

### Quick Start: FAQ Bot

**Step 1: Create New Agent**
```
1. Navigate to Projects → Your Project
2. Click "New Agent"
3. Select "Start from Template"
4. Choose "FAQ Bot"
5. Name: "lab-faq-bot"
```

**Step 2: Configure Knowledge Base**
```yaml
knowledge_base:
  - question: "What are your business hours?"
    answer: "We're open Monday-Friday, 9 AM-5 PM EST"
  
  - question: "How do I reset my password?"
    answer: "Click 'Forgot Password' on the login page"
  
  - question: "What payment methods do you accept?"
    answer: "We accept all major credit cards and PayPal"
```

**Step 3: Test Your Agent**
Test conversations:
- "When are you open?"
- "I forgot my password"
- "Do you take credit cards?"
- "Can I pay with Bitcoin?" (test unknown query)

### Activity 8: Enhance Your FAQ Bot

Add the following capabilities:

1. **Multi-intent Handling**
   - Add compound question handling
   - Implement context carryover
   - Enable clarification requests

2. **Personality Injection**
   ```python
   personality_prompt = """
   You are a helpful and friendly assistant.
   - Be concise but warm
   - Use the customer's name when provided
   - Offer additional help after answering
   """
   ```

3. **Integration Points**
   - Add email notification for complex queries
   - Log unanswered questions
   - Track satisfaction ratings

---

## Lab Completion Checklist

- [ ] Successfully navigated all portal sections
- [ ] Created and configured a project
- [ ] Explored all template types
- [ ] Set up monitoring and alerts
- [ ] Built and tested FAQ bot
- [ ] Documented cost projections
- [ ] Configured logging and diagnostics

## Troubleshooting Guide

**Common Issues:**

1. **Project Creation Fails**
   - Check subscription quotas
   - Verify resource group permissions
   - Try different region

2. **Template Won't Load**
   - Clear browser cache
   - Check network connectivity
   - Verify subscription is active

3. **Agent Testing Errors**
   - Validate JSON/YAML syntax
   - Check API key configuration
   - Review error logs

4. **High Costs**
   - Review resource utilization
   - Check for unused resources
   - Implement caching strategies

## Best Practices

**Do's:**
- ✅ Start with templates, then customize
- ✅ Test thoroughly before deploying
- ✅ Monitor costs from day one
- ✅ Document your configurations
- ✅ Use version control for prompts

**Don'ts:**
- ❌ Skip the planning phase
- ❌ Ignore security best practices
- ❌ Over-provision resources initially
- ❌ Forget to set up monitoring
- ❌ Deploy without testing

## Extension Activities

1. **Advanced Template Customization**
   - Merge two templates into a hybrid agent
   - Add custom ML models to templates
   - Create your own template

2. **Performance Optimization**
   - Implement response caching
   - Optimize prompt efficiency
   - Reduce token usage

3. **Security Hardening**
   - Implement input validation
   - Add rate limiting
   - Set up private endpoints

## Key Takeaways

✅ Azure AI Foundry provides a comprehensive low-code platform for agent development
✅ Templates accelerate development but should be customized for specific needs
✅ Proper project organization is crucial for scalability
✅ Monitoring and cost management should be implemented from the start
✅ The platform supports both simple and complex agent architectures

---

## Resources for Deeper Learning

- [Azure AI Foundry Documentation](https://docs.microsoft.com/azure/ai-foundry/)
- [Best Practices for AI Agent Development](https://aka.ms/ai-agent-best-practices)
- [Azure AI Foundry Video Tutorials](https://aka.ms/ai-foundry-videos)
- [Community Forums](https://techcommunity.microsoft.com/t5/azure-ai/bd-p/AzureAI)

## Next Steps

Tomorrow, we'll explore development approaches and tools, including the Semantic Kernel framework and pro-code options. Make sure your FAQ bot is working—we'll enhance it with code!

**Challenge**: Before tomorrow, try creating an agent using a different template and document what you learn about its structure.