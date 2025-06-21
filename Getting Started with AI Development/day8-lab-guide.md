# Day 8: Build Your First AI Agent - Lab Guide

## Today's Mission
Create a functional AI agent using Azure AI Foundry that can handle real conversations and provide helpful responses.

## Learning Objectives
By the end of this lab, you will be able to:
- Build a complete Q&A agent from scratch
- Customize agent personality and behavior
- Configure response patterns
- Test agent functionality thoroughly
- Deploy your agent for access

## Prerequisites
- Completion of Week 1 (Days 1-7)
- Azure AI Foundry access
- Test scenarios prepared
- Basic understanding of conversational AI

## Time Required
- Total: 2 hours
- Agent creation: 30 minutes
- Customization: 45 minutes
- Testing: 30 minutes
- Deployment: 15 minutes

---

## Part 1: Building Your Q&A Agent (30 minutes)

### Step 1: Project Setup

Navigate to Azure AI Foundry and create a new agent:

```
1. Go to your project from Day 4
2. Click "Create new agent"
3. Select "Q&A Agent" template
4. Name: "smart-qa-agent-[yourname]"
```

### Step 2: Define Agent Purpose

Configure your agent's core settings:

```yaml
Agent Configuration:
  name: "Smart Q&A Assistant"
  description: "Intelligent agent for answering user questions"
  primary_language: "English"
  supported_languages: ["Spanish", "French"]
  
Capabilities:
  - question_answering
  - context_retention
  - clarification_requests
  - source_citation
```

### Step 3: Knowledge Base Setup

Add knowledge sources:

**Option A: Upload Documents**
```
Supported formats:
- PDF documents
- Word documents
- Text files
- CSV data
- JSON structures

Upload process:
1. Click "Add Knowledge Source"
2. Select "Upload Documents"
3. Choose your files
4. Wait for processing
```

**Option B: Create FAQ Pairs**
```yaml
faqs:
  - question: "What are your business hours?"
    answer: "We're open Monday through Friday, 9 AM to 6 PM EST. 
            We're closed on weekends and major holidays."
    
  - question: "How can I track my order?"
    answer: "You can track your order by logging into your account 
            and visiting the 'Order History' section. You'll find 
            real-time tracking information there."
    
  - question: "What is your return policy?"
    answer: "We offer a 30-day return policy for unused items in 
            original packaging. Simply initiate a return through 
            your account or contact customer service."
```

**Option C: Connect External Sources**
```
Available connectors:
- SharePoint sites
- OneDrive folders
- Azure Blob Storage
- Databases (SQL)
- REST APIs
```

### Activity 1: Knowledge Base Creation

Create at least 20 Q&A pairs covering:
- Product information (5 pairs)
- Policies and procedures (5 pairs)
- Technical support (5 pairs)
- General information (5 pairs)

Template for comprehensive Q&A:
```yaml
- question: "[Primary question]"
  alternative_questions:
    - "[Variation 1]"
    - "[Variation 2]"
  answer: "[Detailed answer]"
  follow_up_prompts:
    - "Would you like more details about X?"
    - "Can I help you with Y?"
  metadata:
    category: "[Category]"
    last_updated: "[Date]"
    source: "[Source document/URL]"
```

---

## Part 2: Personality and Behavior Customization (45 minutes)

### Defining Agent Personality

Create a unique personality for your agent:

```python
personality_prompt = """
You are Alex, a knowledgeable and friendly Q&A assistant. 

Your personality traits:
- Warm and approachable, but professional
- Patient with users who need clarification
- Proactive in offering additional help
- Honest about limitations
- Slightly witty when appropriate

Communication style:
- Use clear, concise language
- Avoid jargon unless necessary
- Provide examples when helpful
- Break down complex topics
- Always maintain a positive tone

When you don't know something:
- Acknowledge the limitation honestly
- Suggest alternative resources
- Offer to help with related topics
"""
```

### Behavioral Configuration

**Response Patterns:**
```yaml
greeting_variations:
  morning:
    - "Good morning! How can I brighten your day?"
    - "Morning! What questions can I answer for you?"
  afternoon:
    - "Good afternoon! How may I assist you?"
    - "Hello there! What's on your mind today?"
  evening:
    - "Good evening! How can I help?"
    - "Evening! What can I do for you?"

uncertainty_responses:
  - "I'm not entirely certain about that. Let me find the most accurate information for you."
  - "That's a great question! While I don't have a definitive answer, I can suggest..."
  - "I want to make sure I give you the right information. Could you provide more context?"

clarification_prompts:
  - "To better help you, could you tell me more about..."
  - "I want to make sure I understand correctly. Are you asking about..."
  - "That could mean a few things. Are you referring to..."
```

