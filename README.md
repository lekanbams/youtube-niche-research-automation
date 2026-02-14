# YouTube Trending Niche Research and Content Idea Generator

An intelligent n8n workflow that automatically analyzes YouTube's trending videos, identifies top-performing niches, and generates AI-powered content ideas complete with titles, descriptions, hooks, and SEO keywords. Results are automatically saved to Google Sheets and Airtable for easy content planning.

## 📊 Overview

This automation leverages YouTube's trending data and Google Gemini AI to provide daily content strategy insights. It helps content creators stay ahead of trends by identifying hot niches and generating ready-to-use video concepts.

## ✨ Features

- **Daily Trend Analysis**: Automatically fetches YouTube's top 50 trending videos every day at 9 AM
- **Smart Niche Ranking**: Analyzes videos by category and ranks niches based on views, engagement, and performance metrics
- **AI Content Generation**: Uses Google Gemini to create 10 unique content ideas per niche (20 total ideas daily)
- **SEO Optimization**: Each idea includes targeted keywords, hooks, and difficulty ratings
- **Dual Storage**: Saves results to both Google Sheets and Airtable for flexible workflow integration
- **Engagement Metrics**: Tracks views, likes, comments, and calculates engagement rates
- **Top Performer Analysis**: Identifies top 3 videos in each niche for inspiration

## 🏗️ Workflow Architecture

```
Schedule Trigger (Daily at 9 AM)
    ↓
Set Configuration (API Keys, Settings)
    ↓
Fetch YouTube Trending Videos (Top 50)
    ↓
Process & Rank Niches (Analyze by Category)
    ↓
Prepare Data for AI Analysis
    ↓
Generate Content Ideas (Google Gemini)
    ↓
Split Ideas into Individual Records
    ↓
Format Data for Storage
    ↓
Save to Google Sheets + Airtable
```

## 📋 Prerequisites

### Required Accounts & API Keys

