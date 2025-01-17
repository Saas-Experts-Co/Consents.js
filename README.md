# Consents Integration Guide

This guide covers the integration of Consents consent management system with your website or application. The integration uses termclicks.js to provide a customizable modal interface for collecting and managing user consents.

## Installation

Add the CSS file in your HTML head:

```html
<head>
    <link rel="stylesheet" href="https://consentsapp.com/termclicks.css">
</head>
```

Add the JavaScript file at the bottom of your page, just before the closing body tag to ensure all content is loaded first:

```html
<body>
    <!-- Your page content -->
    
    <script src="https://consentsapp.com/termclicks.js"></script>
    <script>
        // Your Consents initialization
    </script>
</body>
```

## Basic Setup

Initialize with your configuration:

```javascript
const consentOptions = {
    accountId: "YOUR_CONSENTS_ID",
    agreementTypeIds: ["terms-1", "privacy-1"], // Agreement type IDs as strings
    checkboxEl: "consent-checkbox"
};

const termClicks = new TermClicks(consentOptions);
```

## Required Configuration

| Option | Type | Description |
|--------|------|-------------|
| `accountId` | string | Your Consents account identifier |
| `agreementTypeIds` | string[] | Array of agreement type IDs from your Consents dashboard |
| `checkboxEl` | string | ID of consent checkbox element |

## User & Customer Identification

All user identification fields are optional. However, Consents needs either a userId or an email address to track consents.

### User Identification

