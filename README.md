# Discord AI Doubt Solver Bot

## Overview
This is a Discord bot designed to assist users by analyzing images and providing brief, accurate, and actionable solutions to their queries. It leverages OpenAI's advanced vision capabilities to understand image content and respond intelligently within specified Discord channels.

## Key Features
*   **Image Analysis:** Utilizes OpenAI's `gpt-4.1-mini` model to describe and understand images.
*   **Actionable Responses:** Configured to provide concise, accurate, and actionable steps to resolve issues presented in images.
*   **Flexible Input:** Processes both image attachments and image links (if enabled) within messages.
*   **Channel Specificity:** Can be configured to operate in specific Discord channels or across all channels.
*   **Customizable Prompts:** Allows customization of system prompts and starting messages for tailored bot behavior.
*   **Rate Limit Handling:** Includes mechanisms to handle Discord API rate limits for sending messages.

## Setup Instructions

### 1. Clone the Repository
```bash
git clone https://github.com/100x-Engineers100/discord-support-bot.git # Replace with your actual repo link
cd 100x-discord-support-bot
```

### 2. Create a Virtual Environment (Recommended)
```bash
python -m venv venv
source venv/bin/activate # On Windows: .\venv\Scripts\activate
```

### 3. Install Dependencies
Install all required Python packages using the `requirements.txt` file:
```bash
pip install -r requirements.txt
```

### 4. Environment Variables
The bot requires several environment variables for configuration. Create a `.env` file in the root directory of your project (or set them directly in your hosting environment) with the following:

```
DISCORD_BOT_TOKEN=YOUR_DISCORD_BOT_TOKEN
OPENAI_API_KEY=YOUR_OPENAI_API_KEY
CHANNEL_IDS=CHANNEL_ID_1,CHANNEL_ID_2 # Comma-separated list of channel IDs. Leave blank to respond in all channels.
STARTING_MESSAGE="Solve the issue in the attached image." # Default message for image analysis
SYSTEM_PROMPT="Provide brief, accurate responses in the form of actionable steps to solve the issue in user's attached image & query."
MAX_TOKENS=1000 # Max tokens for OpenAI response
MESSAGE_PREFIX="Image Description:" # Prefix for bot responses
REPLY_TO_LINKS=true # Set to 'true' to enable replying to image links, 'false' otherwise
```
**Important:** Never commit your `.env` file to version control. It is already included in `.gitignore`.

### 5. Run the Bot Locally
```bash
source venv/bin/activate # If not already active
python main.py
```

## Deployment

### Python Version
This project is configured to use **Python 3.12**.

### Option 1: Deploying on Render (Recommended)

Render is suitable for hosting this bot as a **Background Worker**.

1.  **Commit your code:** Ensure all your latest changes (including `main.py`, `requirements.txt`, and `Dockerfile`) are committed to your Git repository.
2.  **Create a new service on Render:**
    *   Go to your Render dashboard and create a new service.
    *   Connect your Git repository.
    *   **Crucially, select "Background Worker" as the service type.** (Do NOT select "Web Service" unless you intend to keep the Flask web server mimicry).
3.  **Configure Environment Variables:** Add all the environment variables listed in Step 4 (above) directly in the "Environment" section of your Render service settings.
4.  **Build Command:** Render should automatically detect your `Dockerfile` and build the image.
5.  **Start Command:** Render will use the `CMD ["python", "main.py"]` from your `Dockerfile`.
6.  **Deploy:** Trigger the deployment.

### Option 2: Deploying on Replit (Free Tier)

Replit offers a free tier suitable for hosting Discord bots, especially with an uptime monitor.

1.  **Create a new Python Repl:** On Replit, create a new Python project.
2.  **Import Codebase:** Import your project files (via Git clone or manual upload) into the root of your Replit project.
3.  **Install Dependencies:** Replit usually auto-installs from `requirements.txt`. If not, run `pip install -r requirements.txt` in the Shell.
4.  **Set Secrets:** Use Replit's "Secrets" tab (lock icon) to add all your environment variables (from Step 4) as secrets.
5.  **Configure Run Command:** Ensure your `.replit` file has `run = "python main.py"`.
6.  **Keep Alive (Free Tier):**
    *   **Add a simple web server to your bot:** Include the Flask web server code (as implemented in `main.py` for Render's web service mimicry) to your bot. This will make your Replit project accessible via a URL.
    *   **Use an external uptime monitor:** Sign up for a free service like [UptimeRobot](https://uptimerobot.com/). Create a new "HTTP(s)" monitor, paste your Replit project's webview URL, and set the monitoring interval (e.g., 5 minutes) to keep your bot active.

## Usage
Once the bot is running and connected to Discord:
*   Mention the bot or post an image in a configured channel.
*   The bot will analyze the image and respond with actionable steps based on the `SYSTEM_PROMPT` and `STARTING_MESSAGE`.
