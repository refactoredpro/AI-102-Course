# Day 9: Tool Integration and Enhancement - Lab Guide

## Today's Mission
Extend your AI agent's capabilities by integrating custom tools and external services, transforming it from a simple Q&A bot into a powerful, multi-functional assistant.

## Learning Objectives
By the end of this lab, you will be able to:
- Understand tool integration patterns
- Work with APIs and external services
- Create custom plugins and extensions
- Manage authentication and security
- Build end-to-end integrated workflows

## Prerequisites
- Working AI agent from Day 8
- API documentation for services to integrate
- Development environment ready
- Basic understanding of REST APIs

## Time Required
- Total: 2 hours
- Tool concepts: 30 minutes
- API integration: 45 minutes
- Custom plugin development: 30 minutes
- Testing and debugging: 15 minutes

---

## Part 1: Understanding Tool Integration Patterns (30 minutes)

### What Are Agent Tools?

Tools extend your agent's capabilities beyond conversation:

```
┌─────────────────────────────────────────────────────────┐
│                    Agent Core                            │
│                                                          │
│  User Input → Processing → Tool Selection → Response    │
│                    │                                     │
│                    ▼                                     │
│         ┌─────────────────────┐                        │
│         │    Tool Registry     │                        │
│         ├─────────────────────┤                        │
│         │ • Weather API        │                        │
│         │ • Database Query     │                        │
│         │ • Email Sender       │                        │
│         │ • Calculator         │                        │
│         │ • Custom Functions   │                        │
│         └─────────────────────┘                        │
└─────────────────────────────────────────────────────────┘
```

### Tool Categories

**1. Information Retrieval Tools**
```python
# Example: Weather tool
class WeatherTool:
    name = "get_weather"
    description = "Get current weather for any city"
    
    parameters = {
        "city": {
            "type": "string",
            "description": "City name",
            "required": True
        }
    }
    
    async def execute(self, city: str) -> dict:
        # API call to weather service
        return weather_data
```

**2. Action Execution Tools**
```python
# Example: Email tool
class EmailTool:
    name = "send_email"
    description = "Send an email to specified recipient"
    
    parameters = {
        "to": {"type": "string", "required": True},
        "subject": {"type": "string", "required": True},
        "body": {"type": "string", "required": True}
    }
    
    async def execute(self, to: str, subject: str, body: str) -> bool:
        # Send email via SMTP or API
        return success_status
```

**3. Computation Tools**
```python
# Example: Calculator tool
class CalculatorTool:
    name = "calculate"
    description = "Perform mathematical calculations"
    
    parameters = {
        "expression": {
            "type": "string",
            "description": "Math expression to evaluate"
        }
    }
    
    async def execute(self, expression: str) -> float:
        # Safe math evaluation
        return result
```

### Tool Selection Logic

How agents decide which tool to use:

```yaml
user_input: "What's the weather in Seattle?"

agent_reasoning:
  1. Intent Detection: weather_query
  2. Entity Extraction: city = "Seattle"  
  3. Tool Matching: get_weather tool matches intent
  4. Parameter Mapping: city parameter = "Seattle"
  5. Tool Execution: call weather API
  6. Response Generation: format weather data
```

### Activity 1: Tool Planning

Design tools for your agent:

| Tool Name | Purpose | Input Parameters | Output Format | API/Service |
|-----------|---------|------------------|---------------|-------------|
| weather_check | Get weather | city (string) | temp, conditions | OpenWeather |
| stock_price | Get stock quote | symbol (string) | price, change | Alpha Vantage |
| translate_text | Translate text | text, target_lang | translated_text | Azure Translator |
| | | | | |

---

## Part 2: Implementing API Integrations (45 minutes)

### Weather API Integration

**Step 1: API Setup**
```python
# config.py
WEATHER_API_KEY = "your-openweather-api-key"
WEATHER_BASE_URL = "https://api.openweathermap.org/data/2.5/weather"
```

