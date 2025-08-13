# Travel Agency AI - Multi-Agent Travel Planning System

A sophisticated multi-agent AI travel planning system built with CrewAI, Gradio, and integrated with real travel APIs. This application creates personalized travel itineraries using specialized AI agents working together to provide comprehensive travel planning services.

## Features

### Core Capabilities
- **Interactive Gradio Web Interface** - Modern, card-based UI with progressive loading
- **Multi-Agent AI Collaboration** - Specialized agents for different travel aspects
- **Real API Integrations** - Live data from Amadeus, Google Places, Unsplash, and more
- **Professional PDF Generation** - Detailed travel plans with destination imagery
- **Email Delivery** - Automatic PDF delivery to your inbox
- **Currency Conversion** - Real-time exchange rates
- **Event Discovery** - Local events via Ticketmaster and Eventbrite APIs
- **Weather Integration** - Current conditions and forecasts
- **Safety Information** - Travel advisories and safety guidelines
- **Accessibility Support** - Venue accessibility information

### Advanced Features
- **Loyalty Program Calculations** - Estimate points and benefits
- **Refund Calculations** - Calculate potential refunds for bookings
- **Souvenir Recommendations** - Authentic local product suggestions
- **Photography Integration** - Professional destination photos with proper attribution
- **Messaging System** - Push notifications via Pushover
- **Mock Mode Support** - Fallback data when APIs are unavailable

## Quick Start

### 1. Environment Setup

```bash
# Navigate to the travel agency directory
cd 3_crew/travel_agency

# Copy environment template
cp env.example .env

# Install dependencies (if not already installed)
pip install -r requirements.txt
```

### 2. Configure API Keys

Edit your `.env` file with the following required keys:

```properties
# Essential Keys (Required)
OPENAI_API_KEY=your_openai_api_key_here
GOOGLE_PLACES_API_KEY=your_google_places_key_here

# Travel APIs (Recommended)
AMADEUS_CLIENT_ID=your_amadeus_client_id
AMADEUS_CLIENT_SECRET=your_amadeus_client_secret
UNSPLASH_ACCESS_KEY=your_unsplash_access_key
TICKETMASTER_API_KEY=your_ticketmaster_key
EVENTBRITE_API_KEY=your_eventbrite_key

# Utility APIs (Optional)
EXCHANGE_RATE_API_KEY=your_exchange_rate_key
OPENWEATHER_API_KEY=your_openweather_key
SERPER_API_KEY=your_serper_key

# Email Configuration (Optional - for PDF delivery)
EMAIL_USER=your_email@gmail.com
EMAIL_PASSWORD=your_app_password_here
SMTP_SERVER=smtp.gmail.com
SMTP_PORT=587

# Notifications (Optional)
PUSHOVER_USER_KEY=your_pushover_user_key
PUSHOVER_API_TOKEN=your_pushover_api_token

# Development (Optional)
MOCK_TOOLS=true  # Set to false for production with real APIs
```

### 3. Run the Application

**IMPORTANT**: Always run from the `travel_agency` directory.

```bash
# Method 1: Recommended
python -m src.travel_agency

# Method 2: Alternative
python src/travel_agency/app.py
```

The application will start on `http://localhost:7860`

## API Keys & Services

### Free Tier Available
- **OpenAI** - Required for AI agents ($20 credit for new users)
- **Google Places** - Required for location data (Free tier: 20K requests/month)
- **Unsplash** - Professional photos (Free: 50 requests/hour, Paid: 5K/hour)
- **Ticketmaster** - Events (Free: 5K requests/day)
- **Eventbrite** - Local events (Free tier available)

### Optional Services
- **Amadeus** - Flights/hotels (Free tier: 100 requests/month)
- **OpenWeather** - Weather data (Free: 1K requests/day)
- **ExchangeRate-API** - Currency conversion (Free: 1.5K requests/month)
- **Serper** - Search results (Free: 2.5K searches/month)

### How to Obtain API Keys

#### OpenAI (Required)
1. Visit https://platform.openai.com/
2. Create account and add payment method
3. Go to API Keys section
4. Create new secret key

#### Google Places (Required)
1. Visit https://console.cloud.google.com/
2. Create new project or select existing
3. Enable Places API
4. Create credentials → API Key
5. Restrict key to Places API for security

#### Unsplash (Recommended)
1. Visit https://unsplash.com/developers
2. Create account and new application
3. Name: "Travel Agency PDF Generator"
4. Copy your Access Key

#### Other APIs
- **Ticketmaster**: https://developer.ticketmaster.com/
- **Eventbrite**: https://www.eventbrite.com/platform/api/
- **Amadeus**: https://developers.amadeus.com/
- **OpenWeather**: https://openweathermap.org/api

## Email Setup (for PDF delivery)

### Gmail Setup (Recommended)
1. Enable 2-factor authentication on your Google account
2. Go to: Google Account → Security → App passwords
3. Generate an App Password for "Mail"
4. Use the App Password in `EMAIL_PASSWORD` (not your regular password)

### Other Email Providers
Update these values in your `.env`:
```properties
SMTP_SERVER=your_smtp_server
SMTP_PORT=587  # or 465 for SSL
EMAIL_USER=your_email@provider.com
EMAIL_PASSWORD=your_email_password
```

