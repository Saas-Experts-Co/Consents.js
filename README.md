# Consents Integration Guide

This guide covers the integration of Consents consent management system with your website or application. The integration uses termclicks.js to provide a customizable modal interface for collecting and managing user consents.

## Installation

Add the CSS file in your HTML head:

```
<head>
    <link rel="stylesheet" href="https://cdn.consentsapp.com/termclicks.css">
</head>
```

Add the JavaScript file at the bottom of your page, just before the closing body tag to ensure all content is loaded first:

```
<body>
    <!-- Your page content -->
    
    <script src="https://cdn.consentsapp.com/termclicks.js"></script>
    <script>
        // Your Consents initialization
    </script>
</body>
```

## Basic Setup

Before beginning, you must have your public account Id and the unique Id for each agreement type you want your users to consent to. These can be obtained from within your Consents.so account under **Settings \> Deploy**

To configure your Initialize with your configuration:

```javascript
const consentOptions = {
    accountId: "YOUR_CONSENTS_ID",
    agreementTypeIds: ["agreementId-1", "agreementId-2"], // Agreement type IDs as strings
    checkboxEl: "#consent-checkbox"
};

const termClicks = new TermClicks(consentOptions);
```

## Required Configuration

| Option | Type | Description |
| :---- | :---- | :---- |
| `accountId` | string | Your Consents account identifier |
| `agreementTypeIds` | string\[\] | Array of agreement type IDs from your Consents dashboard |
| `checkboxEl` | string | ID or selector of consent checkbox element |

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
    userNameEl: "#name-input",     // Selector for element containing user's name
    userEmailEl: "#email-input",   // Selector for element containing user's email
    
    // Optional additional data
    userCustomFields: "any-custom-data"
}
```

### Customer Identification

All customer fields are optional:

```javascript
{
    customerId: "customer-123",        // Customer identifier
    customerName: "Acme Corp",         // Customer name
    customerNameEl: "#company-input",  // Selector for element containing customer name
    customerCustomFields: "any-custom-data"
}
```

> ### Important Implementation Notes
>
> - You cannot use both direct values and element selectors for the same field (e.g., `userName` and `userNameEl` cannot be used together)  
> - If no user identification is provided, the modal will include an email input field with built-in validation  
> - When email collection is enabled in the modal, reCAPTCHA v3 is automatically integrated

## Configuration Options Reference

The absolute minimum configuration required is `accountId` and `agreementTypeIds`. All other options are optional and provide additional functionality and customization.

Note: All element selectors (`checkboxEl`, `userNameEl`, etc.) support any valid CSS selector, not just IDs. For example:

- IDs: `"#myCheckbox"`  
- Classes: `".consent-checkbox"`  
- Complex selectors: `"form.signup input[type='checkbox']"`

| Option | Type | Description | Default |
| :---- | :---- | :---- | :---- |
| `accountId` | string | Your Consents account identifier | `""` |
| `agreementTypeIds` | string\[\] | Array of agreement type IDs | `[]` |
| `checkboxEl` | string | Selector for consent checkbox input element. Typically this is your 'I Agree' checkbox | `""` |
| `agreementLinks` | Object\[\] | Array of objects specifying agreement links and their corresponding agreements. Each object should have `el` (selector string) and `ag` (agreement type ID) properties. The `el` property can be a single selector or comma-separated list of selectors. Example: `[{el: "#terms-link", ag: "terms-1"}]` | `[]` |
| `userNameEl` | string | Selector for input element containing user name | `""` |
| `userEmailEl` | string | Selector for input element containing user email | `""` |
| `customerNameEl` | string | Selector for input element containing customer name | `""` |
| `disableEls` | string\[\] | Array of selectors for elements to disable until consent given. Typically these are submit or accept buttons | `[]` |
| `userId` | string | Unique identifier for user | `""` |
| `userName` | string | User's name. If the user name is known, provide it here, and don't use userNameEl. Only used if userId or userEmail is provided | `""` |
| `userEmail` | string | User's email address. If the user e-mail is known, provide it here, and don't use userEmailEl | `""` |
| `userCustomFields` | string | Additional user data. This string must be a valid and parsable JSON object | `""` |
| `customerId` | string | Customer identifier | `""` |
| `customerName` | string | Customer name. Only used if customerId is provided | `""` |
| `customerCustomFields` | string | Additional customer data. This string must be a valid and parsable JSON object | `""` |
| `forceAgreementView` | boolean | Require viewing agreement before consent. Checking the checkboxEl will automatically launch the consent modal | `false` |
| `forceScroll` | boolean | Require scrolling through content. If enabled, the consent modal requires users to page all the way down to the bottom of the agreement before the Accept button is enabled | `false` |
| `requireConfirmation` | boolean | Require explicit confirmation. If enabled, the consent is marked as pending and the user is e-mailed a confirmation link to finalize their consent | `false` |
| `noSendConfirmation` | boolean | Skip sending confirmation. Only used if requireConfirmation is true. Enabling this option will still cause the consent to be marked as pending, but the confirmation link will not be sent. Useful if you want to manually send confirmation links via the Consents admin interface | `false` |
| `disableScreen` | boolean | Disable background while modal is open | `true` |
| `autoUpdate.checkOutdated` | boolean | Check for updated agreements. On initialization automatically check to ensure this user has all the latest version of their agreements up to date. If not, the consent modal is displayed | `false` |
| `autoUpdate.requireUpdatedConsent` | boolean | Force new consent for updates. Only used if autoUpdate.checkOutdated is true. Enabling this option prevents the user from closing the consents dialog until they accept the updated version of the consent | `false` |
| `autoUpdate.userId` | boolean | Push an updated userId to any Person record that may be missing it | `false` |
| `autoUpdate.skipWarningDialog` | boolean | Skip update warning and go directly to consents modal if autoUpdate.checkUpdated is true | `false` |
| `defaultText.buttonAccept` | string | Accept button text | `"Accept"` |
| `defaultText.buttonDecline` | string | Decline button text | `"Decline"` |
| `defaultText.buttonOk` | string | OK button text | `"OK"` |
| `defaultText.buttonClose` | string | Close button text | `"Close"` |
| `defaultText.buttonContinue` | string | Continue button text | `"Continue"` |
| `defaultText.buttonDelay` | string | Delay button text | `"Delay"` |
| `defaultText.tooltipText` | string | Consent check tooltip | `"Checking consent..."` |
| `defaultText.alertTitleAlreadyAccepted` | string | Already accepted alert | `"Already Accepted"` |
| `defaultText.alertAgreementUpdatedTitle` | string | Update alert title | `"Agreement Updated"` |
| `defaultText.alertAgreementUpdated` | string | Update alert message | `"We have upgraded our terms and conditions. Please review and accept the new terms to continue."` |
| `defaultText.invalidEmailError` | string | Email validation error | `"Invalid email address"` |
| `defaultText.emailLabel` | string | Email input label | `"E-mail Required:"` |
| `defaultText.emailPlaceholder` | string | Email input placeholder | `"Enter your email"` |

## Common Implementation Scenarios

### 1\. Anonymous User (Public Website)

If you want to deploy Consents to your public website, this scenario provides the ability to automatically collect visitor email addresses at the time of consent. The modal will handle email collection and validation automatically.

```
<form id="newsletter-form">
    <input type="checkbox" id="consent-checkbox">
    <label for="consent-checkbox">I agree to the terms</label>