**Step 2: Tool Implementation**
```python
# tools/weather_tool.py
import aiohttp
from typing import Dict, Any

class WeatherTool:
    def __init__(self, api_key: str):
        self.api_key = api_key
        self.base_url = WEATHER_BASE_URL
    
    async def get_weather(self, city: str) -> Dict[str, Any]:
        """
        Fetches current weather for a city
        """
        params = {
            'q': city,
            'appid': self.api_key,
            'units': 'metric'
        }
        
        async with aiohttp.ClientSession() as session:
            async with session.get(self.base_url, params=params) as response:
                if response.status == 200:
                    data = await response.json()
                    return {
                        'success': True,
                        'temperature': data['main']['temp'],
                        'description': data['weather'][0]['description'],
                        'humidity': data['main']['humidity'],
                        'wind_speed': data['wind']['speed']
                    }
                else:
                    return {
                        'success': False,
                        'error': f'Weather data not available for {city}'
                    }
    
    def format_response(self, weather_data: Dict[str, Any], city: str) -> str:
        """
        Formats weather data into human-readable response
        """
        if weather_data['success']:
            return f"""
            Current weather in {city}:
            🌡️ Temperature: {weather_data['temperature']}°C
            ☁️ Conditions: {weather_data['description']}
            💧 Humidity: {weather_data['humidity']}%
            💨 Wind Speed: {weather_data['wind_speed']} m/s
            """
        else:
            return weather_data['error']
```

**Step 3: Agent Integration**
```python
# agent_with_tools.py
from semantic_kernel import Kernel
from tools.weather_tool import WeatherTool

class EnhancedAgent:
    def __init__(self):
        self.kernel = Kernel()
        self.weather_tool = WeatherTool(WEATHER_API_KEY)
        
        # Register tool with kernel
        self.kernel.register_function(
            plugin_name="tools",
            function_name="get_weather",
            function=self.weather_tool.get_weather,
            description="Get current weather for a city"
        )
    
    async def process_message(self, user_input: str) -> str:
        # Check if weather information is requested
        if any(word in user_input.lower() for word in ['weather', 'temperature', 'forecast']):
            # Extract city name (simplified - use NER in production)
            city = self.extract_city(user_input)
            
            if city:
                weather_data = await self.weather_tool.get_weather(city)
                return self.weather_tool.format_response(weather_data, city)
        
        # Handle other queries
        return await self.default_response(user_input)
```

### Database Query Tool

**Implementation Example:**
```python
# tools/database_tool.py
import asyncpg
from typing import List, Dict, Any

class DatabaseTool:
    def __init__(self, connection_string: str):
        self.connection_string = connection_string
        self.allowed_tables = ['products', 'orders', 'customers']
    
    async def query_database(self, query_type: str, parameters: Dict[str, Any]) -> List[Dict]:
        """
        Executes safe database queries
        """
        # Validate query type
        if query_type not in ['search_products', 'check_order', 'customer_info']:
            raise ValueError("Invalid query type")
        
        # Build safe query based on type
        query = self._build_safe_query(query_type, parameters)
        
        # Execute query
        conn = await asyncpg.connect(self.connection_string)
        try:
            results = await conn.fetch(query)
            return [dict(record) for record in results]
        finally:
            await conn.close()
    
    def _build_safe_query(self, query_type: str, parameters: Dict[str, Any]) -> str:
        """
        Builds parameterized queries to prevent SQL injection
        """
        queries = {
            'search_products': """
                SELECT name, price, description 
                FROM products 
                WHERE name ILIKE $1 
                LIMIT 10
            """,
            'check_order': """
                SELECT order_id, status, total, created_at
                FROM orders
                WHERE order_id = $1
            """,
            'customer_info': """
                SELECT name, email, join_date
                FROM customers
                WHERE customer_id = $1
            """
        }
        
        return queries.get(query_type)
```

### Activity 2: API Integration Exercise

Implement a stock price tool:

```python
# Your implementation here
class StockPriceTool:
    def __init__(self, api_key: str):
        # Initialize with API key
        pass
    
    async def get_stock_price(self, symbol: str) -> Dict[str, Any]:
        # Implement API call
        # Return price data
        pass
    
    def format_response(self, stock_data: Dict[str, Any]) -> str:
        # Format for user
        pass
```

Test cases:
- "What's the current price of AAPL?"
- "Show me Microsoft stock"
- "How's TSLA doing today?"

---

## Part 3: Creating Custom Plugins (30 minutes)

### Plugin Architecture

