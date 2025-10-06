# Sports Betting Companion

## Database Design

### Overview
Sports Betting Companion is a data-driven tool that helps users find undervalued soccer bets using historical data and live sportsbook odds. With the 2026 World Cup coming to the US, many new bettors may not know much about the teams or players — our goal is to make it easier for them to make smarter, data-backed bets. By connecting historical performance data with sportsbook odds, users can identify which teams are statistically undervalued compared to their betting lines.

### Entity Relationship Diagram


### Tables Description

#### 1. `teams`
Stores information about each national team participating in the tournament.
- `team_id` (UUID): Primary key  
- `country_name` (TEXT): Full team name  
- `fifa_rank` (INTEGER): Team’s latest FIFA ranking  
- `confederation` (TEXT): Continental federation (e.g., UEFA, CONMEBOL)

#### 2. `sportsbooks`
Holds data about each sportsbook used for odds collection.
- `sportsbook_id` (UUID): Primary key  
- `name` (TEXT): Sportsbook name (e.g., DraftKings, Caesars)  
- `country` (TEXT): Country of operation

#### 3. `odds`
Links teams to their current odds on each sportsbook.
- `odds_id` (UUID): Primary key  
- `team_id` (UUID, FK → teams.team_id): Associated team  
- `sportsbook_id` (UUID, FK → sportsbooks.sportsbook_id): Associated sportsbook  
- `win_tourney` (INTEGER): Odds to win the tournament  
- `qualify_group` (INTEGER): Odds to qualify from group stage  

#### 4. `historical_stats`
Stores past performance data for each team (to calculate undervalued bets).
- `stat_id` (UUID): Primary key  
- `team_id` (UUID, FK → teams.team_id): Associated team  
- `year` (INTEGER): Year of the competition  
- `wins` (INTEGER): Number of matches won  
- `losses` (INTEGER): Number of matches lost  
- `goals_for` (INTEGER): Goals scored  
- `goals_against` (INTEGER): Goals conceded  

#### 5. `bet_analysis`
Combines odds and historical data to determine value bets.
- `analysis_id` (UUID): Primary key  
- `team_id` (UUID, FK → teams.team_id)  
- `expected_prob` (FLOAT): Probability based on historical stats  
- `implied_prob` (FLOAT): Probability based on sportsbook odds  
- `value_score` (FLOAT): Expected return difference  
- `timestamp` (TIMESTAMPTZ): When analysis was run

### Security Model
We use **Row Level Security (RLS)** to ensure that users can only view and modify their own data.  
- Only authenticated users can add or view their personalized bet analyses.  
- Public read-only access may be allowed for team and odds data.  
- All tables with user-related or generated analysis data have RLS enabled with policies restricting access by `user_id`.

---

## Setup Instructions


### Prerequisites
- Python 3.10+  
- Supabase account  
- Installed Supabase Python client  
