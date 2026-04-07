---
name: browser-automation
description: Browser automation for social media management using Playwright. Use when user wants to automate browser tasks like posting content, uploading videos, scraping data, or interacting with web interfaces. Supports Instagram, TikTok, Facebook, LinkedIn, and other platforms.
---

# Browser Automation Skill

## Overview

Use Playwright (Python) for browser automation of social media tasks. This skill enables:
- Automated posting to social platforms
- Video/image uploading
- Data scraping
- Content monitoring
- Account management

## Setup

### Environment
```bash
# Install Playwright (already done on server)
python3 -m pip install playwright --break-system-packages

# Install Chromium (already done)
python3 -m playwright install chromium

# Install dependencies (already done)
python3 -m playwright install-deps chromium
```

### Browser Launch Options
```python
from playwright.sync_api import sync_playwright

# Headless mode (no visible browser)
with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    context = browser.new_context(
        user_agent='Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36',
        viewport={'width': 1920, 'height': 1080}
    )
    page = context.new_page()
```

### Authentication with Cookies
```python
# Load cookies from file (exported from browser)
import json

with open('cookies.json', 'r') as f:
    cookies = json.load(f)

context = browser.new_context()
context.add_cookies(cookies)
```

## Platform-Specific Automation

### Instagram (Web)

**Login/Setup:**
1. Install "Get cookies.txt LOCALLY" Chrome extension
2. Login to Instagram
3. Export cookies to `cookies/instagram_cookies.json`

**Post Image:**
```python
def post_instagram(image_path, caption, cookies_path='cookies/instagram_cookies.json'):
    with sync_playwright() as p:
        browser = p.chromium.launch(headless=False)
        context = browser.new_context()
        
        # Load cookies
        with open(cookies_path) as f:
            context.add_cookies(json.load(f))
        
        page = context.new_page()
        page.goto('https://www.instagram.com/')
        
        # Create new post
        page.click('svg[aria-label="New post"]')
        page.wait_for_timeout(2000)
        
        # Upload image
        page.set_input_files('input[type="file"]', image_path)
        page.wait_for_timeout(3000)
        
        # Next
        page.click('button:has-text("Next")')
        page.wait_for_timeout(1000)
        page.click('button:has-text("Next")')
        page.wait_for_timeout(1000)
        
        # Add caption
        page.fill('textarea[aria-label="Write a caption..."]', caption)
        
        # Share
        page.click('button:has-text("Share")')
        page.wait_for_timeout(5000)
        
        browser.close()
```

### TikTok

**Using tiktok-uploader (already installed):**
```python
from tiktok_uploader.upload import TikTokUploader

with TikTokUploader(cookies='cookies/tiktok_cookies.txt') as uploader:
    uploader.upload_video(
        filename='video.mp4',
        description='Check this out! #fyp #viral',
        visibility='everyone'
    )
```

### Facebook (Page Post)

**Post to Page:**
```python
def post_facebook_page(text, cookies_path='cookies/facebook_cookies.json'):
    with sync_playwright() as p:
        browser = p.chromium.launch(headless=False)
        context = browser.new_context()
        
        with open(cookies_path) as f:
            context.add_cookies(json.load(f))
        
        page = context.new_page()
        page.goto('https://www.facebook.com/')
        
        # Find your page
        page.goto('https://www.facebook.com/YOUR_PAGE_NAME/')
        
        # Create post
        page.click('div[aria-label="Create a post"]')
        page.wait_for_timeout(1000)
        
        # Write post
        page.fill('div[aria-label="What\'s on your mind?"]', text)
        page.wait_for_timeout(500)
        
        # Post
        page.click('button:has-text("Publish")')
        page.wait_for_timeout(3000)
        
        browser.close()
```

### LinkedIn

**Post Update:**
```python
def post_linkedin(text, cookies_path='cookies/linkedin_cookies.json'):
    with sync_playwright() as p:
        browser = p.chromium.launch(headless=False)
        context = browser.new_context()
        
        with open(cookies_path) as f:
            context.add_cookies(json.load(f))
        
        page = context.new_page()
        page.goto('https://www.linkedin.com/feed/')
        
        # Start post
        page.click('button:has-text("Start a post")')
        page.wait_for_timeout(1000)
        
        # Write content
        page.fill('div[role="textbox"]', text)
        page.wait_for_timeout(500)
        
        # Post
        page.click('button:has-text("Post")')
        page.wait_for_timeout(3000)
        
        browser.close()
```

### YouTube Studio

**Upload Video:**
```python
def upload_youtube(video_path, title, description, cookies_path='cookies/youtube_cookies.json'):
    with sync_playwright() as p:
        browser = p.chromium.launch(headless=False)
        context = browser.new_context()
        
        with open(cookies_path) as f:
            context.add_cookies(json.load(f))
        
        page = context.new_page()
        page.goto('https://studio.youtube.com/')
        
        # Upload
        page.click('button#create-icon')
        page.wait_for_timeout(500)
        page.click('text=Upload videos')
        page.wait_for_timeout(1000)
        
        # Select file
        page.set_input_files('input#file-loader', video_path)
        page.wait_for_timeout(5000)
        
        # Title
        page.fill('#title-textarea', title)
        
        # Description
        page.fill('#description-textarea', description)
        
        # Publish
        page.click('button#done-button')
        page.wait_for_timeout(2000)
        
        browser.close()
```

## Cookie Export Guide

### Chrome Extension
Use "Get cookies.txt LOCALLY" by coolbuy:
1. Install extension from Chrome Web Store
2. Login to target website
3. Click extension → Export → Copy as JSON or Netscape format

### Cookie File Format
```json
[
  {
    "name": "sessionid",
    "value": "abc123...",
    "domain": ".instagram.com",
    "path": "/"
  }
]
```

## Error Handling

```python
try:
    # Your automation code
    pass
except Exception as e:
    print(f"Error: {e}")
    # Take screenshot for debugging
    page.screenshot(path='error.png')
    # Save cookies for retry
    with open('cookies/backup.json', 'w') as f:
        json.dump(context.cookies(), f)
```

## Rate Limiting

| Platform | Wait Between Actions |
|----------|---------------------|
| Instagram | 30-60 seconds |
| TikTok | 60 seconds |
| Facebook | 30 seconds |
| LinkedIn | 60 seconds |
| YouTube | 30 seconds |

## Best Practices

1. **Always use cookies** - Don't login with credentials directly
2. **Add delays** - Use `page.wait_for_timeout()` between actions
3. **Handle errors** - Try/catch with screenshot on failure
4. **Rotate accounts** - Don't post too frequently from one account
5. **Backup cookies** - Save cookies before each session

## Testing

```python
# Quick test
python3 -c "
from playwright.sync_api import sync_playwright
with sync_playwright() as p:
    browser = p.chromium.launch(headless=True)
    page = browser.new_page()
    page.goto('https://www.google.com')
    print(page.title())
    browser.close()
"
```

---

## Commands/Triggers

- "automate [platform]"
- "post to Instagram"
- "upload TikTok"
- "scrape [platform]"
- "browser automation"