### Advanced Behavior Settings

**Context Management:**
```python
context_settings = {
    "memory_window": 10,  # Remember last 10 exchanges
    "context_relevance_threshold": 0.7,
    "topic_switching_detection": True,
    "conversation_summary_enabled": True
}

# Context retention rules
context_rules = """
1. Remember user preferences mentioned in conversation
2. Track topics discussed to avoid repetition  
3. Maintain context across related questions
4. Summarize long conversations for clarity
5. Reset context on explicit topic change
"""
```

**Response Customization:**
```python
response_config = {
    "max_response_length": 300,
    "include_sources": True,
    "formatting_enabled": True,
    "emoji_usage": "minimal",
    "technical_level": "adaptive"
}

# Adaptive response examples
adaptive_responses = {
    "technical_user": "Here's the detailed technical explanation...",
    "beginner_user": "Let me explain this in simple terms...",
    "business_user": "From a business perspective..."
}
```

### Activity 2: Personality Testing

Test your agent's personality with these scenarios:

1. **Friendly Greeting Test**
   - "Hi there!"
   - "Good morning"
   - "Hey"

2. **Confusion Handling**
   - "I don't understand"
   - "That's not what I meant"
   - "Can you explain differently?"

3. **Edge Cases**
   - Nonsense input: "aksjdfkajsdf"
   - Emotional input: "I'm frustrated!"
   - Off-topic: "What's the meaning of life?"

Document responses and refine personality as needed.

---

## Part 3: Comprehensive Testing (30 minutes)

### Test Scenario Framework

Create systematic test cases:

**Category 1: Basic Functionality**
```yaml
test_cases:
  - id: "basic_001"
    input: "What are your hours?"
    expected_contains: ["Monday", "Friday", "9 AM", "6 PM"]
    
  - id: "basic_002"
    input: "How do I contact support?"
    expected_contains: ["email", "phone", "chat"]
    
  - id: "basic_003"
    input: "Tell me about your products"
    expected_behavior: "Lists main product categories"
```

**Category 2: Context Retention**
```yaml
conversation_flow:
  - user: "I need help with my order"
    agent_should: "Ask for order details"
  
  - user: "It's order #12345"
    agent_should: "Remember order number in follow-ups"
  
  - user: "What's the status?"
    agent_should: "Reference order #12345 without re-asking"
```

**Category 3: Error Handling**
```yaml
edge_cases:
  - empty_input: ""
    expected: "Polite prompt for input"
    
  - gibberish: "asdlkfj aslkdfj"
    expected: "Graceful confusion handling"
    
  - offensive: "[inappropriate content]"
    expected: "Professional redirect"
    
  - out_of_scope: "Can you write code?"
    expected: "Clear scope explanation"
```

### Testing Methodology

**1. Functional Testing Checklist:**
- [ ] All FAQ pairs return correct answers
- [ ] Alternative phrasings work
- [ ] Follow-up suggestions appear
- [ ] Sources are cited when configured
- [ ] Response length is appropriate

**2. Conversation Flow Testing:**
- [ ] Multi-turn conversations work
- [ ] Context is maintained
- [ ] Topic switches are handled
- [ ] Clarifications work properly
- [ ] Memory window functions correctly

**3. Performance Testing:**
- [ ] Response time < 2 seconds
- [ ] Handles concurrent users
- [ ] Degrades gracefully under load
- [ ] Error rate < 1%
- [ ] Availability > 99.9%

### Activity 3: Test Documentation

Create a test report:

```markdown
# Q&A Agent Test Report

## Test Summary
- Date: [Date]
- Agent Version: [Version]
- Total Tests: [Number]
- Pass Rate: [Percentage]

## Functional Tests
| Test Case | Result | Notes |
|-----------|---------|--------|
| FAQ Accuracy | ✅ Pass | All questions answered correctly |
| Context Retention | ✅ Pass | Maintains context for 10 turns |
| Error Handling | ⚠️ Partial | Needs improvement on edge cases |

## Performance Metrics
- Average Response Time: [X] ms
- 95th Percentile: [X] ms
- Error Rate: [X]%
- Throughput: [X] requests/second

## Issues Found
1. [Issue description and severity]
2. [Issue description and severity]

## Recommendations
- [Improvement suggestion]
- [Optimization opportunity]
```

