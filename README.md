# VoyageCrew
VoyageCrew The Multi-Agent AI Travel Agency That Designs Your Trip From Scratch — orchestrating flights, hotels, activities, local experiences, safety advice, and transit plans into one seamless itinerary. This CrewAi app uses specialized AI agents that collaborate to turn a travel request into a complete trip, presented in a Gradio interface.

# Travel Agency CrewAI Application

A multi-agent AI travel planning system built with CrewAI and Gradio.

## Setup

1. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

2. **Set up environment variables:**
   ```bash
   cp env.example .env
   ```
   Then edit `.env` and add your API keys:
   - `OPENAI_API_KEY` - Your OpenAI API key
   - `AMADEUS_CLIENT_ID` - Amadeus API client ID
   - `AMADEUS_CLIENT_SECRET` - Amadeus API client secret
   - `GOOGLE_PLACES_API_KEY` - Google Places API key
   - `EXCHANGE_RATE_API_KEY` - Exchange Rate API key
   - `EMAIL_USER` - Your Gmail address for sending PDFs
   - `EMAIL_PASSWORD` - Your Gmail app password (not regular password)

## Running the Application

**IMPORTANT:** You must run the application from the `travel_agency` directory (this directory).

### Method 1: Run as a module (Recommended)
```bash
cd 3_crew/travel_agency
python -m src.travel_agency.app
```

### Method 2: Run directly (Alternative)
```bash
cd 3_crew/travel_agency
python src/travel_agency/app.py
```

### Method 3: Using the main entry point
```bash
cd 3_crew/travel_agency
python -m src.travel_agency.main
```

## Email Setup (for PDF delivery)

To enable PDF delivery via email:

1. **For Gmail users:**
   - Enable 2-factor authentication on your Google account
   - Generate an App Password: Google Account → Security → App Passwords
   - Use the App Password in `EMAIL_PASSWORD` (not your regular password)

2. **For other email providers:**
   - Update `SMTP_SERVER` and `SMTP_PORT` in your `.env` file
   - Use your email credentials in `EMAIL_USER` and `EMAIL_PASSWORD`

## Troubleshooting

- **"No module named 'src'"**: Make sure you're running from the `travel_agency` directory, not from a parent directory
- **Import errors**: The application uses relative imports, so it must be run as a module or from the correct directory
- **Missing .env file**: Copy `env.example` to `.env` and add your API keys
- **API errors**: Check that all required API keys are set in your `.env` file
- **Email errors**: For Gmail, use an App Password instead of your regular password

## Features

- Interactive Gradio web interface
- Multi-agent AI travel planning
- Flight search via Amadeus API
- Hotel recommendations
- Currency conversion
- Local experiences via Google Places
- Safety and travel rules information
- Loyalty program calculations
- Refund calculations
- Messaging capabilities
- PDF generation with detailed travel plans
- Email delivery of travel plans
- Authentic souvenir recommendations based on destination and interests

## Architecture Overview

This application uses a multi-layer architecture:

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                        Travel Agency Architecture                            │
│                                                                              │
│  ┌──────────────────┐                                                        │
│  │                  │                                                        │
│  │  User Interface  │  Gradio Web Interface (app.py)                         │
│  │      Layer       │  Progressive Loading & Card-Based UI                   │
│  │                  │                                                        │
│  └────────┬─────────┘                                                        │
│           │                                                                  │
│  ┌────────▼─────────┐     ┌─────────────────────────┐                        │
│  │                  │     │                         │                        │
│  │     Agent        │     │     Configuration       │                        │
│  │  Orchestration   ◄─────┤        Layer            │                        │
│  │     Layer        │     │   (YAML Configuration)  │                        │
│  │   (CrewAI)       │     │                         │                        │
│  │                  │     └─────────────────────────┘                        │
│  └────────┬─────────┘                                                        │
│           │                                                                  │
│  ┌────────▼─────────┐                                                        │
│  │                  │                                                        │
│  │      Data        │  Response Formatting, Token Optimisation               │
│  │   Processing     │  Content Extraction, Information Categorisation        │
│  │     Layer        │                                                        │
│  │                  │                                                        │
│  └────────┬─────────┘                                                        │
│           │                                                                  │
│  ┌────────▼─────────┐                                                        │
│  │                  │                                                        │
│  │  External Tools  │  API Integrations, PDF Generation,                     │
│  │  & Services      │  Email Delivery, Search Utilities                      │
│  │                  │                                                        │
│  └──────────────────┘                                                        │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

### 1. User Interface Layer (Gradio)
- **Progressive Loading Pattern**: Content loads in phases for better UX
- **Card-Based UI**: Modular information display with collapsible sections
- **Tabbed Interface**: Separates conversation from travel plan display

### 2. Agent Orchestration Layer (CrewAI)
- **Declarative Configuration**: Agents and tasks defined in YAML files
- **Specialized Agent Roles**: Each agent has a specific travel expertise
- **Tool Distribution**: Agents have access to different tool combinations
- **Collaboration Workflow**: Sequential and parallel task execution

### 3. Data Processing Layer
- **Token Optimization**: Condensing verbose AI outputs for efficiency
- **Response Formatting**: Structuring travel data into consistent formats
- **Content Extraction**: Intelligent parsing of AI-generated content
- **Information Categorization**: Organizing content by type and priority

### 4. Tools & External Services
- **API Integration**: Multiple travel-related API connections
- **PDF Generation**: Creating downloadable travel documents
- **Email Delivery**: Distributing travel plans via email
- **Custom Search Utilities**: Specialized search algorithms for travel data

## Project Structure

