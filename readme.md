# YouTube Mass Unsubscribe Script

A JavaScript browser console script that helps you unsubscribe from multiple YouTube channels quickly and efficiently.

## Description

This script automates the process of unsubscribing from YouTube channels when you're overwhelmed with too many subscriptions. It works directly in your browser's console and handles the entire unsubscription process including confirmation dialogs.

## Features

- Bulk unsubscribe from multiple channels
- Progress tracking in console
- Built-in delays to prevent rate limiting
- Handles confirmation dialogs automatically
- No external dependencies required
- Works directly in the browser console

## Prerequisites

- A modern web browser (Chrome, Firefox, Safari, or Edge)
- Access to your YouTube account
- Basic knowledge of using browser developer tools

## Installation

No installation required! This is a browser console script.

## Usage

1. Navigate to [YouTube Subscriptions Page](https://www.youtube.com/feed/channels)
2. Log in to your YouTube account if you haven't already
3. Open your browser's developer console:
   - Windows/Linux: Press `F12` or `Ctrl + Shift + I`
   - macOS: Press `Cmd + Option + I`
   - Or right-click anywhere on the page and select "Inspect" then click on "Console"
4. Copy and paste the following script into the console:

```javascript
// Function to unsubscribe from a channel
async function unsubscribeFromChannel(button) {
    // Click the subscribe button to open the menu
    button.click();
    
    // Wait for the unsubscribe option to appear
    await new Promise(resolve => setTimeout(resolve, 500));
    
    // Find and click the unsubscribe button in the popup menu
    const unsubButton = document.querySelector('ytd-menu-service-item-renderer');
    if (unsubButton) {
        unsubButton.click();
        
        // Wait for the confirmation dialog
        await new Promise(resolve => setTimeout(resolve, 500));
        
        // Click the confirm button
        const confirmButton = document.querySelector('[aria-label="Unsubscribe"]');
        if (confirmButton) {
            confirmButton.click();
        }
    }
}

// Main function to handle mass unsubscription
async function massUnsubscribe() {
    let processed = 0;
    const totalChannels = document.querySelectorAll('#subscribe-button').length;
    
    console.log(`Found ${totalChannels} channels to unsubscribe from.`);
    
    // Process each subscribe button
    for (const button of document.querySelectorAll('#subscribe-button')) {
        await unsubscribeFromChannel(button);
        processed++;
        
        // Add a delay between unsubscriptions to avoid overwhelming the system
        await new Promise(resolve => setTimeout(resolve, 2000));
        
        console.log(`Processed ${processed}/${totalChannels} channels`);
    }
    
    console.log('Unsubscribe process completed!');
}

// Start the unsubscription process
massUnsubscribe().catch(console.error);
```

5. Press Enter to run the script

## How It Works

The script performs the following actions:
1. Identifies all subscribe buttons on the current page
2. Clicks each subscribe button to open the menu
3. Finds and clicks the unsubscribe option
4. Handles the confirmation dialog
5. Waits for 2 seconds before processing the next channel
6. Provides progress updates in the console

## Safety Features

- Built-in delays prevent overwhelming YouTube's servers
- Progress tracking lets you monitor the unsubscription process
- The script can be stopped at any time by refreshing the page
- Runs entirely in your browser, no data is sent to external servers

## Limitations

- Only unsubscribes from channels visible on the current page
- Requires manual reload if you want to unsubscribe from more channels than are currently loaded
- Must be run on the YouTube channels feed page

## Troubleshooting

If you encounter issues:
1. Make sure you're on the correct page (youtube.com/feed/channels)
2. Verify that you're logged into your YouTube account
3. Check if your browser's console is throwing any errors
4. Try refreshing the page and running the script again
5. Ensure your browser is up to date

## Legal Note

This script is for personal use and automates actions that could be performed manually. Use it responsibly and in accordance with YouTube's terms of service.

## Contributing

Feel free to fork this repository and submit pull requests for any improvements you'd like to make.

## License

This project is licensed under the MIT License - feel free to use it for any purpose.