# Day 5: Development Approaches and Tools - Lab Guide

## Today's Mission
Choose the right development path for your needs and master the tools that will power your AI agent development journey.

## Learning Objectives
By the end of this lab, you will be able to:
- Compare low-code vs. pro-code development strategies
- Set up and use the Semantic Kernel framework
- Understand Azure AI Agent Service capabilities
- Choose appropriate SDKs for your projects
- Build agents using different development approaches

## Prerequisites
- Completion of Days 1-4 labs
- Development environment (VS Code recommended)
- Python 3.8+ or .NET 6+ installed
- Git for version control
- Azure AI Foundry project from Day 4

## Time Required
- Total: 2 hours
- Development approach comparison: 30 minutes
- Semantic Kernel setup: 45 minutes
- SDK exploration: 30 minutes
- Hands-on coding: 15 minutes

---

## Part 1: Development Approaches Comparison (30 minutes)

### Understanding Your Options

```
┌─────────────────────────────────────────────────────────────┐
│              AI Agent Development Spectrum                   │
│                                                              │
│  Low-Code                                      Pro-Code      │
│     ◄──────────────────────────────────────────────►        │
│                                                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  │
│  │   Drag   │  │  Prompt  │  │    SDK   │  │  Custom  │  │
│  │  & Drop  │  │   Flow   │  │  Based   │  │   Code   │  │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘  │
│                                                              │
│  Fastest ←─────────────────────────────────→ Most Flexible │
└─────────────────────────────────────────────────────────────┘
```

### Approach Comparison Matrix

| Aspect | Low-Code | Hybrid | Pro-Code |
|--------|----------|---------|----------|
| **Time to Market** | Days | Weeks | Months |
| **Customization** | Limited | Moderate | Unlimited |
| **Maintenance** | Platform managed | Shared | Self-managed |
| **Scaling** | Platform limits | Flexible | Full control |
| **Cost** | Per usage | Mixed | Infrastructure |
| **Skills Required** | Business analyst | Developer | Engineer |
| **debugging** | Visual tools | Mixed | Full debugging |

### Activity 1: Decision Framework

Answer these questions to determine your approach:

1. **Project Timeline**
   - [ ] Need POC in days → Low-code
   - [ ] Few weeks available → Hybrid
   - [ ] Months for development → Pro-code

2. **Customization Needs**
   - [ ] Standard patterns → Low-code
   - [ ] Some custom logic → Hybrid
   - [ ] Unique requirements → Pro-code

3. **Team Skills**
   - [ ] Non-technical → Low-code
   - [ ] Some coding → Hybrid
   - [ ] Strong engineering → Pro-code

4. **Integration Requirements**
   - [ ] Standard APIs → Low-code
   - [ ] Custom protocols → Hybrid
   - [ ] Complex systems → Pro-code

### When to Use Each Approach

**Low-Code (Azure AI Foundry)**
```yaml
Perfect for:
  - Proof of concepts
  - Standard chatbots
  - FAQ systems
  - Simple workflows
  
Example use cases:
  - HR help desk bot
  - Product recommendation agent
  - Basic customer service
  
Limitations:
  - Template constraints
  - Limited customization
  - Platform dependencies
```

**Hybrid (Prompt Flow + Code)**
```yaml
Perfect for:
  - Complex conversations
  - Custom integrations
  - Moderate scale
  
Example use cases:
  - Multi-step wizards
  - Data analysis agents
  - Process automation
  
Benefits:
  - Visual debugging
  - Code flexibility
  - Faster development
```

**Pro-Code (Semantic Kernel/Custom)**
```yaml
Perfect for:
  - Enterprise solutions
  - High-performance needs
  - Complex architectures
  
Example use cases:
  - Trading systems
  - Medical diagnosis
  - Autonomous agents
  
Advantages:
  - Full control
  - Custom optimization
  - Any integration
```

---

## Part 2: Semantic Kernel Deep Dive (45 minutes)

### What is Semantic Kernel?

Semantic Kernel (SK) is Microsoft's open-source SDK that integrates AI models into applications with:
- Plugin architecture
- Memory management
- Planning capabilities
- Multi-model orchestration

### Setting Up Semantic Kernel

**Option 1: Python Setup**

```bash
# Create virtual environment
python -m venv sk-env
source sk-env/bin/activate  # On Windows: sk-env\Scripts\activate

# Install Semantic Kernel
pip install semantic-kernel
pip install python-dotenv
pip install azure-identity
```

**Option 2: .NET Setup**

```bash
# Create new console app
dotnet new console -n SKAgentLab
cd SKAgentLab

# Add Semantic Kernel packages
dotnet add package Microsoft.SemanticKernel
dotnet add package Microsoft.SemanticKernel.Connectors.OpenAI
```

### Activity 2: First Semantic Kernel Agent

**Python Implementation:**