</form>

<script>
const termClicks = new TermClicks({
    accountId: "YOUR_CONSENTS_ID",
    agreementTypeIds: ["terms-1"],
    checkboxEl: "#consent-checkbox",
    forceAgreementView: true,
    // Don't provide userId or userEmail - modal will collect email
});
</script>
```

### 2\. New User Registration or Shopping Cart Checkout

This scenario captures consent during registration or checkout. The "I agree" checkbox is typically near the final action button.  Since this consent is coming in unsolicited, it also requires them to confirm the request via e-mail.

When you already have the userId during registration:

```
<form id="checkout-form">
    <input type="email" id="user-email" required>
    <input type="checkbox" id="consent-checkbox">
</form>

<script>
const termClicks = new TermClicks({
    accountId: "YOUR_CONSENTS_ID",
    agreementTypeIds: ["terms-1", "privacy-1"],
    forceAgreementView: true,
    requireConfirmation: true,
    checkboxEl: "#consent-checkbox",
    disableEls: ["#checkout-btn"],
    userId: "user_123", // Your system's user ID
    userEmailEl: "#user-email"
});
</script>
```

If you don't have a userId at the time of consent (common in registration flows), first collect the consent with just the email:

```javascript
const termClicks = new TermClicks({
    accountId: "YOUR_CONSENTS_ID",
    agreementTypeIds: ["terms-1"],
    checkboxEl: "#consent-checkbox",
    disableEls: ["#checkout-btn"],
    userEmailEl: "#user-email"
});
```

Then, after the user account is created and you have the userId, ensure the following script with this minimal configuration is deployed with the known user Id:

```javascript
const termClicks = new TermClicks({
    accountId: "YOUR_CONSENTS_ID",
    agreementTypeIds: ["terms-1"],
    userId: "newly_created_user_123",
    userName: "John Doe",
    userCustomFields: "{'property':'address'}",
    customerId: "newly_created_customer_123",
    customerName: "Acme Holdings",
    customerCustomFields: "{'phone':'555-555-5555'}",
    userEmail: "user@example.com",
    autoUpdate: {
        userId: true
    }
});
```

> #### Creating Anonymous Users and Customers Records
>  
> Most registration flows collect user and customer information before you have generated your internal uniqueId.  In cases like this, refer to the above example and simply send the user’s e-mail address.    Some additional things to consider:  
>  
> - If you do not have a userId at this time you can **not** send company information.  The following options should not be set if userId is not provided:  
>   - customerName  
>   - customerNameEl  
>   - customerId  
>   - customerCustomFields  
>  
> - Once the consent record is provided with just the user’s e-mail, you can update the remaining user and customer fields by setting the autoUpdate.userId to **true** (see above example)  
> - You should only set the autoUpdate.userId to **true** when a user’s information has changed.  Otherwise Consents will attempt to update the custom record on every page load from within your application or website.  
> - autoUpdate.userId:true **only works** when the user hasn’t already been assigned a userId.  Any updates requested as a result of this option being set will only occur if Consents finds a user with the specified e-mail address who has **not** previously been assigned a userId.

### 3\. Multiple Agreement Consent

When users need to consent to multiple agreements (e.g., Terms of Service and Privacy Policy):

```
<form id="registration-form">
    <div class="agreements">
        <input type="checkbox" id="consent-checkbox">
        <label for="consent-checkbox">
            I agree to the 
            <a href="#" id="terms-link">Terms of Service</a> and 
            <a href="#" id="privacy-link">Privacy Policy</a>
        </label>
    </div>
    <button id="register-btn" type="submit">Create Account</button>