```
travel_agency/
├── src/travel_agency/
│   ├── app.py          # Gradio web interface
│   ├── crew.py         # CrewAI setup
│   ├── main.py         # CLI entry point
│   ├── utils/          # Utility functions and formatting tools
│   │   ├── response_formatter.py # Token optimization utilities
│   │   └── travel_card.py       # UI components for travel cards
│   ├── tools/          # Custom tools
│   │   ├── pdf_email_service.py  # PDF generation and email service
│   │   └── [other tool modules]
│   ├── components/     # UI components
│   └── config/         # YAML configurations
│       ├── agents.yaml # Agent definitions with roles and tools
│       └── tasks.yaml  # Task definitions and agent assignments
├── .env                # Environment variables
├── env.example         # Environment template
└── README.md          # This file
```

## Configuration Files Guide

The application uses two main YAML configuration files for its multi-agent architecture. These files are extensively commented to explain their structure and purpose.

### agents.yaml

This file defines the specialized AI agents that make up our travel agency crew. Each agent has:

- **Name**: A unique identifier for the agent
- **Role**: A specific area of travel expertise (e.g., flight booking, local experiences)
- **Goal**: The agent's primary objective within the system
- **Backstory**: Background information that shapes the agent's personality and approach
- **Tools**: A list of specialized tools the agent can use to accomplish tasks
- **Allow Delegation**: Whether the agent can delegate subtasks to other agents
- **Verbose**: Whether to show detailed output during operation

When adding new agents:
1. Follow the existing comment style for consistency
2. Ensure each agent has a clear, specialized role without overlap
3. Carefully choose appropriate tools based on the agent's specialty
4. Consider how the new agent will interact with existing agents
5. Test the agent's performance on relevant tasks

### tasks.yaml

This file defines the tasks that can be assigned to our specialized agents. Each task has:

- **Description**: A detailed prompt template with instructions for the agent
- **Agent**: The specialist agent assigned to handle this task
- **Expected Output**: The format and content the agent should return

When adding new tasks:
1. Follow the existing comment style for consistency
2. Use the {user_context} template variable to incorporate user preferences
3. Provide clear, detailed instructions in the description
4. Specify the appropriate specialist agent to handle the task
5. Clearly define the expected output format
6. Test the task with various user inputs

## Future Enhancement Guidelines

### 1. Code Structure & Documentation
- Maintain comprehensive comments in all code and configuration files
- Document all agent roles, tools, and capabilities clearly
- Use consistent commenting style across the codebase
- Keep configuration files (agents.yaml, tasks.yaml) well-documented
- Update documentation when adding new features or agents
- Maintain architecture diagrams that reflect the current system design
- Create tutorials for extending the agent system with new capabilities
- Document the interaction patterns between different agents

### 2. Real-Time Integration & Live Data
- Integrate with flight, hotel, and activity booking APIs
- Add live weather data integration
- Implement price tracking and alerts
- Add travel advisory integration

### 2. Health & Safety Focus
- Add vaccination and health requirement information
- Implement medical facility mapping near accommodations
- Enhance accessibility information for venues with AccessibilityTool expansion
- Add neighborhood safety ratings
- Improve safety warnings with more specific data sources
- Add emergency contact information for each destination
- Build dedicated specialized agents for medical and safety information

### 3. Budget Optimization & Financial Management
- Implement expense tracking against budget
- Enhance the CurrencyTool with real-time exchange rates
- Develop price prediction models for optimal booking times
- Create automated deal and discount finder
- Add budget breakdown visualizations
- Implement budget range recommendations based on destination
- Create cost comparison between different travel options
- Add flexible budget allocation suggestions
- Develop budget alerts for exceeding predefined limits

### 4. Mobile-First & Offline Capabilities
- Convert to Progressive Web App (PWA)
- Add offline access to travel plans
- Implement downloadable maps
- Develop location-based alerts and recommendations
- Further enhance progressive loading for mobile optimization
- Implement responsive card layouts for different screen sizes
- Create native app wrappers for iOS and Android
- Add save-to-device functionality for travel documents
- Implement push notifications for itinerary reminders

### 5. Personalization & AI Learning
- Create user profiles to store preferences and travel history
- Implement travel style recognition based on past choices
- Develop adaptive recommendations based on user feedback
- Add cross-trip learning to improve suggestions over time
- Implement better interest parsing and categorization
- Create more specialized agent roles for niche interests
- Add memory of previous trips to avoid duplicate recommendations
- Implement personalized card prioritization in the UI

### 6. Dynamic Assistance During Travel
- Enable real-time itinerary adjustments
- Add local transportation guidance
- Implement translation assistance
- Create emergency support and quick access to key information

### 7. Sustainability & Responsible Tourism
- Add carbon footprint calculator
- Implement eco-friendly option highlighting
- Add support for local business recommendations
- Include cultural preservation information
- Create a dedicated Sustainability Specialist agent role
- Add ethical tourism guidelines for each destination
- Implement sustainable transportation options
- Highlight community-based tourism initiatives
- Add seasonal tourism impact information to prevent overtourism

### 8. Social & Collaborative Planning
- Enable group planning capabilities
- Add voting on options for group members
- Implement shared itineraries
- Add social media integration

### 9. Augmented Reality & Visualization
- Add virtual tours of destinations
- Implement AR experience previews
- Create interactive, explorable maps
- Develop visual timeline itineraries

### 10. Post-Trip Engagement
- Add trip journal functionality
- Implement photo organization tools
- Add venue review collection
- Create souvenir tracking
- Develop trip memory preservation tools
- Implement follow-up surveys for continuous improvement
- Add expense report generation for business travelers
- Create shareable travel stories with dynamic content
- Add before/after comparison of expectations vs. experiences