```python
# File: sk_agent.py
import asyncio
from semantic_kernel import Kernel
from semantic_kernel.connectors.ai.open_ai import AzureChatCompletion
from semantic_kernel.contents import ChatMessageContent
from semantic_kernel.functions import kernel_function

class SimpleAgent:
    def __init__(self):
        # Initialize kernel
        self.kernel = Kernel()
        
        # Add Azure OpenAI service
        self.kernel.add_service(
            AzureChatCompletion(
                deployment_name="gpt-35-turbo",
                endpoint="YOUR_ENDPOINT",
                api_key="YOUR_API_KEY"
            )
        )
    
    @kernel_function(
        name="analyze_sentiment",
        description="Analyzes sentiment of user input"
    )
    async def analyze_sentiment(self, text: str) -> str:
        prompt = f"Analyze the sentiment of: {text}"
        response = await self.kernel.invoke_prompt_async(prompt)
        return str(response)
    
    @kernel_function(
        name="generate_response",
        description="Generates appropriate response"
    )
    async def generate_response(self, sentiment: str, text: str) -> str:
        prompt = f"""
        User sentiment: {sentiment}
        User message: {text}
        Generate an appropriate, empathetic response.
        """
        response = await self.kernel.invoke_prompt_async(prompt)
        return str(response)

# Test the agent
async def main():
    agent = SimpleAgent()
    
    # Test message
    user_message = "I'm frustrated with the service"
    
    # Analyze sentiment
    sentiment = await agent.analyze_sentiment(user_message)
    print(f"Sentiment: {sentiment}")
    
    # Generate response
    response = await agent.generate_response(sentiment, user_message)
    print(f"Agent: {response}")

if __name__ == "__main__":
    asyncio.run(main())
```

**.NET Implementation:**

```csharp
// File: Program.cs
using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.ChatCompletion;
using Microsoft.SemanticKernel.Connectors.OpenAI;

class SimpleAgent
{
    private readonly Kernel _kernel;
    
    public SimpleAgent()
    {
        var builder = Kernel.CreateBuilder();
        
        builder.AddAzureOpenAIChatCompletion(
            deploymentName: "gpt-35-turbo",
            endpoint: "YOUR_ENDPOINT",
            apiKey: "YOUR_API_KEY"
        );
        
        _kernel = builder.Build();
    }
    
    public async Task<string> AnalyzeSentiment(string text)
    {
        var prompt = $"Analyze the sentiment of: {text}";
        var result = await _kernel.InvokePromptAsync(prompt);
        return result.ToString();
    }
    
    public async Task<string> GenerateResponse(string sentiment, string text)
    {
        var prompt = $@"
        User sentiment: {sentiment}
        User message: {text}
        Generate an appropriate, empathetic response.";
        
        var result = await _kernel.InvokePromptAsync(prompt);
        return result.ToString();
    }
}

class Program
{
    static async Task Main()
    {
        var agent = new SimpleAgent();
        var userMessage = "I'm frustrated with the service";
        
        var sentiment = await agent.AnalyzeSentiment(userMessage);
        Console.WriteLine($"Sentiment: {sentiment}");
        
        var response = await agent.GenerateResponse(sentiment, userMessage);
        Console.WriteLine($"Agent: {response}");
    }
}
```

### Understanding Semantic Kernel Components

**1. Kernel**
- Central orchestrator
- Manages services and plugins
- Handles function execution

**2. Services**
- AI model connections
- External API integrations
- Custom services

**3. Functions**
- Semantic functions (prompts)
- Native functions (code)
- Chainable operations

**4. Memory**
- Short-term (conversation)
- Long-term (embeddings)
- External stores

### Activity 3: Building a Plugin

Create a weather plugin for your agent:

```python
# weather_plugin.py
from semantic_kernel.functions import kernel_function
import aiohttp

class WeatherPlugin:
    @kernel_function(
        name="get_weather",
        description="Gets current weather for a city"
    )
    async def get_weather(self, city: str) -> str:
        # In production, use real weather API
        # This is mock data for demonstration
        weather_data = {
            "Seattle": "Rainy, 15°C",
            "Phoenix": "Sunny, 35°C",
            "New York": "Cloudy, 20°C"
        }
        
        return weather_data.get(
            city, 
            f"Weather data not available for {city}"
        )
    
    @kernel_function(
        name="get_forecast",
        description="Gets weather forecast"
    )
    async def get_forecast(self, city: str, days: int = 3) -> str:
        # Mock forecast data
        return f"{days}-day forecast for {city}: Variable conditions"

# Register plugin with kernel
kernel.add_plugin(
    WeatherPlugin(),
    plugin_name="weather"
)
```

---

## Part 3: Azure AI Agent Service Exploration (30 minutes)

### Understanding Agent Service Architecture

```
┌─────────────────────────────────────────────────────┐
│            Azure AI Agent Service                    │
│                                                      │
│  ┌───────────────┐    ┌──────────────────┐         │
│  │   Agent       │    │   Runtime        │         │
│  │   Definition  │───▶│   Environment    │         │
│  └───────────────┘    └──────────────────┘         │
│                               │                      │
│  ┌───────────────┐    ┌──────────────────┐         │
│  │   Triggers    │    │   Scalability    │         │
│  │   & Events    │───▶│   Management     │         │
│  └───────────────┘    └──────────────────┘         │
│                                                      │
│  ┌─────────────────────────────────────────┐       │
│  │         Integrations & Connectors        │       │
│  └─────────────────────────────────────────┘       │
└─────────────────────────────────────────────────────┘
```

