# Word Game - Supabase Integration Guide

## 🎮 Project Overview

This is a complete Supabase integration for your Word Game with real-time leaderboard functionality. The integration includes:

- **Real-time Leaderboard**: Live updates when players complete games
- **Player Profiles**: Track stats, achievements, and game history
- **Achievement System**: Unlock achievements based on performance
- **Game Sessions**: Complete game tracking with detailed analytics
- **Production-Ready**: Optimized for deployment with security best practices

## 📁 Project Structure

```
word-game/
├── supabase/
│   ├── schema.sql              # Complete database schema
│   └── migrations/             # Future migration files
├── js/
│   ├── supabase-client.js      # Supabase client & API functions
│   ├── game-integrated.js      # Game logic with Supabase
│   ├── ui.js                   # UI utilities
│   └── audio.js                # Sound effects (optional)
├── css/
│   ├── main.css                # Main styles
│   ├── animations.css          # Animations
│   └── responsive.css          # Mobile responsiveness
├── index.html                  # Main HTML file
├── .env.example                # Environment variables template
└── README.md                   # This file
```

## 🚀 Quick Start

### Step 1: Setup Supabase Database

1. **Go to your Supabase project**: https://wngypbnxqoqrcwyxmjwr.supabase.co

2. **Run the database schema**:
   - Navigate to SQL Editor in your Supabase dashboard
   - Copy the entire contents of `supabase-schema.sql`
   - Execute the SQL to create all tables, functions, and policies

3. **Verify tables created**:
   - Check that these tables exist:
     - `players`
     - `game_sessions`
     - `leaderboard`
     - `achievements`
     - `player_achievements`
     - `daily_challenges`

### Step 2: Configure Environment