If neither `userId` nor `userEmail` is provided (or `userEmailEl` isn't bound to an existing element), Consents will automatically display an email collection field in the consent modal.

```javascript
{
    // All fields are optional
    userId: "user-123",            // Unique identifier for the user
    userName: "John Doe",          // User's name
    userEmail: "john@example.com", // User's email address
    
    // OR use element IDs to pull values from your form
    userNameEl: "name-input-id",   // ID of element containing user's name
    userEmailEl: "email-input-id", // ID of element containing user's email
    
    // Optional additional data
    userCustomFields: "any-custom-data"
}
```

### Customer Identification

All customer fields are optional:

```javascript
{
    customerId: "customer-123",      // Customer identifier
    customerName: "Acme Corp",       // Customer name
    customerNameEl: "company-input", // ID of element containing customer name
    customerCustomFields: "any-custom-data"
}
```

## Important Implementation Notes

- You cannot use both direct values and element IDs for the same field (e.g., `userName` and `userNameEl` cannot be used together)
- If no user identification is provided, the modal will include an email input field with built-in validation
- When email collection is enabled in the modal, reCAPTCHA v3 is automatically integrated

## Configuration Options Reference

The absolute minimum configuration required is `accountId` and `agreementTypeIds`. All other options are optional and provide additional functionality and customization.

Note: All element selectors (`checkboxEl`, `userNameEl`, etc.) support any valid CSS selector, not just IDs. For example:
- IDs: `"#myCheckbox"`
- Classes: `".consent-checkbox"`
- Complex selectors: `"form.signup input[type='checkbox']"`

| Option | Type | Description | Default | Category |
|--------|------|-------------|----------|-----------|
| `accountId` | string | Your Consents account identifier | `""` | Core |
| `agreementTypeIds` | string[] | Array of agreement type IDs | `[]` | Core |
| `checkboxEl` | string | Selector for consent checkbox element | `""` | Element Selector |
| `userNameEl` | string | Selector for element containing user name | `""` | Element Selector |
| `userEmailEl` | string | Selector for element containing user email | `""` | Element Selector |
| `customerNameEl` | string | Selector for element containing customer name | `""` | Element Selector |
| `disableEls` | string[] | Selectors for elements to disable until consent given | `[]` | Element Selector |
| `userId` | string | Unique identifier for user | `""` | User Info |
| `userName` | string | User's name | `""` | User Info |
| `userEmail` | string | User's email address | `""` | User Info |
| `userCustomFields` | string | Additional user data | `""` | User Info |
| `customerId` | string | Customer identifier | `""` | Customer Info |
| `customerName` | string | Customer name | `""` | Customer Info |
| `customerCustomFields` | string | Additional customer data | `""` | Customer Info |
| `forceAgreementView` | boolean | Require viewing agreement before consent | `false` | Behavior |
| `forceScroll` | boolean | Require scrolling through content | `false` | Behavior |
| `requireConfirmation` | boolean | Require explicit confirmation | `false` | Behavior |
| `noSendConfirmation` | boolean | Skip sending confirmation | `false` | Behavior |
| `disableScreen` | boolean | Disable background while modal is open | `true` | Behavior |
| `autoUpdate.checkOutdated` | boolean | Check for updated agreements | `false` | Auto-Update |
| `autoUpdate.requireUpdatedConsent` | boolean | Force new consent for updates | `false` | Auto-Update |
| `autoUpdate.skipWarningDialog` | boolean | Skip update warning | `false` | Auto-Update |
| `defaultText.buttonAccept` | string | Accept button text | `"Accept"` | Text Customization |
| `defaultText.buttonDecline` | string | Decline button text | `"Decline"` | Text Customization |
| `defaultText.buttonOk` | string | OK button text | `"OK"` | Text Customization |
| `defaultText.buttonClose` | string | Close button text | `"Close"` | Text Customization |
| `defaultText.buttonContinue` | string | Continue button text | `"Continue"` | Text Customization |
| `defaultText.buttonDelay` | string | Delay button text | `"Delay"` | Text Customization |
| `defaultText.tooltipText` | string | Consent check tooltip | `"Checking consent..."` | Text Customization |
| `defaultText.alertTitleAlreadyAccepted` | string | Already accepted alert | `"Already Accepted"` | Text Customization |
| `defaultText.alertAgreementUpdatedTitle` | string | Update alert title | `"Agreement Updated"` | Text Customization |
| `defaultText.alertAgreementUpdated` | string | Update alert message | `"We have upgraded our terms and conditions. Please review and accept the new terms to continue."` | Text Customization |
| `defaultText.invalidEmailError` | string | Email validation error | `"Invalid email address"` | Text Customization |
| `defaultText.emailLabel` | string | Email input label | `"E-mail Required:"` | Text Customization |
| `defaultText.emailPlaceholder` | string | Email input placeholder | `"Enter your email"` | Text Customization |

## Common Implementation Scenarios

[Coming Soon - Documentation of common implementation patterns and use cases]

## CSS Customization

### Modal Structure

The modal interface uses the following class hierarchy:

```css
#termClicks-modal              /* Modal overlay */
└── .termClicks-card          /* Modal container */
    ├── .termClicks-card-header
    │   ├── h5                /* Title */
    │   └── .termClicks-closeButton
    ├── .termClicks-card-body
    │   └── #termClicks-scrollableContent
    └── .termClicks-card-footer
        ├── .termClicks-email-container   /* When email collection is enabled */
        ├── .termClicks-acceptButton
        ├── .termClicks-declineButton
        └── .termClicks-closeButton
```

### Customizing Styles

1. Add your custom CSS file after the default styles:

```html
<head>
    <link rel="stylesheet" href="https://consentsapp.com/termclicks.css">
    <link rel="stylesheet" href="your-custom-termclicks.css">
</head>
```

2. Override the default styles in your CSS file:

```css
/* Custom modal background */
#termClicks-modal {
    background: rgba(0, 0, 0, 0.7);
}

/* Custom modal size */
.termClicks-card {
    width: 600px;
    height: 80vh;
}

/* Custom button styles */
.termClicks-acceptButton {
    background-color: #4CAF50;
    border-radius: 8px;
    padding: 12px 24px;
}

/* Custom header */
.termClicks-card-header {
    background-color: #f8f9fa;
    border-bottom: 2px solid #dee2e6;
}
```

### Default Responsive Breakpoints

```css
/* Default (mobile) */
.termClicks-card {
    width: 90%;
}

/* Medium screens (768px+) */
@media (min-width: 768px) {
    .termClicks-card {
        width: 700px;
    }
}

/* Large screens (1200px+) */
@media (min-width: 1200px) {
    .termClicks-card {
        width: 750px;
    }
}
```

## Support

For implementation support or questions about your Consents account, please contact support@consents.com.
