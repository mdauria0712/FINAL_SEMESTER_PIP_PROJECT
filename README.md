# Sports Betting Companion

## Database Design

### Overview
Sports Betting Companion is a data-driven tool that helps users find undervalued soccer bets using historical data and live sportsbook odds. With the 2026 World Cup coming to the US, many new bettors may not know much about the teams or players. Our goal is to make it easier for them to make smarter, data-backed bets. By connecting historical performance data with sportsbook odds, users can identify which teams are statistically undervalued compared to their betting lines.

### Entity Relationship Diagram
```mermaid
erDiagram
    USERS {
        id uuid PK
        username text
        email text
        password_hash text
        balance numeric
        created_at timestamptz
    }

    TEAMS {
        id bigint PK
        PI float
        Age float
        Poss float
        PrgC float
        PrgP float
        Gls float
        Ast float
        G_A float
        G_PK float
        G_A_PK float
        xG float
        xAG float
        xG_plus_xAG float
        npxG float
        npxG_plus_xAG float
        Group_Stage_Opponent_1 text
        Group_Stage_Opponent_2 text
        Group_Stage_Opponent_3 text
        RO16_Opponent text
        Quarterfinal_Opponent text
        SemiFinal_Opponent text
        Final_Opponent text
    }

    PLAYERS {
        id bigint PK
        name text
        team_id bigint FK
        position text
    }

    MATCHES {
        id bigint PK
        team1_id bigint FK
        team2_id bigint FK
        match_date timestamptz
        score_team1 int
        score_team2 int
        status text
    }

    BETS {
        id bigint PK
        user_id uuid FK
        match_id bigint FK
        bet_type text
        bet_on text
        odds numeric
        amount numeric
        result text
        created_at timestamptz
    }

    USERS ||--o{ BETS : places
    MATCHES ||--o{ BETS : contains
    TEAMS ||--o{ PLAYERS : has
    TEAMS ||--o{ MATCHES : team1
    TEAMS ||--o{ MATCHES : team2
```

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
- '#Pl' float -Players Used
- Age, float -Average Age of players used
- Possession, float -Average Possession for team throughout tournament
- PrgC, float - Progressive Passes Completed
- ProP, float - Progressive Passes Attempted
- Goals, float - Goals Scored
- Ast, float- Assists
- G + A, float, Goals + Assists
- G + A - PK, float - Goals not including penalty kicks
- xG, float - Expected Goals
- xAG, float - Expected Goals Against
- xG - xAG, float - Difference between Expected Goals and Excepted Goals against
- Group_Stage_Opponent_1, text -Name of the first opponent the team faces during the group stage of a tournament.
- Group_Stage_Opponent_2, text -Name of the second opponent in the group stage.
- Group_Stage_Opponent_3, text -Name of the third opponent in the group stage.
- RO16_Opponent, text -The opponent team the club/nation plays in the Round of 16 (knockout phase).
- Quarterfinal_Opponent, text -The opposing team in the Quarter-Final round, if the team advances that far.
- Column	Data Type	Explanation
Group_Stage_Opponent_1	text	Name of the first opponent the team faces during the group stage of a tournament.
Group_Stage_Opponent_2	text	Name of the second opponent in the group stage.
Group_Stage_Opponent_3	text	Name of the third opponent in the group stage.
RO16_Opponent	text	The opponent team the club/nation plays in the Round of 16 (knockout phase).
Quarterfinal_Opponent	text -The opposing team in the Quarter-Final round, if the team advances that far.
SemiFinal_Opponent, text	-The opponent faced in the Semi-Final stage of the tournament.
Final_Opponent, text -The final match opponent if the team reaches the championship game.

### Security Model
We use **Row Level Security (RLS)** to ensure that users can only view and modify their own data.  
- Only authenticated users can add or view their personalized bet analyses.
- Furthermore, for data privacy reasons, we restrict the Bets and Users tables to respect the users that use our product.  

### Prerequisites
- Python 3.10+
- Supabase account