1. **Get your Supabase credentials**:
   - Go to Project Settings → API
   - Copy your `Project URL` (already have: https://wngypbnxqoqrcwyxmjwr.supabase.co)
   - Copy your `anon` / `public` API key

2. **Update the configuration**:
   - Open `js/supabase-client.js`
   - Replace `'YOUR_SUPABASE_ANON_KEY_HERE'` with your actual anon key:
   ```javascript
   const SUPABASE_CONFIG = {
       url: 'https://wngypbnxqoqrcwyxmjwr.supabase.co',
       anonKey: 'YOUR_ACTUAL_ANON_KEY_HERE'
   };
   ```

### Step 3: Enable Real-time

1. **In Supabase Dashboard**:
   - Go to Database → Replication
   - Enable replication for these tables:
     - ✅ `leaderboard`
     - ✅ `players`
     - ✅ `game_sessions`

2. **Verify Real-time is working**:
   - The app will automatically subscribe to changes
   - Test by opening the leaderboard in two browser windows

### Step 4: Deploy

#### Option A: Vercel (Recommended)
```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel --prod
```

#### Option B: Netlify
```bash
# Install Netlify CLI
npm install -g netlify-cli

# Deploy
netlify deploy --prod
```

#### Option C: GitHub Pages
1. Push code to GitHub
2. Enable GitHub Pages in repository settings
3. Set source to main branch

## 🔧 Configuration Details

### Environment Variables

Create a `.env` file (for development):
```env
VITE_SUPABASE_URL=https://wngypbnxqoqrcwyxmjwr.supabase.co
VITE_SUPABASE_ANON_KEY=your_anon_key_here
```

### Supabase Security

The schema includes Row Level Security (RLS) policies:

- **Players**: Anyone can view, players can update their own profile
- **Game Sessions**: Players can view/create/update their own sessions
- **Leaderboard**: Read-only for everyone
- **Achievements**: Read-only for everyone
- **Player Achievements**: Players can unlock their own achievements

### API Rate Limits

Default Supabase limits:
- Free tier: 500MB database, 2GB bandwidth/month
- Real-time: 200 concurrent connections
- API: No hard rate limit, but subject to fair use

## 📊 Database Schema Overview

### Players Table
Stores user profiles and aggregated statistics:
- Username, email, avatar
- Total games, wins, score
- Win streak tracking
- Created/last active timestamps

### Game Sessions Table
Tracks individual game plays:
- Player reference
- Difficulty, target word, attempts
- Time taken, score, bonuses
- Guess history (JSONB)
- Win/loss status

### Leaderboard Table
Real-time updated rankings:
- Aggregated player stats
- Calculated rankings
- Win rates, averages
- Auto-updated via triggers

### Achievements System
- Predefined achievements
- Player unlocks tracking
- Points and categories
- Automatic checking after games

## 🎯 Features

### Real-time Leaderboard
- Updates instantly when any player completes a game
- Top 100 players displayed
- Filter by difficulty level
- Shows rank, total score, games played, win rate

### Player Statistics
- Total games played
- Win/loss record
- Win streaks (current and longest)
- Total score accumulation
- Highest single-game score

### Achievement System
15+ achievements including:
- **First Victory**: Win your first game (10 pts)
- **Hot Streak**: Win 3 games in a row (25 pts)
- **Unstoppable**: Win 5 games in a row (50 pts)
- **Legend**: Win 10 games in a row (100 pts)
- **Century**: Score 100 points in one game (20 pts)
- **Speed Demon**: Win in under 30 seconds (60 pts)
- And more...

### Game Modes
Three difficulty levels:
- **Easy**: 3 letters, 6 attempts, 3 minutes
- **Medium**: 4 letters, 7 attempts, 4 minutes
- **Hard**: 5 letters, 8 attempts, 5 minutes

### Scoring System
Score calculation:
```
Base Score = 100 points
Time Bonus = Time Remaining × 2
Efficiency Bonus = Unused Attempts × 10
Difficulty Multiplier = 1.0 (easy), 1.5 (medium), 2.0 (hard)

Final Score = (Base + Time Bonus + Efficiency Bonus) × Multiplier
```

## 🔍 Testing

### Test Database Connection
Open browser console and run:
```javascript
await testConnection()
```

### Test Player Creation
```javascript
const result = await createOrGetPlayer('TestPlayer');
console.log(result);
```

### Test Leaderboard
```javascript
const leaderboard = await getLeaderboard(10);
console.log(leaderboard);
```

### Test Real-time
1. Open game in two browser windows
2. Complete a game in one window
3. Watch leaderboard update in the other window

## 🐛 Troubleshooting

### "Supabase client not initialized"
- Ensure Supabase CDN script is loaded before your scripts
- Check browser console for script loading errors
- Verify your anon key is correct

### "Failed to load leaderboard"
- Check database tables exist
- Verify RLS policies are configured
- Check browser network tab for API errors

### Real-time not working
- Ensure replication is enabled for tables
- Check Supabase dashboard for real-time status
- Verify subscription code is running

### CORS errors
- Ensure your domain is allowed in Supabase settings
- For local development, use `localhost` or `127.0.0.1`

## 🚀 Performance Optimizations

### Database Indexes
The schema includes optimized indexes on:
- Player usernames
- Leaderboard scores (for fast ranking)
- Game session timestamps
- Achievement lookups

### Caching Strategy
- Player data cached in localStorage
- Leaderboard refreshed every 5 minutes
- Real-time updates for instant changes

### Optimization Tips
1. Use database functions for complex queries
2. Leverage indexes for sorting/filtering
3. Implement pagination for large datasets
4. Use connection pooling for high traffic

## 📱 Mobile Responsiveness

The app is fully responsive:
- Touch-friendly keyboard
- Optimized for portrait and landscape
- Swipe gestures support
- Progressive Web App (PWA) ready

## 🔐 Security Best Practices

1. **Never expose Supabase service key**: Only use anon key in client
2. **Use RLS policies**: Enforce data access rules at database level
3. **Validate input**: Sanitize all user inputs
4. **Rate limiting**: Implement on API endpoints
5. **HTTPS only**: Always use secure connections

## 📈 Scaling Considerations

### For High Traffic
1. **Upgrade Supabase tier**: Get more concurrent connections
2. **Implement caching**: Use Redis for leaderboard cache
3. **Database optimization**: Add more indexes, optimize queries
4. **CDN**: Use for static assets
5. **Load balancing**: For multiple server instances

### Monitoring
- Set up Supabase monitoring dashboard
- Track API usage and response times
- Monitor database connection pool
- Set up alerts for errors

## 🎨 Customization

### Change Color Scheme
Edit CSS variables in `css/main.css`:
```css
:root {
    --primary-color: #667eea;  /* Your brand color */
    --secondary-color: #764ba2;
    /* ... more colors */
}
```

### Add New Achievements
1. Add to database:
```sql
INSERT INTO achievements (achievement_key, name, description, icon, points)
VALUES ('custom_achievement', 'Custom Name', 'Description', '🏆', 50);
```

2. Add check logic in `checkAndUnlockAchievements()` function

### Modify Scoring
Edit `DIFFICULTY_CONFIG` in `game-integrated.js`:
```javascript
const DIFFICULTY_CONFIG = {
    easy: {
        scoreMultiplier: 1.0  // Adjust multiplier
    }
    // ...
};
```

## 📝 API Reference

### Main Functions

#### Player Management
- `createOrGetPlayer(username, email)` - Create or retrieve player
- `getPlayerById(playerId)` - Get player by ID
- `updatePlayer(playerId, updates)` - Update player profile

#### Game Sessions
- `createGameSession(playerId, config)` - Start new game
- `updateGameSession(sessionId, updates)` - Update during play
- `completeGameSession(sessionId, results)` - Finish game
- `getPlayerGameHistory(playerId, limit)` - Get past games

#### Leaderboard
- `getLeaderboard(limit)` - Get top players
- `getPlayerRank(playerId)` - Get player's rank
- `subscribeToLeaderboard(callback)` - Real-time updates

#### Achievements
- `getAllAchievements()` - Get all achievements
- `getPlayerAchievements(playerId)` - Get unlocked achievements
- `unlockAchievement(playerId, key)` - Unlock achievement
- `checkAndUnlockAchievements(playerId, stats)` - Auto-check

## 🤝 Contributing

To contribute to this project:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

This project is open source and available under the MIT License.

## 🆘 Support

For issues or questions:
1. Check this README
2. Review Supabase documentation
3. Check browser console for errors
4. Open a GitHub issue with details

## 🎉 Credits

Built with:
- [Supabase](https://supabase.com) - Backend & Real-time
- [Gemini API](https://ai.google.dev) - Word generation
- Modern vanilla JavaScript - No framework overhead
- CSS Grid & Flexbox - Responsive layouts

---

**Made with ❤️ for word game enthusiasts**
