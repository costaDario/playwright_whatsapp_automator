# WhatsApp Web Automation with Playwright

This project is a Python script designed to send automated WhatsApp messages to a list of contacts via WhatsApp Web. It uses the [Playwright](https://playwright.dev/) library for browser automation.

## Features

- Sends personalized messages to leads from a CSV file.
- Verifies if a contact has already received a specific message to avoid duplicates.
- Tracks statistics, such as the number of successfully sent and failed messages.
- Handles malformed rows in the CSV file gracefully.
- Adds random delays between messages to reduce the risk of detection by WhatsApp's anti-bot mechanisms.

---

## Requirements

### 1. Software Requirements

- Python 3.7 or newer
- A Google Chrome user profile logged into WhatsApp Web
- WhatsApp Web access on your browser
- A CSV file (`leads.csv`) with the contacts' details

### 2. Python Libraries

This script uses the following Python libraries:

- [Playwright](https://playwright.dev/): Browser automation
- [asyncio](https://docs.python.org/3/library/asyncio.html): Asynchronous programming
- Other standard Python modules (`csv`, `datetime`, `random`)

---

## Setup 

### 1. Install Dependencies

Install Playwright and other required dependencies using pip:

```bash
pip install playwright
playwright install
```

This will also install the browser binaries necessary for Playwright to operate.

### 2. Prepare the CSV File

Create a CSV file (`leads.csv`) with at least two columns:
- **Column 0**: The lead's full name (e.g., "John Doe")
- **Column 1**: The lead's phone number in international format (e.g., "1234567890")

**Example `leads.csv`:**
```csv
John Doe,1234567890
Jane Smith,9876543210
```

### 3. Configure the Script

Update the following constants in the script to match your use case:

- `HELLO`: The greeting part of the message (e.g., `"Hi "`).
- `MEET_AND_GREET`: Contextual intro after the greeting (e.g., `", hope I find you well\n\n"`).
- `BODY`: The main content of the message.
- `CHECK_STRING`: A unique phrase or keyword from the message to ensure the lead hasn’t already received it.
- `FILE_TO_OPEN`: The path to the CSV file (defaults to `"leads.csv"`).
- `user_data_dir`: Update the path to your Google Chrome user profile in the Playwright browser setup. You can find the Chrome profile in `~/.config/google-chrome` (Linux), `%LOCALAPPDATA%\Google\Chrome\User Data` (Windows), or `~/Library/Application Support/Google/Chrome` (macOS).

### 4. WhatsApp Web Login

Ensure that your Google Chrome profile is already logged into WhatsApp Web. If not, you will be prompted to scan the QR code for WhatsApp Web during the script execution.

---

## Usage

Run the script from the command line:

```bash
python whatsapp_playwright_automator.py
```

### Workflow:
1. The script will open WhatsApp Web in a Chromium browser.
2. You will be prompted to scan the QR code if not already logged in.
3. After successful login, the script will read `leads.csv` and begin sending messages to each contact.
4. Statistics and logs will be displayed in the console.

---

## Logs and Statistics

The script provides real-time logs for each operation:
- Successful deliveries
- Skipped messages (for leads who already received the message)
- Leads without WhatsApp accounts
- Malformed rows in the CSV file

At the end of execution, the script prints a summary:
- Total messages successfully sent
- Total failed messages
- Lists of leads without WhatsApp

---

## Example Output

Here's an example of the output you'll see in the terminal while running the script:

```
[2024-12-27 11:05:32] Scan WhatsApp Web QR Code to continue...
[2024-12-27 11:06:10] QR Code successfully scanned!
[2024-12-27 11:06:15] Successfully sent message to John.
[2024-12-27 11:06:18] Jane already received the message.
[2024-12-27 11:06:20] Number 9876543210 has no WhatsApp.
[2024-12-27 11:06:25] Successfully sent 1 messages
[2024-12-27 11:06:25] Failed to send 1 messages
```

---

## Notes & Limitations

1. **Anti-bot Detection**:
   - WhatsApp Web employs a bland anti-bot mechanisms. To reduce detection, I used random delays between successive message sends. It has been enough.

2. **Blocked Contacts**:
   - If a lead has blocked you, the message cannot be delivered. Such exception will be handled and an error will be logged.

3. **Duplicate Messages**:
   - In case the browser crashes, or connection is unstable, or maybe you are using an old slow computer, etc. the program may occasionally fail and you may be unsure about who received the message properly. You can edit `CHECK_STRING` and ensure it matches a unique part of the message (e.g., the URL you are sending to the lead, if any) to avoid sending duplicates during retries.

4. **Testing**:
   - Use a small list of test contacts before running the script on a large dataset.

---

## Troubleshooting

### Common Issues

1. **Browser Not Launching**:
   - Ensure Playwright is installed and the browser binaries are properly set up:
     ```bash
     pip install pytest-playwright
     playwright install
     ```

2. **CSV Formatting Errors**:
   - Ensure the `leads.csv` file has the correct format (name in column 0, phone number in column 1).

---

## License

This project is open-source and available under the MIT License.
