# Veracross Calendar Sync

> Automated Google Apps Script that fetches 8-day rotation schedules from Veracross and publishes them as iCal calendars via GitHub Pages.

## Features

- **🔄 Automated Data Sync** - Connects to Veracross API to retrieve rotation day data
- **📅 iCal Compatible** - Generates standard calendar format for universal device compatibility
- **🌏 Timezone Optimized** - Built for Asia/Seoul timezone with all-day events
- **📱 Multi-Device Support** - Subscribe once, sync across all devices
- **⚙️ Smart Processing** - Applies override rules for special schedules and Day 0 events
- **🚀 GitHub Pages Integration** - Automatically publishes calendar updates
- **⏰ Scheduled Updates** - Configurable automation (daily/weekly)

## Quick Start

### Calendar Subscription
Add this URL to your calendar app:
```
https://songdo-technology.github.io/8-day-rotation.ics
```

**Subscription Instructions:**
- **iOS/macOS**: Settings → Calendar → Accounts → Add Account → Other → Add Subscribed Calendar
- **Google Calendar**: Settings → Add calendar → From URL
- **Outlook**: Home → Add Calendar → Subscribe from web

## System Architecture

```
Veracross API → Google Apps Script → Data Processing → GitHub Pages → iCal Subscription
```

The system automatically:
1. Fetches rotation data from Veracross
2. Processes and applies override rules
3. Converts to iCal format
4. Publishes to GitHub Pages
5. Notifies subscribers of updates

## Installation

### Prerequisites

Before setting up the script, ensure you have:

**Veracross API Access**
  - School route: `chadwickinternational`
  - OAuth client credentials
  - API permissions for `academics.calendar_rotation_days:list`

**GitHub Repository** 
  - Repository: `songdo-technology/songdo-technology.github.io`
  - Personal Access Token with repository write permissions
  - GitHub Pages enabled

**Google Apps Script Environment**
  - Google account with Apps Script access
  - Google Sheets document for the script

### Setup Instructions

1. **Create Google Apps Script Project**
   ```bash
   # Go to script.google.com and create a new project
   # Copy the provided code into the script editor
   ```

2. **Configure Script Properties**
   
   Navigate to Project Settings → Script Properties and add:
   
   ```
   VERACROSS_SCHOOL_ROUTE     = chadwickinternational
   VERACROSS_CLIENT_ID        = your_oauth_client_id
   VERACROSS_CLIENT_SECRET    = your_oauth_client_secret
   GITHUB_TOKEN              = your_github_personal_access_token
   GITHUB_REPO               = songdo-technology/songdo-technology.github.io
   ```

3. **Set Academic Year**
   ```javascript
   // Run this function in the Apps Script editor
   setSchoolYear() // Will prompt for 4-digit year (e.g., "2025")
   ```

4. **Test Configuration**
   ```javascript
   // Verify all settings are correct
   testSetup()
   ```

5. **Initial Calendar Publish**
   ```javascript
   // Publish calendar for the first time
   publishCalendarToGitHub()
   ```

6. **Setup Automation** *(Optional)*
   ```javascript
   // Enable daily updates at 6 AM Korea time
   setupAutoUpdate()
   ```

## Usage

### Script Menu Options

The script adds a "Veracross Calendar" menu to your Google Sheets with these options:

| Menu Item | Description |
|-----------|-------------|
| **Fetch Data to Sheet** | Import rotation data to spreadsheet for review |
| **Publish to GitHub** | Manually update the calendar |
| **Setup Auto-Update** | Enable daily automation |
| **Set School Year** | Configure academic year |
| **Test Setup** | Validate all configurations |

### Manual Operations

```javascript
// View data in Google Sheets (optional)
fetchDataToSheet()

// One-time manual calendar update  
publishCalendarToGitHub()

// Enable daily automation
setupAutoUpdate()
```

## Data Processing Logic

The script intelligently processes rotation data with these override rules:

### Standard Processing
```
Input:  Day 3 + <Default Schedule>
Output: "Day 3"
```

### Day 0 Override
```
Input:  Day 1 + "Day 0 - Assembly"  
Output: "Day 0 - Assembly"
```