</form>

<script>
const termClicks = new TermClicks({
    accountId: "YOUR_CONSENTS_ID",
    agreementTypeIds: ["terms-1", "privacy-1"],
    checkboxEl: "#consent-checkbox",
    disableEls: ["#register-btn"],
    forceAgreementView: true,
    agreementLinks: [
        { el: "#terms-link", ag: "terms-1" },
        { el: "#privacy-link", ag: "privacy-1" }
    ]
});
</script>
```

### 4\. Multiple Agreement Links

Create multiple entry points to view agreements on your page:

```
<nav>
    <a href="#" id="nav-terms">Terms</a>
    <a href="#" id="nav-privacy">Privacy</a>
</nav>

<footer>
    <a href="#" id="footer-terms">Terms of Service</a>
    <a href="#" id="footer-privacy">Privacy Policy</a>
</footer>

<script>
const termClicks = new TermClicks({
    accountId: "YOUR_CONSENTS_ID",
    agreementTypeIds: ["terms-1", "privacy-1"],
    agreementLinks: [
        { el: "#nav-terms, #footer-terms", ag: "terms-1" },
        { el: "#nav-privacy, #footer-privacy", ag: "privacy-1" }
    ]
});
</script>
```

### 5\. Custom Fields Collection

Collect additional information with each consent record:

```javascript
const termClicks = new TermClicks({
    accountId: "YOUR_CONSENTS_ID",
    agreementTypeIds: ["terms-1"],
    checkboxEl: "#consent-checkbox",
    userId: "user_123",
    userCustomFields: '{"subscriptionTier":"premium","registrationSource":"webinar","marketingPreferences":["email","sms"]}',
    customerCustomFields: '{"accountType":"enterprise","industrySegment":"healthcare","contractId":"cont_789"}'
});
```

## CSS Customization

### Modal Structure

The modal interface uses the following class hierarchy:

```
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

```
<head>
    <link rel="stylesheet" href="https://cdn.consentsapp.com/termclicks.css">
    <link rel="stylesheet" href="your-custom-termclicks.css">
</head>
```

2. Override the default styles in your CSS file:

```
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

```
/* Default (mobile) */
.termClicks-card {
    width: 90%;
}

/*
```
