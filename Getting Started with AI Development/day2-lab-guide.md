# Day 2: Azure AI Services Ecosystem - Lab Guide

## Today's Mission
Navigate Azure's AI playground without getting lost and understand how each service contributes to building intelligent agents.

## Learning Objectives
By the end of this lab, you will be able to:
- Set up and navigate the Azure portal effectively
- Identify and understand core Azure AI services
- Estimate costs using the Azure pricing calculator
- Create your first Azure resources
- Understand service integration patterns

## Prerequisites
- Completion of Day 1 lab
- Valid email address for Azure account
- Credit card (for identity verification - won't be charged)
- Basic understanding of cloud computing concepts

## Time Required
- Total: 1.5-2 hours
- Account setup: 20 minutes
- Portal exploration: 40 minutes
- Service overview: 30 minutes
- Pricing exercise: 30 minutes

## Lab Setup Requirements
- Modern web browser (Chrome, Edge, Firefox)
- Stable internet connection
- Note-taking application
- Calculator or spreadsheet for pricing exercises

---

## Part 1: Azure Account Setup (20 minutes)

### Step-by-Step Account Creation

**1. Navigate to Azure Free Account Page**
- Go to: https://azure.microsoft.com/free/
- Click "Start free"
- Have your credit card ready (verification only)

**2. Account Creation Process**
```
Personal Information:
- Email: [Use a permanent email]
- Phone: [For verification]
- Country: [Your location]

Verification:
- Credit card (not charged)
- Phone verification code
- Identity confirmation
```

**3. Free Tier Benefits**
Document what you receive:
- $200 credit for 30 days
- 12 months of free services
- Always free services
- No automatic charges

### Activity 1: Account Verification
After setup, confirm access to:
- [ ] Azure Portal (portal.azure.com)
- [ ] Billing section showing $200 credit
- [ ] Subscription details
- [ ] Resource group creation ability

**Troubleshooting Tips:**
- Use a personal email if work email has restrictions
- Try different browser if verification fails
- Ensure pop-up blocker is disabled
- Contact Azure support if credit doesn't appear

---

## Part 2: Azure Portal Navigation (40 minutes)

### Portal Orientation

**1. Dashboard Customization**
- Pin frequently used services
- Create custom dashboards
- Set up resource shortcuts

**2. Key Navigation Areas**
```
Home
├── Create a resource
├── All services
├── Recent resources
└── Azure services

Resource Groups
├── Organization containers
├── Access control
└── Cost management

Subscriptions
├── Free tier status
├── Usage + quotas
└── Billing
```

### Activity 2: Portal Scavenger Hunt
Find and document the location of:

| Service | Portal Location | Purpose for AI Agents |
|---------|----------------|----------------------|
| Azure OpenAI | Cognitive Services | LLM capabilities |
| Computer Vision | Cognitive Services | Image analysis |
| Speech Services | Cognitive Services | Voice interaction |
| Language Understanding | Cognitive Services | Intent recognition |
| Azure Machine Learning | AI + Machine Learning | Custom models |
| Logic Apps | Integration | Workflow automation |
| Functions | Compute | Serverless execution |
| Service Bus | Integration | Message queuing |

### Portal Best Practices
1. **Use Search**: Cmd/Ctrl + / for quick navigation
2. **Bookmark Services**: Pin to dashboard
3. **Tag Resources**: Use consistent naming
4. **Monitor Costs**: Set up cost alerts early

---

## Part 3: Azure AI Services Deep Dive (30 minutes)

### Service Categories and Capabilities

**1. Azure Cognitive Services**

**Vision**
- Computer Vision API
  - Object detection
  - OCR capabilities
  - Image analysis
- Face API
  - Face detection
  - Emotion recognition
  - Identity verification
- Custom Vision
  - Train custom models
  - Edge deployment

**Speech**
- Speech-to-Text
  - Real-time transcription
  - Batch transcription
  - Custom models
- Text-to-Speech
  - Neural voices
  - Custom voice
  - SSML support

**Language**
- Text Analytics
  - Sentiment analysis
  - Key phrase extraction
  - Language detection
- Translator
  - 100+ languages
  - Real-time translation
  - Document translation
- LUIS (Language Understanding)
  - Intent recognition
  - Entity extraction
  - Conversational AI

**Decision**
- Personalizer
  - Recommendation engine
  - A/B testing
  - Reinforcement learning

### Activity 3: Service Exploration
For each category, complete:

```
Service: [Name]
Free Tier: [Limits/availability]
Use Case: [Agent application]
Integration: [How it connects to agents]
Pricing Model: [Per request/hour/month]
```

**2. Azure OpenAI Service**

**Available Models**
- GPT-4 (latest capabilities)
- GPT-3.5 (cost-effective)
- DALL-E (image generation)
- Whisper (speech recognition)

**Key Features**
- Fine-tuning support
- Content filtering
- Enterprise security
- Regional deployment

### Activity 4: OpenAI Service Setup
1. Request access (if needed)
2. Understand quotas and limits
3. Review responsible AI policies
4. Note regional availability

**3. Azure Machine Learning**

**Core Components**
- Workspaces
- Compute resources
- Datasets
- Models
- Endpoints

**For AI Agents**
- Custom model training
- Real-time inference
- Batch predictions
- Model versioning

---

## Part 4: Cost Analysis Workshop (30 minutes)

### Understanding Azure Pricing

**Pricing Models**
1. **Consumption-based**: Pay for what you use
2. **Reserved Instances**: Commitment discounts
3. **Hybrid Benefits**: Use existing licenses
4. **Free Tiers**: Limited usage at no cost

### Activity 5: Pricing Calculator Exercise

**Scenario**: Calculate monthly costs for a customer service AI agent

**Requirements**:
- 10,000 conversations/month
- Average 10 messages per conversation
- Speech-to-text for 20% of interactions
- Language translation for 10% of messages
- Sentiment analysis on all messages

**Step-by-Step Calculation**:

1. **Navigate to Pricing Calculator**
   - https://azure.microsoft.com/pricing/calculator/

2. **Add Services**:
   ```
   Azure OpenAI Service
   - Model: GPT-3.5
   - Tokens: Calculate based on messages
   
   Speech Services
   - Speech-to-text: 2,000 conversations
   - Estimate 1 minute per conversation
   
   Translator
   - 10,000 messages to translate
   - Average 50 words per message
   
   Text Analytics
   - 100,000 sentiment analyses
   ```

3. **Document Findings**:
   | Service | Units | Cost per Unit | Monthly Cost |
   |---------|-------|---------------|--------------|
   | OpenAI GPT-3.5 | X tokens | $Y/1K tokens | $Z |
   | Speech-to-text | X minutes | $Y/minute | $Z |
   | Translator | X characters | $Y/million | $Z |
   | Sentiment | X transactions | $Y/1K | $Z |
   | **Total** | | | **$Total** |

### Cost Optimization Strategies
1. **Use Free Tiers**: Maximize free allocations
2. **Batch Processing**: Reduce per-transaction costs
3. **Caching**: Store repeated results
4. **Model Selection**: Balance capability vs. cost
5. **Regional Deployment**: Choose cost-effective regions

### Activity 6: Cost Comparison
Compare costs for:
- Small startup (1,000 conversations/month)
- Medium business (50,000 conversations/month)
- Enterprise (500,000 conversations/month)

Identify break-even points for:
- Reserved capacity
- Commitment tiers
- Hybrid architectures

---

## Part 5: Hands-On Resource Creation (20 minutes)

### Creating Your First Resources

**1. Create a Resource Group**
```
Name: rg-ai-agents-lab
Region: [Choose closest to you]
Tags: 
  - Environment: Development
  - Course: AI-Agents
  - Day: 2
```

**2. Deploy a Cognitive Service**
- Service: Text Analytics
- Pricing Tier: Free (F0)
- Location: Same as resource group

**3. Explore the Deployed Service**
- Find API keys
- Review endpoints
- Test in API console
- Check metrics dashboard

### Activity 7: Service Testing
Using the Azure portal's built-in testing tools:

1. **Text Analytics Test**:
   - Input: "I love learning about AI agents!"
   - Document: Sentiment score
   - Try negative: "This is frustrating"
   - Try neutral: "The sky is blue"

2. **Document Results**:
   ```json
   {
     "sentiment": "positive",
     "confidenceScores": {
       "positive": 0.99,
       "neutral": 0.01,
       "negative": 0.00
     }
   }
   ```

---

## Lab Completion Checklist

- [ ] Azure account created and verified
- [ ] Portal navigation completed
- [ ] AI services catalog documented
- [ ] Pricing calculation exercise done
- [ ] Cost comparison completed
- [ ] Resource group created
- [ ] First AI service deployed
- [ ] API testing successful

## Troubleshooting Guide

**Account Issues**
- **Credit not showing**: Wait 5-10 minutes, refresh billing page
- **Verification failing**: Try different browser or incognito mode
- **Region restrictions**: Choose supported regions for services

**Portal Navigation**
- **Can't find service**: Use search bar with exact name
- **Access denied**: Check subscription and permissions
- **Resource creation fails**: Verify quotas and limits

**Pricing Calculator**
- **Service not listed**: May be in preview or renamed
- **Prices seem wrong**: Check region and currency settings
- **Missing options**: Some features may require enterprise agreement

## Best Practices Summary

**Do's:**
- ✅ Use resource groups for organization
- ✅ Tag everything consistently
- ✅ Set up cost alerts immediately
- ✅ Document API keys securely
- ✅ Use free tiers while learning

**Don'ts:**
- ❌ Share API keys in code
- ❌ Forget to delete unused resources
- ❌ Ignore cost notifications
- ❌ Use production tier for testing
- ❌ Skip the documentation

## Extension Activities

**For the Ambitious:**
1. Set up Azure CLI and create resources via command line
2. Explore ARM templates for infrastructure as code
3. Create a cost alert for $10 spending
4. Compare Azure AI services with competitors
5. Set up a basic CI/CD pipeline with Azure DevOps

## Resources for Deeper Learning

**Essential Documentation:**
- [Azure AI Services Documentation](https://docs.microsoft.com/azure/cognitive-services/)
- [Azure Pricing Documentation](https://azure.microsoft.com/pricing/)
- [Azure Architecture Center](https://docs.microsoft.com/azure/architecture/)

**Helpful Tools:**
- Azure Mobile App (monitor on the go)
- Azure Storage Explorer
- Visual Studio Code with Azure extensions
- Postman for API testing

## Key Takeaways

✅ Azure provides comprehensive AI services that work together like Lego blocks
✅ Understanding pricing early prevents expensive surprises
✅ The portal is powerful but requires practice to navigate efficiently
✅ Free tiers are generous enough for learning and prototyping
✅ Proper organization (resource groups, tags) saves time later

---

## Reflection and Planning

Before Day 3, consider:
1. Which Azure AI service excites you most for agent development?
2. How might costs scale for your intended use case?
3. What integration patterns are emerging from your exploration?
4. Which services might you combine for your capstone project?

## Next Steps

Tomorrow, we'll dive into agent architecture and design patterns. You'll learn how to combine these Azure services into intelligent, autonomous systems. Make sure your Azure account is fully set up - we'll start building soon!

**Pro Tip**: Spend 10 extra minutes exploring one service deeply rather than skimming all services superficially. Deep knowledge of one service often transfers to others.