### Key Features

**1. Managed Infrastructure**
- Auto-scaling
- High availability
- Security compliance
- Performance monitoring

**2. Built-in Integrations**
- Microsoft 365
- Dynamics 365
- Power Platform
- Third-party APIs

**3. Development Tools**
- Visual designers
- Testing frameworks
- Deployment pipelines
- Version control

### Activity 4: Agent Service Comparison

| Feature | Semantic Kernel | Agent Service | Custom Code |
|---------|----------------|---------------|-------------|
| Setup Time | 1 hour | 10 minutes | Days |
| Flexibility | High | Medium | Unlimited |
| Maintenance | Self-managed | Managed | Self-managed |
| Cost Model | Infrastructure | Per execution | Infrastructure |
| Best For | Complex logic | Standard agents | Unique needs |

### Creating an Agent with Agent Service

```yaml
# agent-definition.yaml
name: CustomerSupportAgent
version: 1.0.0
description: Handles customer inquiries

triggers:
  - type: http
    path: /chat
  - type: event
    source: ServiceBus
    topic: customer-messages

capabilities:
  - natural_language_understanding
  - sentiment_analysis
  - knowledge_retrieval
  - ticket_creation

integrations:
  - name: dynamics_crm
    type: Dynamics365
    entity: tickets
  - name: knowledge_base
    type: SharePoint
    site: support_docs

behaviors:
  greeting:
    trigger: conversation_start
    action: |
      Hello! I'm here to help with your questions.
      How can I assist you today?
  
  escalation:
    trigger: negative_sentiment_high
    action: create_ticket_and_notify_human
```

---

## Part 4: SDK Options and Language Support (15 minutes)

### Available SDKs

**1. Python SDK**
```python
# Installation
pip install azure-ai-agent-sdk

# Features
- Async/await support
- Type hints
- Rich ecosystem
- Data science integration
```

**2. .NET SDK**
```csharp
// Installation
dotnet add package Azure.AI.Agent

// Features
- Strong typing
- LINQ integration  
- Enterprise features
- Performance
```

**3. JavaScript/TypeScript SDK**
```javascript
// Installation
npm install @azure/ai-agent

// Features
- Browser compatible
- Node.js support
- React integration
- Real-time capabilities
```

**4. Java SDK**
```java
// Installation
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-ai-agent</artifactId>
</dependency>

// Features
- Enterprise Java
- Spring integration
- Reactive streams
```

### Activity 5: SDK Selection Guide

Answer these to choose your SDK:

1. **Primary Use Case**
   - Web frontend → JavaScript/TypeScript
   - Backend API → Python/.NET/Java
   - Data analysis → Python
   - Enterprise → .NET/Java

2. **Team expertise**
   - What languages does your team know?
   - What's your existing tech stack?

3. **Performance Requirements**
   - High throughput → .NET/Java
   - Rapid development → Python
   - Real-time → JavaScript

4. **Ecosystem Needs**
   - ML libraries → Python
   - Enterprise tools → .NET/Java
   - Web frameworks → JavaScript

### Quick Start Templates

**Python Fast Start:**
```python
from azure.ai.agent import Agent, AgentBuilder

# Build agent
agent = AgentBuilder() \
    .with_name("QuickBot") \
    .with_model("gpt-35-turbo") \
    .with_system_prompt("You are a helpful assistant") \
    .build()

# Use agent
response = await agent.chat("Hello!")
print(response.content)
```

**.NET Fast Start:**
```csharp
using Azure.AI.Agent;

// Build agent
var agent = new AgentBuilder()
    .WithName("QuickBot")
    .WithModel("gpt-35-turbo")
    .WithSystemPrompt("You are a helpful assistant")
    .Build();

// Use agent
var response = await agent.ChatAsync("Hello!");
Console.WriteLine(response.Content);
```

---

## Lab Completion Checklist

- [ ] Compared development approaches
- [ ] Installed Semantic Kernel
- [ ] Built first SK agent
- [ ] Created custom plugin
- [ ] Explored Agent Service
- [ ] Evaluated SDK options
- [ ] Ran quick start template

## Troubleshooting Guide

**Installation Issues:**
- Python: Check virtual environment activation
- .NET: Verify SDK version (6.0+)
- API Keys: Ensure proper formatting
- Endpoints: Include https:// prefix

**Common Errors:**
1. **"Module not found"**
   - Reinstall packages
   - Check virtual environment

2. **"Unauthorized"**
   - Verify API key
   - Check endpoint URL
   - Confirm region

3. **"Timeout"**
   - Increase timeout settings
   - Check network connectivity
   - Verify service status

## Best Practices by