1. **n8n Instance** (Cloud or Self-hosted)
   - [Get started with n8n](https://n8n.io/)

2. **YouTube Data API v3** (Free tier available)
   - Create project in [Google Cloud Console](https://console.cloud.google.com/)
   - Enable YouTube Data API v3
   - Create API key
   - Free quota: 10,000 units/day (this workflow uses ~5 units/day)

3. **Google Gemini API** (Free tier available)
   - Sign up at [Google AI Studio](https://makersuite.google.com/app/apikey)
   - Generate API key
   - Free tier: 60 requests/minute

4. **Google Sheets**
   - Any Google account
   - OAuth2 authentication required in n8n

5. **Airtable** (Optional but recommended)
   - Free account available at [Airtable](https://airtable.com/)
   - Create a personal access token
   - Free tier: Unlimited bases, 1,200 records per base

### Technical Requirements

- n8n version 1.0.0 or higher
- Internet connection for API calls
- Google Cloud project with YouTube API enabled

## 🚀 Installation

### Step 1: Set Up YouTube API

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project (or select existing)
3. Navigate to **APIs & Services** → **Library**
4. Search for "YouTube Data API v3" and enable it
5. Go to **Credentials** → **Create Credentials** → **API Key**
6. Copy your API key (restrict it to YouTube Data API for security)

### Step 2: Set Up Google Sheets

1. Create a new Google Sheet
2. Add these column headers in the first row:
   ```
   Niche | Content_Idea_Title | Description | Hook | Keywords | Difficulty | Generated_Date
   ```
3. Copy the Sheet ID from the URL:
   ```
   https://docs.google.com/spreadsheets/d/[SHEET_ID]/edit
   ```

### Step 3: Set Up Airtable (Optional)

1. Log into Airtable
2. Create a new base called "YouTube Content Database"
3. Create a table called "Content Ideas"
4. Add these fields:
   - **Niche** (Single line text)
   - **Content_Idea_Title** (Single line text)
   - **Description** (Long text)
   - **Hook** (Long text)
   - **Keywords** (Long text)
   - **Difficulty** (Number)
   - **Generated_Date** (Date)
5. Go to [Create Token](https://airtable.com/create/tokens)
6. Create a token with:
   - Scope: `data.records:read` and `data.records:write`
   - Access: Select your base
7. Copy the token

### Step 4: Import n8n Workflow

1. Download `YouTube_Niche_Research_and_Content_Idea_Generator.json`
2. Open your n8n instance
3. Click **Workflows** → **Import from File**
4. Select the downloaded JSON file
5. Click **Import**

### Step 5: Configure Credentials

#### A. YouTube API Key

1. Locate the **"Workflow Configuration"** node
2. Click to edit
3. In the `youtubeApiKey` field, replace `YOUR_YOUTUBE_API_KEY` with your actual API key

#### B. Google Gemini API

1. Click the **"Google Gemini Chat Model"** node
2. Click **Credentials** → **Create New**
3. Select **Google PaLM/Gemini API**
4. Enter your Gemini API key
5. Save and select the credential

#### C. Google Sheets OAuth

1. Click the **"Save to Google Sheets"** node
2. Click **Credentials** → **Create New**
3. Select **Google Sheets OAuth2**
4. Follow the OAuth flow to authorize
5. Save the credential
6. Update the `documentId` field with your Sheet ID

#### D. Airtable Token (Optional)

1. Click the **"Save to Airtable"** node
2. Click **Credentials** → **Create New**
3. Select **Airtable Token API**
4. Enter your Airtable personal access token
5. Save the credential
6. Update `baseId` and `tableId` with your base and table IDs

## ⚙️ Configuration Options

### Customize Analysis Settings

Edit the **"Workflow Configuration"** node:

```javascript
{
  youtubeApiKey: "YOUR_KEY",
  timeframe7Days: 7,        // Not currently used but reserved for future filtering
  timeframe30Days: 30,      // Not currently used but reserved for future filtering
  topNichesCount: 2         // Number of niches to analyze (default: 2)
}
```

### Adjust Number of Content Ideas

1. Open the **"Content Ideas Generator"** node
2. Modify the system message:
   ```
   generate 10 creative, actionable content ideas
   ```
   Change "10" to your desired number (recommended: 5-15)

### Change Region for Trending Videos

1. Open the **"Fetch YouTube Trending Data"** node
2. Find the `regionCode` parameter
3. Change from "US" to your country code:
   - `GB` - United Kingdom
   - `CA` - Canada
   - `IN` - India
   - `AU` - Australia
   - [Full list of region codes](https://www.iso.org/obp/ui/#search)

### Modify Schedule

1. Open the **"Schedule Trigger"** node
2. Current setting: Daily at 9 AM
3. To change:
   - Click the trigger settings
   - Adjust `triggerAtHour` (0-23 for hour of day)
   - Or change to different interval type (hourly, weekly, etc.)

### Adjust Number of Trending Videos

1. Open **"Fetch YouTube Trending Data"** node
2. Find `maxResults` parameter
3. Change from "50" to your preference (1-50)
4. **Note**: Higher numbers = more comprehensive analysis but more API quota usage

## 📊 Understanding the Output

### Google Sheets/Airtable Columns

| Column | Description | Example |
|--------|-------------|---------|
| **Niche** | YouTube category ID | "Gaming", "Education", "Entertainment" |
| **Content_Idea_Title** | AI-generated video title | "10 Automation Hacks That Will Save You 10 Hours/Week" |
| **Description** | Video concept overview | "A practical tutorial showing viewers how to set up..." |
| **Hook** | Opening line to grab attention | "What if I told you that you're wasting 10 hours every week..." |
| **Keywords** | SEO-optimized keywords | "automation, productivity, n8n, workflow" |
| **Difficulty** | Production complexity (1-10) | 5 (1=easy, 10=very complex) |
| **Generated_Date** | When idea was created | "2026-02-14T09:00:00Z" |

### Niche Ranking Metrics

The workflow analyzes each niche based on:

- **Total Views**: Combined views across all videos in the category
- **Average Views**: Mean views per video
- **Engagement Rate**: (Likes + Comments) / Views
- **Top Performers**: Top 3 videos by views
- **Unique Channels**: Number of different creators
- **Engagement Score**: avgViews × engagementRate × 1000 (used for ranking)

## 📸 Sample Output

See the `/screenshots` folder for examples of:
- n8n workflow canvas
- Google Sheets populated with ideas
- Airtable database view
- AI-generated content samples
- Niche analysis dashboard

*(Add your screenshots after setup)*

## 🎯 Use Cases

### For Content Creators
- Daily content inspiration based on real trends
- SEO-optimized titles and keywords
- Difficulty ratings to plan production capacity
- Hooks to improve video performance

### For Marketing Teams
- Trend monitoring and analysis
- Competitive intelligence
- Content calendar planning
- Multi-channel strategy development

### For YouTube Agencies
- Client reporting and insights
- Niche opportunity identification
- Scalable content ideation
- Data-driven strategy recommendations

## 🔧 Customization Ideas

### Add Custom Prompts

Modify the **"Content Ideas Generator"** system message to:
- Target specific audience demographics
- Focus on particular content formats (shorts, long-form, etc.)
- Include brand voice guidelines
- Generate scripts instead of just ideas
- Create thumbnail concepts

### Extend with Additional Tools

Consider adding nodes for:
- **Slack Notifications**: Alert team when new ideas are generated
- **Email Digest**: Weekly summary of top performing niches
- **Notion Integration**: Sync with team wiki or content calendar
- **Discord Webhook**: Share insights with community
- **Twitter API**: Track trending hashtags alongside YouTube data

### Advanced Analytics

Add custom JavaScript nodes to:
- Track niche performance over time
- Compare week-over-week trends
- Identify emerging vs declining niches
- Calculate content saturation scores
- Predict viral potential

## ⚠️ Important Notes

### API Quotas & Limits

**YouTube Data API v3**:
- Free quota: 10,000 units/day
- This workflow uses ~5 units per execution
- 1 execution/day = 35 units/week (well within limits)
- Monitor usage in Google Cloud Console

**Google Gemini**:
- Free tier: 60 requests/minute
- This workflow makes 2 requests per execution
- Monitor usage in Google AI Studio

**Google Sheets**:
- API limits: 500 requests per 100 seconds per project
- This workflow makes 1-20 requests per execution (depending on ideas generated)

**Airtable**:
- Free tier: 5 requests/second
- This workflow makes 1-20 requests per execution

### YouTube API Categories

YouTube uses numeric category IDs. Common ones include:
- `1` - Film & Animation
- `2` - Autos & Vehicles
- `10` - Music
- `15` - Pets & Animals
- `17` - Sports
- `19` - Travel & Events
- `20` - Gaming
- `22` - People & Blogs
- `23` - Comedy
- `24` - Entertainment
- `25` - News & Politics
- `26` - Howto & Style
- `27` - Education
- `28` - Science & Technology

You can map these IDs to friendly names by adding a mapping node.

### Data Freshness

- YouTube's "trending" updates multiple times per day
- Running once daily at 9 AM captures morning trends
- For more real-time insights, increase execution frequency
- Consider regional time zones when scheduling

## 🐛 Troubleshooting

### "Invalid API Key" Error

1. Verify API key is correct in **Workflow Configuration** node
2. Ensure YouTube Data API v3 is enabled in Google Cloud Console
3. Check that API key restrictions allow YouTube Data API
4. Confirm billing is enabled on Google Cloud project

### "Quota Exceeded" Error

1. Check your quota usage in [Google Cloud Console](https://console.cloud.google.com/apis/api/youtube.googleapis.com/quotas)
2. Reduce `maxResults` in the fetch node
3. Decrease execution frequency
4. Request quota increase from Google (takes 24-48 hours)

### No Content Ideas Generated

1. Check that **"Google Gemini Chat Model"** credentials are valid
2. Verify the **"Structured Output Parser"** is properly connected
3. Review execution logs for AI errors
4. Ensure niche data is properly formatted before AI processing

### Google Sheets Not Updating

1. Verify OAuth credentials are still valid (re-authenticate if needed)
2. Check that Sheet ID is correct
3. Ensure column headers match exactly (case-sensitive)
4. Confirm the service account has edit permissions

### Airtable Errors

1. Verify personal access token is valid
2. Check base ID and table ID are correct
3. Ensure field names match exactly
4. Confirm token has write permissions to the base

### Workflow Doesn't Execute on Schedule

1. Check that workflow is **Active** (toggle in top-right)
2. Verify schedule trigger settings
3. Check n8n execution logs
4. Ensure n8n instance is running (for self-hosted)

## 🔒 Security Best Practices

- **Never commit API keys** to public repositories
- Use n8n's credential system instead of hardcoding keys
- Restrict YouTube API key to only YouTube Data API
- Use Google OAuth2 instead of service account keys when possible
- Regularly rotate Airtable personal access tokens
- Set up API usage alerts in Google Cloud Console
- Monitor for unusual API activity

## 💡 Pro Tips

1. **Batch Processing**: Run once daily to stay within API limits while maintaining freshness
2. **Niche Focus**: Modify the code to filter for specific categories you care about
3. **Quality Over Quantity**: Start with 2 niches and 10 ideas each. Scale up once you can handle the volume
4. **Keyword Research**: Cross-reference generated keywords with actual YouTube search trends
5. **A/B Testing**: Generate multiple titles per idea and test which performs better
6. **Trend Timing**: Check when your niche typically trends (weekends vs weekdays) and adjust schedule
7. **Archive Old Ideas**: Set up a separate workflow to move ideas older than 30 days to an archive sheet

## 📈 Measuring Success

Track these metrics to evaluate the automation's value:

- **Content Production Rate**: How many ideas you actually produce
- **View Performance**: Compare videos based on generated ideas vs organic ideas
- **Time Saved**: Hours saved on content planning
- **Trend Hit Rate**: Percentage of generated ideas that align with emerging trends
- **SEO Performance**: Ranking improvements for generated keywords

## 🤝 Contributing

Ideas for improvements:
- Add sentiment analysis to gauge audience mood
- Integrate with YouTube Analytics API for channel-specific insights
- Create competitor tracking for specific channels
- Add thumbnail generation using AI image models
- Build a scoring system for idea prioritization

## 📝 License

This workflow is provided as-is for educational and commercial use. Modify as needed for your use case.

## 📞 Support

For issues with:
- **n8n**: [n8n Community Forum](https://community.n8n.io/)
- **YouTube API**: [YouTube API Documentation](https://developers.google.com/youtube/v3)
- **Google Gemini**: [Google AI Documentation](https://ai.google.dev/docs)
- **Airtable**: [Airtable Support](https://support.airtable.com/)

## 🎯 Future Enhancements

Potential improvements:

- [ ] Add competitor channel tracking
- [ ] Integrate with YouTube Analytics for channel performance
- [ ] Create automated thumbnail concepts
- [ ] Add script generation for selected ideas
- [ ] Build trend prediction ML model
- [ ] Create content performance dashboard
- [ ] Add multi-language support
- [ ] Integrate with video editing tools
- [ ] Build content calendar automation
- [ ] Add audience sentiment analysis

---

**Made with ❤️ using n8n + AI**

*Last Updated: February 2026*
