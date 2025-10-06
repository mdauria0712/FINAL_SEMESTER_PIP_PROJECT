# Sports Betting Companion

## Database Design

### Overview
Sports Betting Companion is a data-driven tool that helps users find undervalued soccer bets using historical data and live sportsbook odds. With the 2026 World Cup coming to the US, many new bettors may not know much about the teams or players. Our goal is to make it easier for them to make smarter, data-backed bets. By connecting historical performance data with sportsbook odds, users can identify which teams are statistically undervalued compared to their betting lines.

### Entity Relationship Diagram


### Tables Description

#### 1) `users`
Stores user information and account balance.
- `id` (UUID, PK)  
- `username` (TEXT)  
- `email` (TEXT, unique)  
- `password_hash` (TEXT)  
- `balance` (NUMERIC) – user’s account balance  
- `created_at` (TIMESTAMPTZ, default now)

#### 2) `teams`
Represents each national team.
- `id` (BIGINT, PK)  
- `name` (TEXT) – team name (e.g., Argentina)  
- `country_code` (TEXT) – 3-letter FIFA code  
- `group_name` (TEXT) – group label (A–H)

#### 3) `players`
Links individual players to their team.
- `id` (BIGINT, PK)  
- `name` (TEXT)  
- `team_id` (BIGINT, FK → teams.id)  
- `position` (TEXT)

#### 4) `matches`
Stores match information and outcomes.
- `id` (BIGINT, PK)  
- `team1_id` (BIGINT, FK → teams.id)  
- `team2_id` (BIGINT, FK → teams.id)  
- `match_date` (TIMESTAMPTZ)  
- `score_team1` (INT)  
- `score_team2` (INT)  
- `status` (TEXT: scheduled, in_progress, finished)  
- `stage` (TEXT) – tournament stage (Group, R16, etc.)  
- `venue` (TEXT)  
- `api_ref` (TEXT) – optional external reference

#### 5) `bets`
Tracks bets users place on matches.
- `id` (BIGINT, PK)  
- `user_id` (UUID, FK → users.id)  
- `match_id` (BIGINT, FK → matches.id)  
- `bet_type` (TEXT) – e.g., moneyline, spread, total  
- `bet_on` (TEXT) – team or outcome bet on  
- `odds` (NUMERIC) – American odds  
- `amount` (NUMERIC) – wagered amount  
- `result` (TEXT: pending, won, lost, void)  
- `created_at` (TIMESTAMPTZ, default now)

### Security Model
We use **Row Level Security (RLS)** to ensure that users can only view and modify their own data.  
- Only authenticated users can add or view their personalized bet analyses.
- Furthermore, for data privacy reasons, we restrict the Bets and Users tables to respect the users that use our product.


---

## Setup Instructions


### Prerequisites
- Python 3.10+
- Supabase account