### Thursday Pattern Conversion
```
Input:  Day 2 + "Thursday 7"
Output: "Day 7"
```

### Processing Examples

| Original Day | Block Schedule | Final Output |
|--------------|----------------|--------------|
| Day 3 | `<Default>` | Day 3 |
| Day 1 | Day 0 - First Day of School | Day 0 - First Day of School |
| Day 4 | Thursday 5 | Day 5 |
| Day 2 | Day 0 - Assembly | Day 0 - Assembly |

## Output Files

The script generates these files in your GitHub repository:

- **`8-day-rotation.ics`** - The iCal calendar file for subscription
- **`index.html`** *(updated)* - Landing page with subscription information and timestamps

## Troubleshooting

### Common Issues

**❌ Authentication Failures**
- Verify Veracross credentials in Script Properties
- Check OAuth client has proper permissions
- Confirm school route identifier is correct

**❌ GitHub Publishing Issues** 
- Ensure GitHub token has repository write access
- Verify repository format: `username/repository-name`
- Confirm GitHub Pages is enabled

**❌ No Data Retrieved**
- Check if school year is set correctly via `setSchoolYear()`
- Verify API permissions for calendar data access
- Ensure "8-day cycle" rotation type exists in Veracross

**❌ Calendar Not Updating**
- Check Apps Script execution logs for errors
- Verify automation trigger is active
- Test manual publish with `publishCalendarToGitHub()`

### Debugging Steps

1. **Check Logs**: View execution history in Apps Script editor
2. **Test Configuration**: Run `testSetup()` to validate all settings
3. **Review Data**: Use `fetchDataToSheet()` to examine raw Veracross data
4. **Step-by-Step Testing**: Execute functions individually to isolate issues

## Customization

### Timezone Configuration
```javascript
// In convertToICal() function, change timezone:
'X-WR-TIMEZONE:America/New_York\r\n'  // Change from Asia/Seoul
```

### Calendar Properties
```javascript
// Customize calendar name and description:
'X-WR-CALNAME:Your School Rotation Days\r\n' +
'X-WR-CALDESC:Custom description here\r\n'
```

### Update Schedule
```javascript
// Modify automation timing:
ScriptApp.newTrigger('publishCalendarToGitHub')
  .timeBased()
  .everyDays(1)
  .atHour(8)  // Change from 6 AM to 8 AM
  .inTimezone('America/New_York')  // Change timezone
  .create();
```

### Custom Override Rules
```javascript
// Add new processing patterns in processRotationData():
else if (blockSchedule.includes('Special Event')) {
  // Your custom processing logic
  processedDay = blockSchedule;
}
```

## Maintenance

### Annual Tasks
- [ ] Update school year using `setSchoolYear()`
- [ ] Test complete workflow after year change
- [ ] Verify override rules still apply correctly
- [ ] Update any custom processing logic if needed

### Regular Monitoring  
- [ ] Check daily execution logs
- [ ] Monitor GitHub Pages deployment status
- [ ] Verify calendar subscription is working
- [ ] Rotate OAuth credentials periodically

## Security Best Practices

- ✅ Store credentials only in Script Properties (encrypted)
- ✅ Use minimal required permissions for GitHub token
- ✅ Regularly rotate OAuth credentials  
- ✅ Monitor script execution logs for suspicious activity
- ✅ Limit repository access to necessary users only

## API Reference

### Core Functions

| Function | Description |
|----------|-------------|
| `getConfig()` | Retrieves and validates all configuration |
| `setSchoolYear()` | Interactive academic year configuration |
| `getAccessToken(config)` | Authenticates with Veracross OAuth |
| `getRotationDays(token, config)` | Fetches rotation data from API |
| `processRotationData(rawData)` | Applies override rules and filtering |
| `convertToICal(rotationDays)` | Generates RFC-compliant iCal format |
| `publishCalendarToGitHub()` | Main publishing workflow |

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For issues and questions:
- 📧 Contact your school's IT department
- 🐛 [Report bugs](https://github.com/songdo-technology/songdo-technology.github.io/issues)
- 📖 Check the troubleshooting section above

---

**Built with ❤️ for Chadwick International School**