## Architecture Overview

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
│  │      Data        │  Response Formatting, Token Optimization               │
│  │   Processing     │  Content Extraction, Information Categorization        │
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

## Project Structure

```
travel_agency/
├── src/travel_agency/
│   ├── app.py                    # Gradio web interface
│   ├── crew.py                   # CrewAI setup and agent orchestration
│   ├── main.py                   # CLI entry point
│   ├── utils/                    # Utility functions
│   │   ├── response_formatter.py # Token optimization utilities
│   │   └── travel_card.py        # UI components for travel cards
│   ├── tools/                    # Custom tools and API integrations
│   │   ├── pdf_email_service.py  # PDF generation with Unsplash images
│   │   ├── flight_search_tool.py # Amadeus flight search
│   │   ├── hotel_search_tool.py  # Hotel recommendations
│   │   ├── google_places_tool.py # Google Places integration
│   │   ├── local_events_tool.py  # Ticketmaster/Eventbrite events
│   │   ├── currency_tool.py      # Exchange rate conversion
│   │   ├── weather_tool.py       # Weather information
│   │   ├── rules_safety_tool.py  # Travel safety guidelines
│   │   ├── accessibility_tool.py # Accessibility information
│   │   ├── messaging_tool.py     # Push notifications
│   │   └── souvenir_recommendations_tool.py # Local products
│   ├── config/                   # YAML configurations
│   │   ├── agents.yaml          # Agent definitions and roles
│   │   └── tasks.yaml           # Task definitions and workflows
│   └── components/              # UI components
├── .env                         # Environment variables (create from template)
├── env.example                  # Environment template
├── requirements.txt             # Python dependencies
└── README.md                   # This file
```

## Specialized AI Agents

The system uses multiple specialized AI agents working together:

### Core Agents
- **Travel Coordinator** - Orchestrates the planning process
- **Flight Search Specialist** - Finds and compares flights via Amadeus API
- **Local Experience Curator** - Discovers activities via Google Places
- **Budget Analyst** - Handles costs and currency conversion
- **Safety and Rules Expert** - Provides travel advisories and guidelines
- **Accessibility Specialist** - Ensures inclusive travel options

### Optional Agents (when tools available)
- **Loyalty Rewards Advisor** - Calculates program benefits
- **Refund Rights Expert** - Handles cancellation policies
- **Event Discovery Agent** - Finds local events and entertainment

## Troubleshooting

### Common Issues

**"No module named 'src'"**
- Solution: Run from the `travel_agency` directory, not parent directories
- Correct: `cd 3_crew/travel_agency && python -m src.travel_agency`

**Import errors or module not found**
- Solution: Use relative imports by running as a module
- Correct: `python -m src.travel_agency`
- Incorrect: `python app.py`

**Missing .env file**
- Solution: Copy `env.example` to `.env` and add your API keys
- Command: `cp env.example .env`

**API errors or "Mock mode" responses**
- Solution: Add real API keys to your `.env` file
- Check that keys don't start with "your_" (placeholder values)

**Gmail email errors**
- Solution: Use App Password, not regular password
- Enable 2FA first, then generate App Password

**PDF generation fails**
- Solution: Ensure `reportlab` is installed
- Command: `pip install reportlab`

**Gradio interface won't start**
- Solution: Check if port 7860 is available
- Try: `python -m src.travel_agency --port 7861`

### Performance Tips

1. **API Rate Limits**: The app respects API rate limits automatically
2. **Mock Mode**: Set `MOCK_TOOLS=true` for testing without API calls
3. **Image Loading**: Unsplash images enhance PDFs but require API key
4. **Email Delivery**: Optional feature, app works without email configuration

## Contributing

### Adding New Agents
1. Define agent in `config/agents.yaml`
2. Add corresponding tasks in `config/tasks.yaml`
3. Create specialized tools in `tools/` directory
4. Update `crew.py` tool mapping
5. Test with various user inputs

### Adding New Tools
1. Create tool class inheriting from `BaseTool`
2. Add to `tools/` directory with proper error handling
3. Include in agent tool assignments
4. Add API key to `.env.example` if needed
5. Update README with setup instructions

### Code Standards
- Remove all Unicode symbols (emojis) from output
- Use "SUCCESS:", "WARNING:", "ERROR:" prefixes for status messages
- Include comprehensive error handling
- Add timeout parameters to all API calls
- Support both live and mock modes

## API Compliance

### Unsplash API
- **Attribution**: All images include "Photo by [photographer] on Unsplash"
- **Download Tracking**: Automatic compliance with API guidelines
- **Rate Limits**: 50 requests/hour (demo), 5,000/hour (production)

### Google Places API
- **Terms**: Complies with Google Places API terms of service
- **Attribution**: Proper credit for place data

### Amadeus API
- **Usage**: Flight and hotel search within API guidelines
- **Rate Limits**: Automatic handling of rate limiting

## License

This project is licensed under the MIT License. See LICENSE file for details.

## Support

For issues and questions:
1. Check this README for common solutions
2. Verify your `.env` configuration
3. Test with `MOCK_TOOLS=true` first
4. Check terminal output for specific error messages

---

**Ready to plan your next adventure with AI-powered travel planning!**
