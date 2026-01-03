# FreelanceTM Bot

## Overview

FreelanceTM Bot is a Telegram-based freelance marketplace platform designed for the Turkmenistan market. It connects clients who need work done with freelancers who can complete tasks. The bot handles the entire workflow: user registration, order creation, freelancer responses, order management, payment processing with commission handling, reviews, and withdrawals.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Bot Framework
- **Framework**: aiogram 3.x (Python async Telegram bot framework)
- **State Management**: FSM (Finite State Machine) via aiogram's FSMContext for multi-step user interactions
- **Runtime**: Python asyncio-based event loop

### Data Storage
- **Storage Type**: JSON file-based database (no external database)
- **Data Directory**: `data/` folder for persistent storage
- **Core Data Files**:
  - `users.json` - User profiles, roles (client/freelancer), balances
  - `orders.json` - Job postings with status tracking
  - `responses.json` - Freelancer applications to orders
  - `reviews.json` - User ratings and feedback
  - `services.json` - Freelancer service listings
  - `withdrawals.json` - Payment withdrawal requests
  - `counters.json` - Auto-increment IDs for entities

### User Roles
- **Clients**: Post orders, select freelancers, confirm completion
- **Freelancers**: Create profiles, respond to orders, offer services
- **Admins**: Manage withdrawals, top-ups, platform administration

### Order Workflow
1. Client creates order with title, description, category, budget, deadline
2. Freelancers respond to orders
3. Client selects a freelancer
4. Both parties confirm completion
5. Payment released to freelancer (minus commission)
6. Review exchange

### Financial System
- **Commission Rate**: 10% on completed orders
- **Withdrawal Commission**: 10% on withdrawals
- **Minimum Withdrawal**: 10 TMT
- **Balance Types**: Available balance and frozen balance

### Multi-language Support
- Turkmen (tm)
- Russian (ru)

### Keep-Alive Mechanism
- Simple HTTP server on port 5000 to prevent idle shutdown
- Runs in daemon thread alongside bot

### Channel Subscription
- Required channel subscription check before bot usage
- Configurable via `REQUIRED_CHANNEL` environment variable

## External Dependencies

### Telegram Bot API
- Bot token configured via `BOT_TOKEN` environment variable
- Admin IDs configured via `ADMIN_IDS` environment variable (comma-separated)

### Required Python Packages
- `aiogram` - Telegram Bot framework
- Standard library: `asyncio`, `json`, `logging`, `datetime`, `pathlib`, `threading`, `http.server`

### Environment Variables
- `BOT_TOKEN` - Telegram bot API token
- `ADMIN_IDS` - Comma-separated list of admin Telegram user IDs
- `REQUIRED_CHANNEL` - Telegram channel username for mandatory subscription

### No External Database
- All data persisted to local JSON files
- Multiple backup files with timestamps exist (legacy data migrations)
- Consider migrating to proper database (PostgreSQL with Drizzle) for production scale