```python
# plugins/base_plugin.py
from abc import ABC, abstractmethod
from typing import Dict, Any, List

class BasePlugin(ABC):
    """Base class for all agent plugins"""
    
    @property
    @abstractmethod
    def name(self) -> str:
        """Plugin name"""
        pass
    
    @property
    @abstractmethod
    def description(self) -> str:
        """Plugin description"""
        pass
    
    @property
    @abstractmethod
    def functions(self) -> List[Dict[str, Any]]:
        """List of functions provided by plugin"""
        pass
    
    @abstractmethod
    async def execute(self, function_name: str, **kwargs) -> Any:
        """Execute plugin function"""
        pass
```

### Business Logic Plugin Example

```python
# plugins/business_plugin.py
from datetime import datetime, timedelta
from typing import Dict, Any, List

class BusinessPlugin(BasePlugin):
    name = "business_operations"
    description = "Handles business-specific operations"
    
    @property
    def functions(self) -> List[Dict[str, Any]]:
        return [
            {
                "name": "calculate_discount",
                "description": "Calculate discount based on rules",
                "parameters": {
                    "original_price": {"type": "float", "required": True},
                    "customer_tier": {"type": "string", "required": True}
                }
            },
            {
                "name": "check_inventory",
                "description": "Check product availability",
                "parameters": {
                    "product_id": {"type": "string", "required": True},
                    "quantity": {"type": "integer", "required": True}
                }
            },
            {
                "name": "estimate_delivery",
                "description": "Estimate delivery date",
                "parameters": {
                    "location": {"type": "string", "required": True},
                    "shipping_method": {"type": "string", "required": True}
                }
            }
        ]
    
    async def execute(self, function_name: str, **kwargs) -> Any:
        if function_name == "calculate_discount":
            return self._calculate_discount(**kwargs)
        elif function_name == "check_inventory":
            return await self._check_inventory(**kwargs)
        elif function_name == "estimate_delivery":
            return self._estimate_delivery(**kwargs)
        else:
            raise ValueError(f"Unknown function: {function_name}")
    
    def _calculate_discount(self, original_price: float, customer_tier: str) -> Dict[str, Any]:
        """Calculate discount based on customer tier"""
        discounts = {
            "bronze": 0.05,
            "silver": 0.10,
            "gold": 0.15,
            "platinum": 0.20
        }
        
        discount_rate = discounts.get(customer_tier.lower(), 0)
        discount_amount = original_price * discount_rate
        final_price = original_price - discount_amount
        
        return {
            "original_price": original_price,
            "discount_rate": discount_rate,
            "discount_amount": discount_amount,
            "final_price": final_price,
            "savings": discount_amount
        }
    
    async def _check_inventory(self, product_id: str, quantity: int) -> Dict[str, Any]:
        """Check inventory availability"""
        # In production, this would query actual inventory system
        mock_inventory = {
            "PROD001": 150,
            "PROD002": 23,
            "PROD003": 0,
            "PROD004": 87
        }
        
        available = mock_inventory.get(product_id, 0)
        is_available = available >= quantity
        
        return {
            "product_id": product_id,
            "requested": quantity,
            "available": available,
            "is_available": is_available,
            "message": "In stock" if is_available else f"Only {available} units available"
        }
    
    def _estimate_delivery(self, location: str, shipping_method: str) -> Dict[str, Any]:
        """Estimate delivery date based on location and method"""
        base_days = {
            "standard": 5,
            "express": 2,
            "overnight": 1
        }
        
        location_modifier = {
            "local": 0,
            "national": 1,
            "international": 3
        }
        
        days = base_days.get(shipping_method, 5)
        days += location_modifier.get(location, 1)
        
        delivery_date = datetime.now() + timedelta(days=days)
        
        return {
            "shipping_method": shipping_method,
            "location": location,
            "estimated_days": days,
            "delivery_date": delivery_date.strftime("%Y-%m-%d"),
            "tracking_available": shipping_method != "standard"
        }
```

### Plugin Registration and Management

