# 🎬 HeyGen AI Avatar News Video Generator

[![n8n](https://img.shields.io/badge/n8n-Automation-orange)](https://n8n.io/)
[![HeyGen](https://img.shields.io/badge/HeyGen-AI%20Avatars-blue)](https://heygen.com/)
[![Google Gemini](https://img.shields.io/badge/Gemini-AI%20Script-brightgreen)](https://ai.google.dev/)
[![Automated](https://img.shields.io/badge/Fully-Automated-success)](https://github.com)

> **Fully automated AI video generator that transforms RSS news feeds into professional avatar-presented videos. Fetches headlines, generates scripts with Gemini AI, and creates videos with HeyGen's realistic AI avatars.**

Simply click execute and watch as your workflow automatically creates a news video from the latest BBC headlines—complete with an AI avatar presenter and professionally generated script.

----

🎥 **Project Demo:** 

<video src="https://github.com/Awaisali36/ai-avatar-video-generation-system/raw/main/Vidoes%20/preview_video_target.mp4" controls width="600">
  Your browser does not support the video tag.
</video>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [How It Works](#-how-it-works)
- [System Architecture](#-system-architecture)
- [Tech Stack](#-tech-stack)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Workflow Breakdown](#-workflow-breakdown)
- [Customization](#-customization)
- [Troubleshooting](#-troubleshooting)
- [Best Practices](#-best-practices)
- [License](#-license)

---

## 🎯 Overview

**HeyGen AI Avatar News Video Generator** is an intelligent n8n workflow that fully automates the creation of news videos. It combines RSS feeds, Google Gemini AI for script generation, and HeyGen's cutting-edge AI avatars to produce professional-quality videos without any manual editing.

### What Makes This Special?

- 🤖 **Fully Automated** - No manual intervention from feed to final video
- 📰 **RSS Integration** - Pulls latest news from any RSS feed (BBC News by default)
- 🧠 **AI Script Generation** - Gemini 2.5 Flash creates engaging news scripts
- 👤 **Realistic AI Avatars** - HeyGen's premium avatars present your content
- 🎙️ **Natural Voice Synthesis** - Automatic male voice selection
- ⏱️ **Smart Polling** - Waits for video generation with exponential backoff
- 📥 **Auto Download** - Saves completed video automatically
- 🔄 **Error Handling** - Robust retry logic and status checking

### Who Is This For?

- 📺 **Content Creators** - Generate news recap videos daily
- 🎥 **Social Media Managers** - Create engaging video content at scale
- 📱 **News Channels** - Automate news video production
- 🏢 **Marketing Teams** - Quick video content from industry news
- 🎓 **Educators** - Create educational news summaries
- 🤖 **Automation Enthusiasts** - Learn advanced n8n workflows

---

## ✨ Key Features

### 📰 **RSS Feed Integration**
- Fetches latest headlines from BBC News RSS
- Processes multiple articles automatically
- Creates digestible news summaries
- Supports any RSS feed URL
- Configurable number of headlines (default: 5)

### 🧠 **AI Script Generation (Google Gemini)**
- Uses Gemini 2.5 Flash model
- Converts headlines into engaging scripts
- Natural conversational tone
- Optimized for avatar delivery
- Character limit enforcement (1400 chars)
- Automatic script validation

### 🎬 **HeyGen Avatar Video Creation**
- Access to HeyGen's avatar library
- Automatic non-premium avatar selection
- Smart male voice picker
- Customizable video dimensions (1280x720)
- Professional avatar presentation
- Natural gestures and expressions

### 🎙️ **Voice Synthesis**
- Automatic male voice selection from HeyGen library
- Fallback voice ID for reliability
- Natural speech patterns
- Adjustable speech speed
- Gender preference matching

### ⏱️ **Smart Video Processing**
- Polls HeyGen API for completion status
- Exponential backoff strategy
- Maximum wait time protection
- Status checking (completed/failed/waiting/pending/processing)
- Automatic retry on transient failures

### 📥 **Video Download & Management**
- Automatic video file download
- Thumbnail and GIF URLs extracted
- Complete metadata capture
- Job ID tracking
- Duration and creation timestamps

---

## 🔄 How It Works

### User Experience

```
Step 1: Click "Execute Workflow" in n8n

Step 2: Workflow automatically:
        ├─ Fetches BBC News RSS feed
        ├─ Extracts top 5 headlines
        ├─ Creates news digest
        ├─ Sends to Gemini AI for script generation
        ├─ Fetches HeyGen avatars
        ├─ Fetches HeyGen voices
        ├─ Selects best avatar and voice
        ├─ Submits video generation request
        ├─ Polls for completion (every 6-30 seconds)
        └─ Downloads finished video

Step 3: Video ready! (~2-5 minutes total)
        • Professional AI avatar presentation
        • Natural voice narration
        • HD quality (1280x720)
        • Ready to publish
```

### Technical Flow

```
Manual Trigger
     ↓
RSS Feed Reader (BBC News)
     ↓
Extract Top 5 Headlines
     ↓
Create News Digest
     ↓
Google Gemini API
├─ Generate engaging script
└─ Validate & truncate to 1400 chars
     ↓
Prepare Configuration
     ↓
Parallel Fetch:
├─ HeyGen Avatars → Pick Non-Premium Avatar
└─ HeyGen Voices → Pick Male Voice
     ↓
Generate Video Request (HeyGen API)
     ↓
Extract Video ID
     ↓
Poll Video Status Loop:
├─ Check Status
├─ Switch (completed/failed/waiting/pending/processing)
├─ If Still Processing:
│   ├─ Calculate Backoff Wait Time
│   ├─ Wait (6-30 seconds)
│   └─ Poll Again
└─ If Completed:
    ├─ Download Video File
    ├─ Extract Metadata
    └─ Return Results
```

---

## 🏗️ System Architecture

### Data Flow

```
┌─────────────────────────────────────────────────────────────┐
│                      INPUT LAYER                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────┐          │
│  │         RSS Feed Reader                      │          │
│  │         (BBC News RSS)                       │          │
│  │                                               │          │
│  │  Fetches: Title, Description, Link, PubDate  │          │
│  │  Returns: Array of news articles             │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
└───────────────────────┼──────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│                   PROCESSING LAYER                           │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────┐          │
│  │        Create News Digest                    │          │
│  │        (JavaScript Code Node)                │          │
│  │                                               │          │
│  │  • Select first 5 headlines                  │          │
│  │  • Format as bullet points                   │          │
│  │  • Create single digest string               │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
│                       ↓                                      │
│  ┌──────────────────────────────────────────────┐          │
│  │        Google Gemini API                     │          │
│  │        (Script Generation)                   │          │
│  │                                               │          │
│  │  Input: News digest                          │          │
│  │  Model: Gemini 2.5 Flash                     │          │
│  │  Output: Engaging news script                │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
│                       ↓                                      │
│  ┌──────────────────────────────────────────────┐          │
│  │        Extract & Validate Script             │          │
│  │        (JavaScript Code Node)                │          │
│  │                                               │          │
│  │  • Extract text from Gemini response         │          │
│  │  • Trim whitespace                           │          │
│  │  • Enforce 1400 character limit              │          │
│  │  • Validate script is not empty              │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
└───────────────────────┼──────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│                   HEYGEN PREPARATION                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────┐          │
│  │        Set Default Configuration             │          │
│  │        (Edit Fields Node)                    │          │
│  │                                               │          │
│  │  • Avatar ID: 9bb55566ca0242609aa1fea1f7144656│         │
│  │  • Voice ID: 0912d8004fd948a9aab02fffd264a3c1 │         │
│  │  • Dimensions: 1280x720                      │          │
│  │  • Script: Pass through                      │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
│          ┌────────────┴────────────┐                        │
│          ↓                         ↓                        │
│  ┌──────────────────┐      ┌──────────────────┐           │
│  │  Fetch Avatars   │      │  Fetch Voices    │           │
│  │  (HeyGen API)    │      │  (HeyGen API)    │           │
│  └────────┬─────────┘      └────────┬─────────┘           │
│           ↓                         ↓                        │
│  ┌──────────────────┐      ┌──────────────────┐           │
│  │  Pick Avatar     │      │  Pick Male Voice │           │
│  │  (Non-Premium)   │      │  (Preference)    │           │
│  └────────┬─────────┘      └────────┬─────────┘           │
│           │                         │                        │
│           └────────────┬────────────┘                        │
│                        │                                      │
└────────────────────────┼──────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────────────┐
│                   VIDEO GENERATION                           │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────┐          │
│  │     HeyGen Video Generation Request          │          │
│  │                                               │          │
│  │  POST /v2/video/generate                     │          │
│  │  {                                            │          │
│  │    "video_inputs": [{                        │          │
│  │      "character": { avatar_id, avatar_style },│         │
│  │      "voice": { input_text, voice_id, speed } │         │
│  │    }],                                        │          │
│  │    "dimension": { width, height }            │          │
│  │  }                                            │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
│                       ↓                                      │
│  ┌──────────────────────────────────────────────┐          │
│  │     Extract Video ID                         │          │
│  │     Initialize Polling State                 │          │
│  │                                               │          │
│  │  • video_id: From HeyGen response            │          │
│  │  • attempts: 0                               │          │
│  │  • max_poll_seconds: 180                     │          │
│  │  • poll_interval_seconds: 6                  │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
└───────────────────────┼──────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│                   POLLING LOOP                               │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────┐          │
│  │  ┌─────────────────────────────────────┐     │          │
│  │  │   Poll Video Status                 │     │          │
│  │  │   GET /v1/video_status.get          │     │          │
│  │  └────────────┬────────────────────────┘     │          │
│  │               │                               │          │
│  │               ↓                               │          │
│  │  ┌─────────────────────────────────────┐     │          │
│  │  │   Switch on Status                  │     │          │
│  │  ├─────────────────────────────────────┤     │          │
│  │  │  • completed → Download Video       │     │          │
│  │  │  • failed → Throw Error             │     │          │
│  │  │  • waiting → Continue Loop          │     │          │
│  │  │  • pending → Continue Loop          │     │          │
│  │  │  • processing → Continue Loop       │     │          │
│  │  └────────────┬────────────────────────┘     │          │
│  │               │                               │          │
│  │               ↓ (if not completed)           │          │
│  │  ┌─────────────────────────────────────┐     │          │
│  │  │   Calculate Backoff Wait            │     │          │
│  │  │   • Base: 6 seconds                 │     │          │
│  │  │   • Add: +2s per attempt            │     │          │
│  │  │   • Max: 30 seconds                 │     │          │
│  │  │   • Check: Total time < 180s        │     │          │
│  │  └────────────┬────────────────────────┘     │          │
│  │               │                               │          │
│  │               ↓                               │          │
│  │  ┌─────────────────────────────────────┐     │          │
│  │  │   Wait Node                         │     │          │
│  │  │   (Dynamic duration)                │     │          │
│  │  └────────────┬────────────────────────┘     │          │
│  │               │                               │          │
│  │               └──────────┐                    │          │
│  │                          │                    │          │
│  └──────────────────────────┼────────────────────┘          │
│                             │ (Loop back)                    │
└─────────────────────────────┼──────────────────────────────────┘
                              ↓ (completed)
┌─────────────────────────────────────────────────────────────┐
│                   OUTPUT LAYER                               │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────┐          │
│  │     Download Video File                      │          │
│  │     (HTTP Request - Binary Response)         │          │
│  └────────────────────┬─────────────────────────┘          │
│                       │                                      │
│                       ↓                                      │
│  ┌──────────────────────────────────────────────┐          │
│  │     Format Output Metadata                   │          │
│  │                                               │          │
│  │  • video_url: Direct download link           │          │
│  │  • thumbnail_url: Preview image              │          │
│  │  • gif_url: Animated preview                 │          │
│  │  • job_id: HeyGen job identifier             │          │
│  │  • status: completed                         │          │
│  │  • created_at: Timestamp                     │          │
│  │  • duration: Video length                    │          │
│  │  • message: Success confirmation             │          │
│  └──────────────────────────────────────────────┘          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Category | Technology | Purpose |
|----------|------------|---------|
| **Automation** | n8n | Workflow orchestration |
| **RSS Feed** | BBC News RSS | News headline source |
| **AI Script** | Google Gemini 2.5 Flash | Script generation |
| **Avatar Platform** | HeyGen | AI avatar video creation |
| **Voice Synthesis** | HeyGen Voices | Natural speech generation |
| **Programming** | JavaScript (n8n Code nodes) | Data processing and logic |
| **API Integration** | HTTP Request nodes | External service communication |

---

## 📦 Prerequisites

### Required Accounts & API Keys

| Service | Required? | Purpose | Cost |
|---------|-----------|---------|------|
| **n8n** | ✅ Yes | Run workflows | Free (self-hosted) or $20/mo |
| **Google AI Studio** | ✅ Yes | Gemini API access | Free tier: 15 requests/min |
| **HeyGen** | ✅ Yes | AI avatar videos | $24-$120/mo (based on plan) |

### API Keys Needed

- ✅ Google Gemini API Key
- ✅ HeyGen API Key (X-Api-Key)

### Technical Requirements

- n8n instance (v1.0+)
- Stable internet connection
- ~5 GB storage for video files

---

## 🚀 Installation

### Step 1: Download Workflow

1. Download `My workflow.json` from repository
2. Save to your computer

### Step 2: Import to n8n

1. Open your n8n instance
2. Click **"Workflows"** → **"Import from File"**
3. Select `My workflow.json`
4. Click **"Import"**
5. Workflow appears in your workspace

### Step 3: Get Google Gemini API Key

1. Visit https://ai.google.dev/
2. Click **"Get API key"**
3. Create new project or select existing
4. Click **"Create API Key"**
5. Copy and save the key

**Format:** `AIzaSyAxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

### Step 4: Get HeyGen API Key

1. Sign up at https://www.heygen.com/
2. Go to **"API"** section in dashboard
3. Copy your API key

**Format:** `MDU2NTExODlmZWJmNGY2MTk4ZjBhMjEzMjM1NGYyNGItMTc1OTY0NjA2Ng==`

**Note:** The key in the workflow is the actual HeyGen key from the JSON. Replace it with your own key!

---

## ⚙️ Configuration

### Update API Keys

You need to update API keys in **5 HTTP Request nodes**:

#### 1. HTTP Request20 (Gemini API)

Find this node and update the URL:

**Before:**
```
https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=AIzaSyAqNnJtQE888wIZHoTCYwP1pn0sZ3X2y_Q
```

**After:**
```
https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=YOUR_GEMINI_API_KEY
```

---

#### 2. HTTP Request2 (Fetch Avatars)

Find the node and update headers:

**Header Parameters:**
- Name: `X-Api-Key`
- Value: Replace with `YOUR_HEYGEN_API_KEY`

---

#### 3. HTTP Request16 (Fetch Voices)

**Header Parameters:**
- Name: `X-Api-Key`
- Value: Replace with `YOUR_HEYGEN_API_KEY`

---

#### 4. HTTP Request17 (Generate Video)

**Header Parameters:**
- Name: `x-api-key`
- Value: Replace with `YOUR_HEYGEN_API_KEY`

---

#### 5. HTTP Request18 (Poll Status)

**Header Parameters:**
- Name: `X-Api-Key`
- Value: Replace with `YOUR_HEYGEN_API_KEY`

---

### Customize RSS Feed

By default, workflow uses BBC News. To change:

1. Find **"RSS Read2"** node
2. Update URL parameter:

**Current:**
```
https://feeds.bbci.co.uk/news/rss.xml
```

**Other options:**
```
CNN: http://rss.cnn.com/rss/cnn_topstories.rss
Reuters: http://feeds.reuters.com/reuters/topNews
TechCrunch: https://techcrunch.com/feed/
```

---

### Adjust Headline Count

1. Find **"Code1"** node (Create Digest)
2. Modify `maxItems` value:

**Current:**
```javascript
const maxItems = 5;
```

**Change to:**
```javascript
const maxItems = 10; // Get 10 headlines instead
```

---

### Customize Video Settings

1. Find **"Edit Fields1"** node
2. Adjust values:

**Avatar ID:**
```javascript
"avatar_id": "9bb55566ca0242609aa1fea1f7144656"
```

**Voice ID:**
```javascript
"voice_id": "0912d8004fd948a9aab02fffd264a3c1"
```

**Video Dimensions:**
```javascript
"width": 1920,  // Change to 1920 for Full HD
"height": 1080  // Change to 1080 for Full HD
```

**Default Script** (for testing):
```javascript
"script": "Your custom test script here"
```

---

### Polling Configuration

The workflow uses smart polling with exponential backoff. To adjust:

1. Find **"Code15"** node (Initialize polling)
2. Modify default values:

**Current:**
```javascript
max_poll_seconds: 180,  // 3 minutes max
poll_interval_seconds: 6  // Start at 6 seconds
```

**For faster polling:**
```javascript
max_poll_seconds: 300,  // 5 minutes max
poll_interval_seconds: 4  // Start at 4 seconds
```

---

## 📖 Workflow Breakdown

### Stage 1: News Collection

**Nodes: Manual Trigger → RSS Read2 → Code1**

1. **Manual Trigger**: Click to start workflow
2. **RSS Read2**: Fetches BBC News RSS feed
3. **Code1 (Create Digest)**:
   ```javascript
   // Selects first 5 headlines
   const maxItems = 5;
   const selected = input.slice(0, maxItems);
   
   // Formats as bullet points
   const lines = selected.map(it => `• ${it.json.title}`);
   const digest = lines.join('\n');
   ```

**Output Example:**
```
• UK inflation rises to 2.6% amid energy costs
• Climate summit reaches historic agreement
• Tech giant announces new AI breakthrough
• Sports: Manchester United wins championship
• Markets surge on positive economic data
```

---

### Stage 2: Script Generation

**Nodes: HTTP Request20 → Code14**

1. **HTTP Request20 (Gemini API)**:
   - Sends news digest to Gemini 2.5 Flash
   - Model generates engaging script
   - Returns JSON response

2. **Code14 (Extract Script)**:
   ```javascript
   // Extract text from Gemini response
   const cands = items[0].json.candidates || [];
   const text = cands?.[0]?.content?.parts?.[0]?.text || "";
   
   // Safety: trim & hard cap at 1400 chars
   const script = text.trim().slice(0, 1400);
   
   if (!script) {
     throw new Error("Empty script from Gemini");
   }
   ```

**Output Example:**
```
"Good morning! Let's dive into today's top stories. 
Starting with the economy, UK inflation has climbed to 2.6%..."
```

---

### Stage 3: HeyGen Preparation

**Nodes: Use My Script1 → Edit Fields1 → HTTP Request2 & HTTP Request16**

1. **Use My Script1**: Assigns script to variable
2. **Edit Fields1**: Sets default configuration
3. **Parallel Execution**:
   - **HTTP Request2**: Fetches available avatars
   - **HTTP Request16**: Fetches available voices

---

### Stage 4: Avatar & Voice Selection

**Nodes: Pick Avatar1 → Pick Male Voice1**

**Pick Avatar1 (JavaScript)**:
```javascript
// Choose first non-premium avatar
const avatars = root?.data?.avatars || root?.avatars || [];
const chosen = avatars.find(a => a?.premium === false) || avatars[0];
const avatarId = chosen.avatar_id || 'Abigail_expressive_2024112501';
```

**Pick Male Voice1 (JavaScript)**:
```javascript
// Prefer explicit male gender
const voices = root?.data?.voices || root?.voices || [];
const preferred = voices.find(v => 
  String(v.gender || '').toLowerCase() === 'male'
);

// Fallback if no male voice found
const voiceId = preferred?.voice_id ?? '06f9223be7d444d887045ba89c1d00b6';
```

---

### Stage 5: Video Generation

**Node: HTTP Request17**

**API Request:**
```javascript
POST https://api.heygen.com/v2/video/generate

Headers:
- x-api-key: YOUR_HEYGEN_API_KEY
- Content-Type: application/json

Body:
{
  "video_inputs": [{
    "character": {
      "type": "avatar",
      "avatar_id": "{{ selected_avatar_id }}",
      "avatar_style": "normal"
    },
    "voice": {
      "type": "text",
      "input_text": "{{ script }}",
      "voice_id": "{{ selected_voice_id }}",
      "speed": 1.1
    }
  }],
  "dimension": {
    "width": 1280,
    "height": 720
  }
}
```

**Response:**
```json
{
  "data": {
    "video_id": "abc123xyz789"
  }
}
```

---

### Stage 6: Status Polling Loop

**Nodes: Code15 → HTTP Request18 → Switch2 → Code16 → Wait2**

**Flow:**

1. **Code15**: Extract video_id, initialize polling state
2. **HTTP Request18**: Check video status
3. **Switch2**: Route based on status
   - **completed** → Download video
   - **failed** → Throw error
   - **waiting/pending/processing** → Continue loop

4. **Code16**: Calculate backoff wait time
   ```javascript
   const prev = $json.attempts || 0;
   const base = $json.poll_interval_seconds || 6;
   
   // Exponential backoff: 6s, 8s, 10s, 12s, ..., max 30s
   const wait = Math.min(base + prev * 2, 30);
   
   // Check if exceeded max polling time
   if (elapsed >= 180) {
     throw new Error("Polling budget exceeded");
   }
   ```

5. **Wait2**: Pause for calculated duration
6. **Loop back** to HTTP Request18

---

### Stage 7: Video Download

**Nodes: If2 → HTTP Request19 → Edit Fields5**

1. **If2**: Verify video_url exists
2. **HTTP Request19**: Download video file
   - Response format: Binary (video file)
3. **Edit Fields5**: Format metadata
   ```javascript
   {
     "video_url": "https://...",
     "thumbnail_url": "https://...",
     "gif_url": "https://...",
     "job_id": "...",
     "status": "completed",
     "duration": "...",
     "message": "Video successfully generated"
   }
   ```

---

## 🎨 Customization

### Use Different News Sources

**Add Multiple RSS Feeds:**

1. Duplicate **RSS Read2** node
2. Update URL for each source:
   ```
   Source 1: BBC News
   Source 2: CNN
   Source 3: TechCrunch
   ```
3. Merge results before **Code1** node

---

### Customize Script Prompt

Add system prompt to Gemini for specific styles:

**In HTTP Request20 body:**
```javascript
{
  "contents": [{
    "parts": [{
      "text": "You are a professional news anchor. Create an engaging 60-second news script from these headlines. Use a conversational yet authoritative tone.\n\nHeadlines:\n" + $json.digest
    }]
  }]
}
```

**Prompt variations:**
- "Create a humorous take on these news stories"
- "Write in the style of a sports commentator"
- "Make it suitable for children aged 8-12"

---

### Select Specific Avatar

Instead of auto-selection:

**In Edit Fields1 node:**
```javascript
"avatar_id": "YOUR_SPECIFIC_AVATAR_ID"
```

**To find avatar IDs:**
1. Run workflow once
2. Check output of HTTP Request2
3. Browse `data.avatars` array
4. Copy desired `avatar_id`

---

### Choose Female Voice

**In Pick Male Voice1 node**, change logic:

```javascript
// Change from male to female
const preferred = voices.find(v => 
  String(v.gender || '').toLowerCase() === 'female'
);

// Fallback female voice
const voiceId = preferred?.voice_id ?? 'FEMALE_VOICE_ID';
```

---

### Add Background Music

This requires HeyGen Pro features. In video generation request:

```javascript
{
  "video_inputs": [...],
  "dimension": {...},
  "audio": {
    "type": "music",
    "url": "https://yourmusicurl.com/background.mp3",
    "volume": 0.3
  }
}
```

---

### Schedule Automatic Execution

Replace **Manual Trigger** with **Schedule Trigger**:

1. Delete "When clicking 'Execute workflow'" node
2. Add **"Schedule Trigger"** node
3. Configure schedule:
   ```
   Cron Expression: 0 8 * * *
   Meaning: Every day at 8:00 AM
   ```

**Other schedules:**
```
Every hour: 0 * * * *
Every 6 hours: 0 */6 * * *
Weekdays at 9 AM: 0 9 * * 1-5
```

---

## 🐛 Troubleshooting

### Issue: "Empty script from Gemini"

**Cause:** Gemini API returned no text

**Solutions:**
1. Check API key is valid
2. Verify API quota not exceeded
3. Check if digest was properly formatted
4. Test Gemini API directly
5. Review request body in HTTP Request20

---

### Issue: "HeyGen did not return video_id"

**Cause:** Video generation request failed

**Solutions:**
1. Verify HeyGen API key is correct
2. Check HeyGen account has credits
3. Verify avatar_id is valid
4. Confirm voice_id exists
5. Check script length (<1400 chars)
6. Review HTTP Request17 response

---

### Issue: "Polling budget exceeded"

**Cause:** Video took longer than 3 minutes

**Solutions:**
1. Increase `max_poll_seconds` in Code15:
   ```javascript
   max_poll_seconds: 300  // 5 minutes
   ```
2. Check HeyGen service status
3. Try shorter script
4. Verify account limits

---

### Issue: Video quality is poor

**Cause:** Resolution settings or HeyGen plan limits

**Solutions:**
1. Upgrade HeyGen plan for better quality
2. Increase dimensions in Edit Fields1:
   ```javascript
   "width": 1920,
   "height": 1080
   ```
3. Select premium avatars (may cost more)

---

### Issue: Wrong avatar selected

**Cause:** Auto-selection picking incorrect avatar

**Solutions:**
1. Hardcode specific avatar ID
2. Modify Pick Avatar1 logic:
   ```javascript
   // Prefer specific style
   const chosen = avatars.find(a => 
     a.avatar_style === 'professional'
   );
   ```
3. Review available avatars list

---

### Issue: Voice sounds robotic

**Cause:** Voice selection or speed settings

**Solutions:**
1. Try different voice_id
2. Adjust speech speed in HTTP Request17:
   ```javascript
   "speed": 1.0  // Slower, more natural
   ```
3. Select premium voices
4. Check voice preview on HeyGen

---

### Issue: RSS feed fails to load

**Cause:** Feed URL changed or network issue

**Solutions:**
1. Test RSS URL in browser
2. Check for redirects
3. Try alternative RSS feeds
4. Add error handling:
   ```javascript
   if (!items || items.length === 0) {
     throw new Error("No RSS items found");
   }
   ```

---

## 💡 Best Practices

### Cost Management

**HeyGen Credits:**
- Each video consumes credits
- Monitor credit usage in dashboard
- Consider bulk credit purchases
- Use non-premium avatars for testing

**Gemini API:**
- Free tier: 15 requests/minute
- Cache script results if re-using
- Monitor quota usage

**Cost per video (estimated):**
```
Gemini API: $0.00 (free tier)
HeyGen: $0.50 - $2.00 per video
Total: ~$0.50 - $2.00 per video
```

---

### Script Optimization

**Best practices:**
- Keep scripts under 60 seconds (~150 words)
- Use conversational language
- Avoid complex terms unless necessary
- Include pauses (commas, periods)
- Test different prompts

**Example good script:**
```
"Good morning! In today's news... [pause]
The UK economy shows mixed signals with inflation at 2.6%. 
Meanwhile, climate leaders reached a historic agreement..."
```

---

### Avatar Selection

**Guidelines:**
- Use professional avatars for news
- Match avatar ethnicity to audience
- Consider gender based on voice
- Test different avatars for engagement

**Popular avatar types:**
- News anchor style
- Business professional
- Casual presenter

---

### Performance Optimization

**Speed up workflow:**
1. Reduce headline count (5 → 3)
2. Shorter scripts generate faster
3. Use standard resolution (720p vs 1080p)
4. Consider parallel processing for multiple videos

---

### Error Recovery

**Add error handling:**

**After Gemini request:**
```javascript
// Retry logic
let attempts = 0;
while (attempts < 3) {
  try {
    // API call
    break;
  } catch (e) {
    attempts++;
    if (attempts === 3) throw e;
    // Wait and retry
  }
}
```

---

### Quality Assurance

**Before publishing videos:**
- Watch complete video
- Check audio sync
- Verify script accuracy
- Test on target platform
- Review thumbnail/preview

---

## 📊 Use Cases

### 1. Daily News Recaps

**Schedule:**
- Run every morning at 8 AM
- Generate 2-minute news summary
- Post to YouTube/social media

**Benefits:**
- Consistent content schedule
- No video editing required
- Professional presentation

---

### 2. Industry News Updates

**Example: Tech News**
- RSS: TechCrunch
- Script focus: Latest tech developments
- Avatar: Professional tech presenter

**Distribution:**
- LinkedIn posts
- Company newsletter
- Internal communications

---

### 3. Educational Content

**Example: Daily History Facts**
- RSS: History feed
- Script: Engaging historical narratives
- Avatar: Teacher/educator style

**Target:**
- Educational platforms
- Social media education accounts

---

### 4. Market Updates

**Example: Financial News**
- RSS: Bloomberg/Reuters
- Script: Market analysis summary
- Avatar: Business professional

**Audience:**
- Investors
- Business professionals
- Financial advisors

---

### 5. Social Media Content

**Quick video clips:**
- 30-second news highlights
- Shareable format
- Optimized for mobile

**Platforms:**
- Instagram Reels
- TikTok
- YouTube Shorts
- Twitter/X

---

## 📈 Performance Metrics

**Workflow Execution Time:**

| Stage | Duration |
|-------|----------|
| RSS Feed Fetch | 2-5 seconds |
| Script Generation | 3-8 seconds |
| Avatar/Voice Selection | 2-4 seconds |
| Video Generation Request | 1-2 seconds |
| Video Processing | 60-180 seconds |
| Video Download | 5-15 seconds |
| **Total** | **~2-5 minutes** |

**Resource Usage:**
- CPU: Low (mostly API calls)
- Memory: <100 MB
- Network: ~50-200 MB per video
- Storage: 10-50 MB per video

---

## 💰 Cost Analysis

### Monthly Costs (30 videos/month)

| Service | Cost |
|---------|------|
| **n8n** (self-hosted) | $0 or $20/mo (cloud) |
| **Google Gemini** | $0 (free tier) |
| **HeyGen** | $24-120/mo (plan-based) |
| **Total** | **$24-140/month** |

**Cost per video:** $0.80 - $4.67

---

## 🚀 Advanced Features

### Multi-Language Support

Generate videos in different languages:

**Modify Gemini prompt:**
```javascript
"Generate this news script in Spanish..."
```

**Select appropriate voice:**
- Filter voices by language in Pick Male Voice node

---

### A/B Testing

Create multiple versions:

1. Generate with different avatars
2. Try various script styles
3. Test speech speeds
4. Compare engagement metrics

---

### Batch Processing

Generate multiple videos:

1. Add loop node before RSS Read
2. Array of topics/sources
3. Process each sequentially
4. Save all videos

---

## 📄 License

This project is licensed under the **MIT License**.

✅ Commercial use allowed  
✅ Modification allowed  
✅ Distribution allowed  
✅ Private use allowed  
⚠️ No warranty or liability

---

## 🌟 Acknowledgments

Built with powerful tools:

- [n8n](https://n8n.io) - Workflow automation
- [HeyGen](https://heygen.com) - AI avatar platform
- [Google Gemini](https://ai.google.dev/) - Script generation AI
- [BBC News RSS](https://www.bbc.com/news) - News source

---

## 🎬 Ready to Create AI Videos!

**Start generating professional AI avatar videos automatically:**

1. ✅ Import workflow to n8n
2. ✅ Add API keys (Gemini & HeyGen)
3. ✅ Click execute
4. ✅ Wait 2-5 minutes
5. ✅ Download your video!

**Made with 🎬 for content creators everywhere**

⭐ Automate your video content creation!
🤖 Professional AI avatars at scale!
📺 High-quality videos without editing!

---