---

## Part 4: Deployment and Access (15 minutes)

### Deployment Options

**Option 1: Web Widget**
```html
<!-- Embed code for website -->
<div id="qa-agent-widget"></div>
<script src="https://ai.azure.com/embed/[your-agent-id].js"></script>
<script>
  AzureAIAgent.init({
    agentId: '[your-agent-id]',
    theme: 'light',
    position: 'bottom-right',
    welcomeMessage: 'Hi! How can I help you today?'
  });
</script>
```

**Option 2: API Endpoint**
```python
import requests

# Agent API configuration
agent_endpoint = "https://[your-instance].azure-ai.com/agents/[agent-id]/chat"
api_key = "your-api-key"

# Send message
response = requests.post(
    agent_endpoint,
    headers={"Authorization": f"Bearer {api_key}"},
    json={"message": "What are your business hours?"}
)

print(response.json()["reply"])
```

**Option 3: Teams Integration**
```yaml
teams_app_manifest:
  name: "Q&A Assistant"
  description: "Intelligent Q&A bot for Teams"
  bot_id: "[your-bot-id]"
  commands:
    - title: "ask"
      description: "Ask a question"
    - title: "help"
      description: "Get help using the bot"
```

### Deployment Configuration

```yaml
deployment_settings:
  environment: "production"
  scaling:
    min_instances: 1
    max_instances: 10
    scale_trigger: "cpu > 70%"
  
  security:
    authentication: "api_key"
    rate_limiting: "100 requests/minute"
    ip_whitelist: []
  
  monitoring:
    application_insights: true
    custom_metrics: true
    alerts:
      - metric: "response_time"
        threshold: "3000ms"
        action: "email_notification"
```

### Activity 4: Deploy Your Agent

1. Choose deployment method
2. Configure security settings
3. Set up monitoring
4. Test deployed endpoint
5. Document access instructions

**Deployment Checklist:**
- [ ] Endpoint URL documented
- [ ] API keys securely stored
- [ ] Rate limits configured
- [ ] Monitoring enabled
- [ ] Backup plan created
- [ ] Documentation updated

---

## Lab Completion Checklist

- [ ] Q&A agent created successfully
- [ ] 20+ Q&A pairs configured
- [ ] Personality customized
- [ ] Behavior patterns defined
- [ ] Comprehensive testing completed
- [ ] Test report documented
- [ ] Agent deployed
- [ ] Access instructions created

## Troubleshooting Guide

**Common Issues:**

1. **Agent Not Responding**
   - Check knowledge base processing status
   - Verify API keys are correct
   - Ensure endpoints are accessible
   - Review error logs

2. **Incorrect Answers**
   - Review Q&A pair matching
   - Check confidence thresholds
   - Verify knowledge base quality
   - Update training data

3. **Slow Performance**
   - Optimize knowledge base size
   - Enable response caching
   - Check resource allocation
   - Review query complexity

4. **Context Loss**
   - Verify memory settings
   - Check session management
   - Review context window size
   - Test conversation ID tracking

## Best Practices

**Do's:**
- ✅ Test with real user scenarios
- ✅ Monitor usage patterns
- ✅ Iterate based on feedback
- ✅ Document all configurations
- ✅ Plan for scaling

**Don'ts:**
- ❌ Skip testing edge cases
- ❌ Ignore error messages
- ❌ Overlook security settings
- ❌ Forget about maintenance
- ❌ Assume perfect accuracy

## Extension Activities

1. **Multi-language Support**
   - Add Spanish Q&A pairs
   - Test language detection
   - Configure translation

2. **Advanced Analytics**
   - Track popular questions
   - Identify knowledge gaps
   - Measure satisfaction

3. **Integration Expansion**
   - Connect to CRM
   - Add email notifications
   - Implement escalation

## Key Takeaways

✅ Building AI agents involves iterative refinement
✅ Personality significantly impacts user experience
✅ Thorough testing prevents production issues
✅ Deployment requires security and monitoring
✅ Real-world usage provides best feedback

---

## Resources

- [Azure AI Agent Best Practices](https://docs.microsoft.com/azure/ai)
- [Conversation Design Guidelines](https://docs.microsoft.com/conversational-ai)
- [Testing Strategies for AI](https://docs.microsoft.com/ai-testing)

## Next Steps

Tomorrow, we'll enhance your agent with:
- Custom tool integrations
- External API connections
- Advanced workflows
- Dynamic responses

**Homework:** Use your agent for real tasks and note improvement areas!