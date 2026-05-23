# 🗓️ AI Event Planner Agent

An AI-powered ReAct agent that plans your weekend by combining real-time weather forecasts, live event listings, and route planning, all from a single natural language prompt.

## How It Works

The agent uses the **ReAct (Reasoning + Action)** framework: it reasons about your request step by step, decides which tool to call, observes the result, and repeats until it has a final answer. No hand-holding required, just ask it what you want to do.

**Example prompt:**
> *"I'm in Edinburgh and want to attend an outdoor event in London this weekend, but only if it won't rain. If it will rain, find me an indoor event instead. How long is the drive and should I leave Friday night or Saturday morning to make it on time?"*

The agent will automatically check the weather, find a suitable event, calculate drive times for both departure options, and give you a recommendation.

## Tools

| Tool | API | What it does |
|---|---|---|
| `weather_lookup` | Open-Meteo | Fetches daily weather forecasts (up to 16 days) for any city |
| `events_search` | Ticketmaster | Finds upcoming events in a given city |
| `routes_search` | Google Routes | Calculates travel distance and duration between two locations |

Note: Our agent is allowed to repeatedly call these API's. This becomes very helpful for `routes_search` where the agent decides 
which commute method is best. 

## Setup

This project runs on **Google Colab**. Before running, add the following to your Colab secrets (the key icon in the left sidebar):

| Secret name | Where to get it |
|---|---|
| `TICKET_MASTER_API_KEY` | [Ticketmaster Developer Portal](https://developer.ticketmaster.com/) |
| `GOOGLE_ROUTES_API_KEY` | [Google Cloud Console](https://console.cloud.google.com/), please enable the Routes API |
| `GROQ_API_KEY` | [Groq Console](https://console.groq.com/) |

Then run all cells from top to bottom.

## Usage

Call `ask_agent()` with any natural language query involving weather, events, and/or travel:

```python
ask_agent("Find me something fun to do in Chicago this weekend if the weather is nice.")

ask_agent("I'm in Austin. I want to head to Houston for a concert (any concert). How long will my commute be?")

ask_agent("Is it worth going to an outdoor festival in Seattle this weekend, or will it rain?")
```

You can also adjust the maximum reasoning steps (default is 10):

```python
ask_agent("Plan my weekend in NYC.", steps=15)
```

## Dependencies

```
groq
geopy
requests
```

Install via:
```bash
pip install groq geopy
```

## Model

We use **Llama 3.3 70B** via the [Groq](https://groq.com/) API for fast inference.
Note that Llama 3.3 70B was used because it's cheap and easily accessible for developers while providing great Agentic abilities. Feel free to change to other models. 
