```markdown
# Telegram Downloader Bot

## Features
- Download video, music, photo, GIF, document from YouTube, Instagram, Twitter, TikTok, Facebook, and 1000+ sites
- Subscription system (1, 2, 3, 6, 12, 24 months)
- 3 free downloads for new users
- Auto quality: 360p (free user), 720p (HD for subscribers 1-5 months), 1080p (Full HD for subscribers 6+ months)
- Force channel join (multiple channels supported)
- Admin panel: statistics, broadcast message, pending receipts, user search, ban/unban, force channel management
- Download queue with concurrent download limit
- Payment system: user sends receipt photo, admin approves with months
- SMS alert for internet disconnection (optional)
- Works on Linux, Windows, Termux (Android)

## Installation

### Linux Server (Ubuntu/Debian)
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install python3 python3-pip ffmpeg git -y
git clone https://github.com/Ahmad-Cyber-prince/Telegram-Downloader-Bot.git
cd Telegram-Downloader-Bot
pip3 install -r requirements.txt
```

### Windows
```bash
1. Install Python 3.8+ from python.org
2. Install ffmpeg and add to PATH
3. Open CMD as administrator
git clone https://github.com/Ahmad-Cyber-prince/Telegram-Downloader-Bot.git
cd Telegram-Downloader-Bot
pip install -r requirements.txt
```

### Termux (Android)
```bash
pkg update && pkg upgrade
pkg install python ffmpeg git -y
git clone https://github.com/Ahmad-Cyber-prince/Telegram-Downloader-Bot.git
cd Telegram-Downloader-Bot
pip install -r requirements.txt
```

## Configuration

Open `BOT` file with any text editor and edit these lines:

### Required Settings
```python
TOKEN = "1234567890:AAFhFakeToken_YourRealTokenFromBotFather"
ADMIN_ID = 123456789
```

### Payment Settings (shown to user)
```python
CARD_NUMBER = "6037-XXXX-XXXX-XXXX"
CARD_HOLDER = "John Doe"
MONTHLY_PRICE = 50000
```

### Optional SMS Alert (for internet disconnection)
```python
SMS_API_KEY = "your_sms_ir_api_key"
SMS_LINE = "3000XXXX"
ADMIN_PHONE = "09123456789"
```

### Advanced Settings (adjust as needed)
```python
FREE_DOWNLOADS = 3
MAX_FILE_SIZE_MB = 45
MAX_MONTHS = 24
MAX_CONCURRENT = 2
```

### Get Admin ID
Send `/start` to @userinfobot on Telegram to get your numeric ID.

## Create Bot Token
1. Open Telegram
2. Search for @BotFather
3. Send `/newbot`
4. Choose name and username
5. Copy the token (looks like: `1234567890:AAFhFakeToken`)

## Run Bot

### Linux / Termux
```bash
python3 BOT
```

### Windows
```bash
python BOT
```

### Keep bot running after closing terminal (Linux)
```bash
nohup python3 BOT &
```

### Stop bot
Press `Ctrl + C` or `pkill -f "python3 BOT"`

## Bot Commands

| Command | Description | Access |
|---------|-------------|--------|
| `/start` | Start bot and show main menu | All users |
| `/panel` | Open admin control panel | Admin only |

## User Panel Buttons

| Button | Function |
|--------|----------|
| 💰 خرید اشتراک | Buy subscription (select months, send receipt photo) |
| 📊 وضعیت حساب | Show account status (free downloads left, subscription expiry) |
| 📞 پشتیبانی | Contact admin directly |

## Admin Panel Buttons

| Button | Function |
|--------|----------|
| 📊 آمار | Show bot statistics (users, channels, active downloads, queue) |
| 📨 پیام همگانی | Send broadcast message to all users |
| 📋 رسیدها | View pending payment receipts |
| 👤 جستجو | Search user by ID |
| 🚫 مسدود/آزاد | Ban or unban user |
| ⚙️ عضویت اجباری | Manage force join channels |

## Force Channel Management

### Add channel
Click `➕ افزودن` and send:
```
@channelusername|Channel Name|https://t.me/invite_link
```

### Remove channel
Click `➖ حذف` and send channel ID (e.g., `@channelusername`)

### List channels
Click `📋 لیست` to see all active channels

## Payment System Flow

1. User clicks `💰 خرید اشتراک`
2. User selects months (1, 2, 3, 6, 12, 24)
3. Bot shows card number and amount
4. User sends receipt photo
5. Admin receives notification with Approve/Reject buttons
6. Admin clicks Approve and sends number of months
7. User receives confirmation and subscription is activated

## Database Structure

File: `robot_data.db` (auto-created)

### users table
| Column | Type | Description |
|--------|------|-------------|
| user_id | INTEGER | Primary key |
| username | TEXT | Telegram username |
| first_name | TEXT | User's first name |
| join_date | TEXT | First start date |
| free_downloads | INTEGER | Remaining free downloads |
| subscription_expire | TEXT | Subscription expiry date |
| total_downloads | INTEGER | Total downloads count |
| is_banned | INTEGER | 0=active, 1=banned |

### payments table
| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Primary key |
| user_id | INTEGER | User ID |
| months | INTEGER | Months purchased |
| amount | INTEGER | Amount paid |
| receipt_file_id | TEXT | Telegram file ID |
| status | TEXT | pending/approved/rejected |
| date | TEXT | Receipt date |

### force_channels table
| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Primary key |
| channel_id | TEXT | @username or ID |
| channel_name | TEXT | Display name |
| invite_link | TEXT | Invite link |
| is_active | INTEGER | 1=active, 0=inactive |

## Download Settings

- Maximum concurrent downloads: `MAX_CONCURRENT` (default 2)
- Maximum file size: `MAX_FILE_SIZE_MB` (default 45MB)
- Download folder: `./downloads` (auto-created, auto-cleaned)
- Supported formats: mp4, mkv, webm, mov, avi, mp3, m4a, wav, flac, ogg, jpg, jpeg, png, webp, gif, bmp, tiff

## Quality System

| User Type | Quality | Label |
|-----------|---------|-------|
| Free user (no subscription) | 360p | 🆓 پایه |
| Subscriber (1-5 months) | 720p | ⭐ HD |
| Subscriber (6+ months) | 1080p | 👑 Full HD |

## Troubleshooting

### Bot doesn't start
- Check if TOKEN is correct
- Check if ADMIN_ID is numeric
- Run `pip install -r requirements.txt` again

### ffmpeg error
```bash
# Linux
sudo apt install ffmpeg

# Termux
pkg install ffmpeg

# Windows
Download from ffmpeg.org and add to PATH
```

### Download fails
- Check internet connection
- URL might be blocked in your country
- File might be larger than `MAX_FILE_SIZE_MB`

### Permission denied
```bash
chmod +x BOT
```

## Requirements File

Create `requirements.txt`:
```
python-telegram-bot>=20.0
yt-dlp>=2023.12.30
requests>=2.31.0
```

## File Structure After Installation
```
Telegram-Downloader-Bot/
├── BOT
├── requirements.txt
├── README.md
├── robot_data.db
└── downloads/
```

## Uninstall
```bash
cd ..
rm -rf Telegram-Downloader-Bot
pip uninstall python-telegram-bot yt-dlp requests -y
```

## Contact

- Telegram: @Ahmad_tel_bot
- GitHub: https://github.com/Ahmad-Cyber-prince

## License

MIT License - Free to use, modify, and distribute
```