```python
# plugin_manager.py
from typing import Dict, Any, List
from plugins.base_plugin import BasePlugin

class PluginManager:
    def __init__(self):
        self.plugins: Dict[str, BasePlugin] = {}
    
    def register_plugin(self, plugin: BasePlugin):
        """Register a new plugin"""
        self.plugins[plugin.name] = plugin
        print(f"Registered plugin: {plugin.name}")
    
    def get_all_functions(self) -> List[Dict[str, Any]]:
        """Get all available functions from all plugins"""
        all_functions = []
        for plugin in self.plugins.values():
            for func in plugin.functions:
                func['plugin'] = plugin.name
                all_functions.append(func)
        return all_functions
    
    async def execute_function(self, plugin_name: str, function_name: str, **kwargs) -> Any:
        """Execute a function from a specific plugin"""
        if plugin_name not in self.plugins:
            raise ValueError(f"Plugin not found: {plugin_name}")
        
        plugin = self.plugins[plugin_name]
        return await plugin.execute(function_name, **kwargs)
    
    def describe_functions(self) -> str:
        """Generate human-readable description of all functions"""
        descriptions = []
        for plugin in self.plugins.values():
            descriptions.append(f"\n{plugin.name}: {plugin.description}")
            for func in plugin.functions:
                params = ", ".join(func['parameters'].keys())
                descriptions.append(f"  - {func['name']}({params}): {func['description']}")
        
        return "\n".join(descriptions)
```

### Activity 3: Create Your Custom Plugin

Design and implement a plugin for your domain:

```python
# Template for your custom plugin
class YourCustomPlugin(BasePlugin):
    name = "your_plugin_name"
    description = "What your plugin does"
    
    @property
    def functions(self) -> List[Dict[str, Any]]:
        return [
            # Define your functions here
        ]
    
    async def execute(self, function_name: str, **kwargs) -> Any:
        # Implement function execution
        pass
```

Ideas for custom plugins:
- Appointment scheduling
- Report generation
- Data analysis
- Notification system
- Workflow automation

---

## Part 4: Security and Authentication (15 minutes)

### Securing Tool Access

**API Key Management:**
```python
# secure_config.py
import os
from cryptography.fernet import Fernet

class SecureConfig:
    def __init__(self):
        self.cipher = Fernet(os.environ['ENCRYPTION_KEY'])
    
    def encrypt_api_key(self, api_key: str) -> bytes:
        """Encrypt API key for storage"""
        return self.cipher.encrypt(api_key.encode())
    
    def decrypt_api_key(self, encrypted_key: bytes) -> str:
        """Decrypt API key for use"""
        return self.cipher.decrypt(encrypted_key).decode()
    
    def get_api_key(self, service: str) -> str:
        """Securely retrieve API key for service"""
        # In production, retrieve from secure vault
        encrypted = os.environ.get(f'{service.upper()}_API_KEY_ENCRYPTED')
        if encrypted:
            return self.decrypt_api_key(encrypted.encode())
        return None
```

**Rate Limiting:**
```python
# rate_limiter.py
from datetime import datetime, timedelta
from collections import defaultdict
import asyncio

class RateLimiter:
    def __init__(self, calls_per_minute: int = 60):
        self.calls_per_minute = calls_per_minute
        self.calls = defaultdict(list)
        self.lock = asyncio.Lock()
    
    async def check_rate_limit(self, user_id: str) -> bool:
        """Check if user is within rate limits"""
        async with self.lock:
            now = datetime.now()
            minute_ago = now - timedelta(minutes=1)
            
            # Clean old calls
            self.calls[user_id] = [
                call_time for call_time in self.calls[user_id]
                if call_time > minute_ago
            ]
            
            # Check limit
            if len(self.calls[user_id]) >= self.calls_per_minute:
                return False
            
            # Record call
            self.calls[user_id].append(now)
            return True
```

**Input Validation:**
```python
# validators.py
import re
from typing import Any, Dict

class ToolInputValidator:
    @staticmethod
    def validate_email(email: str) -> bool:
        pattern = r'^[\w\.-]+@[\w\.-]+\.\w+$'
        return re.match(pattern, email) is not None
    
    @staticmethod
    def validate_phone(phone: str) -> bool:
        pattern = r'^\+?1?\d{9,15}$'
        return re.match(pattern, phone) is not None
    
    @staticmethod
    def sanitize_input(text: str) -> str:
        """Remove potentially harmful characters"""
        # Remove SQL injection attempts
        dangerous_patterns = ['--', ';', '/*', '*/', 'xp_', 'sp_']
        for pattern in dangerous_patterns:
            text = text.replace(pattern, '')
        
        # Limit length
        return text[:1000]
    
    @staticmethod
    def validate_parameters(params: Dict[str, Any], schema: Dict[str, Dict]) -> Dict[str, Any]:
        """Validate parameters against schema"""
        validated = {}
        
        for param_name, param_schema in schema.items():
            value = params.get(param_name)
            
            # Check required
            if param_schema.get('required') and value is None:
                raise ValueError(f"Missing required parameter: {param_name}")
            
            # Type validation
            if value is not None:
                expected_type = param_schema.get('type')
                if expected_type == 'string' and not isinstance(value, str):
                    value = str(value)
                elif expected_type == 'integer' and not isinstance(value, int):
                    value = int(value)
                elif expected_type == 'float' and not isinstance(value, (int, float)):
                    value = float(value)
                
                validated[param_name] = value
        
        return validated
```

---

## Testing Your Enhanced Agent

### Integration Test Suite

```python
# test_enhanced_agent.py
import asyncio
import pytest

class TestEnhancedAgent:
    @pytest.mark.asyncio
    async def test_weather_integration(self):
        agent = EnhancedAgent()
        response = await agent.process_message("What's the weather in London?")
        assert "temperature" in response.lower()
        assert "London" in response
    
    @pytest.mark.asyncio
    async def test_multiple_tools(self):
        agent = EnhancedAgent()
        
        # Test weather
        weather_response = await agent.process_message("Weather in Paris?")
        assert weather_response
        
        # Test calculator
        calc_response = await agent.process_message("What's 25% of 840?")
        assert "210" in calc_response
        
        # Test database
        db_response = await agent.process_message("Check order #12345")
        assert "order" in db_response.lower()
    
    @pytest.mark.asyncio
    async def test_error_handling(self):
        agent = EnhancedAgent()
        
        # Invalid city
        response = await agent.process_message("Weather in Atlantis?")
        assert "not available" in response.lower()
        
        # Invalid calculation
        response = await agent.process_message("Calculate invalid expression")
        assert "error" in response.lower() or "cannot" in response.lower()
```

### Activity 4: End-to-End Testing

Create test scenarios that use multiple tools:

1. **Travel Planning Scenario**
   - "What's the weather in Tokyo?"
   - "Convert 1000 USD to JPY"
   - "Find flights to Tokyo next week"

2. **Shopping Assistant Scenario**
   - "Check if PROD001 is in stock"
   - "Calculate discount for gold member on $299"
   - "Estimate delivery to California with express shipping"

3. **Data Analysis Scenario**
   - "Show me sales for last month"
   - "Calculate growth percentage"
   - "Generate summary report"

---

## Lab Completion Checklist

- [ ] Understood tool integration patterns
- [ ] Implemented weather API integration
- [ ] Created database query tool
- [ ] Built custom business plugin
- [ ] Configured security measures
- [ ] Tested all integrations
- [ ] Documented tool capabilities

## Troubleshooting Guide

**API Integration Issues:**
- Verify API keys are correct
- Check rate limits
- Confirm endpoint URLs
- Test with curl/Postman first

**Plugin Problems:**
- Ensure proper inheritance
- Verify function signatures
- Check async/await usage
- Review error messages

**Security Concerns:**
- Never hardcode API keys
- Implement input validation
- Use environment variables
- Enable HTTPS only

## Best Practices

**Do's:**
- ✅ Version your plugins
- ✅ Document all functions
- ✅ Handle errors gracefully
- ✅ Cache API responses
- ✅ Monitor usage metrics

**Don'ts:**
- ❌ Trust user input blindly
- ❌ Expose internal errors
- ❌ Skip rate limiting
- ❌ Ignore API costs
- ❌ Forget logging

## Key Takeaways

✅ Tools transform agents from chatbots to assistants
✅ Proper architecture enables easy tool addition
✅ Security must be built-in, not bolted-on
✅ Testing integrated systems requires comprehensive scenarios
✅ Good error handling improves user experience

---

## Resources

- [API Integration Best Practices](https://docs.microsoft.com/api-design)
- [Plugin Architecture Patterns](https://docs.microsoft.com/plugins)
- [Security for AI Applications](https://docs.microsoft.com/ai-security)

## Next Steps

Tomorrow we'll explore advanced orchestration with Semantic Kernel, including:
- Multi-agent scenarios
- Complex workflow management
- Agent-to-agent communication
- Advanced planning strategies

**Challenge**: Add one more tool to your agent tonight and test it thoroughly!