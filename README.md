Here's a complete `README.md` file for your public repository, tailored to include all the details from the knowledge base and structured to make it accessible for developers:

```markdown
# TermClicks.js

A powerful and customizable JavaScript library to manage user consent for online agreements, including terms of service and privacy policies. Designed for flexibility, compliance, and ease of use.

---

## Features

- **Customizable Options**: Configure user consent behavior to suit your needs.
- **Auto-Update Support**: Automatically check for and handle updated agreements.
- **Multiple Consent Modes**: Supports manual consent, auto-accept, and checkbox integration.
- **User and Customer Tracking**: Easily track consent by users and customers with custom fields.
- **Seamless Integration**: Embed consent modals and workflows directly into your app.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Implementation](#implementation)
3. [Consent Modes](#consent-modes)
4. [Configuration Options](#configuration-options)
5. [Examples](#examples)
6. [License](#license)

---

## Getting Started

### Installation

Include the `termclicks.js` file in your project:
```html
<script src="https://consentsapp.com/termclicks.js"></script>
```

### Initialization

Define your configuration options and initialize the `TermClicks` object:
```javascript
var consentOptions = {
    accountId: "123456",
    agreementTypeIds: [1, 2],
    checkboxEl: "consentCheckbox",
    disableEls: ["submitButton"],
    forceAgreementView: true,
};
var termClicksInstance = new TermClicks(consentOptions);
```

---

## Implementation

### Manual Consent
Manually present modals to users for consent:
```javascript
termClicksInstance.show(agreementTypeId).then((result) => {
    if (result) {
        console.log("User manually accepted the agreement.");
    }
});
```

### Auto-Accept
Automatically accept all agreements programmatically:
```javascript
termClicksInstance.autoAccept().then(() => {
    console.log("All agreements were automatically accepted.");
});
```

### Checkbox Integration
Tie consent to a checkbox in your form:
```javascript
var consentOptions = {
    checkboxEl: "consentCheckbox",
    disableEls: ["submitButton"],
    forceAgreementView: true,
};
var termClicksInstance = new TermClicks(consentOptions);
```

---

## Consent Modes

| **Mode**             | **Description**                                                                             | **Best Use Case**                                    |
|-----------------------|---------------------------------------------------------------------------------------------|-----------------------------------------------------|
| **Manual Consent**    | Presents modal for explicit user review and acknowledgment.                                | For legal or compliance-sensitive workflows.        |
| **Auto-Accept**       | Automatically accepts agreements without user interaction.                                 | Internal systems or testing environments.           |
| **Checkbox Integration** | Links user consent to a form checkbox, enabling further actions only after consent.       | Traditional form-based consent workflows.           |

---

## Configuration Options

Here is a complete list of available configuration options:

### Basic Configuration
| **Option**           | **Type**      | **Description**                                                                 | **Default** |
|-----------------------|---------------|---------------------------------------------------------------------------------|-------------|
| `accountId`          | `string`     | Unique identifier for your account.                                            | `""`        |
| `agreementTypeIds`   | `Array<int>` | List of agreement IDs to track.                                                | `[]`        |
| `checkboxEl`         | `string`     | ID of the checkbox element used for consent.                                   | `""`        |
| `disableEls`         | `Array<string>` | List of element IDs to disable until consent is given.                         | `[]`        |

### Behavioral Controls
| **Option**             | **Type**      | **Description**                                                                | **Default** |
|-------------------------|---------------|--------------------------------------------------------------------------------|-------------|
| `forceAgreementView`   | `boolean`    | Requires users to view agreements before consenting.                          | `false`     |
| `forceScroll`          | `boolean`    | Requires users to scroll through the agreement content before accepting.      | `false`     |
| `disableScreen`        | `boolean`    | Disables interaction with other elements while the modal is displayed.        | `true`      |

### User Information
| **Option**             | **Type**      | **Description**                                                                | **Default** |
|-------------------------|---------------|--------------------------------------------------------------------------------|-------------|
| `userId`               | `string`     | User's unique ID.                                                             | `""`        |
| `userNameEl`           | `string`     | ID of the element containing the user's name.                                 | `""`        |

### Auto-Update
| **Option**                   | **Type**      | **Description**                                                                | **Default** |
|-------------------------------|---------------|--------------------------------------------------------------------------------|-------------|
| `autoUpdate.checkOutdated`   | `boolean`    | Checks for outdated agreements and prompts for re-consent.                    | `false`     |

### Default Text
| **Option**                     | **Type**      | **Description**                                                                | **Default** |
|---------------------------------|---------------|--------------------------------------------------------------------------------|-------------|
| `defaultText.buttonAccept`      | `string`     | Text for the "Accept" button.                                                 | `"Accept"`  |
| `defaultText.alertAgreementUpdated` | `string` | Message shown when agreements are updated.                                    | `"We’ve upgraded our terms. Please review."` |

---

## Examples

### Full Integration Example
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>TermClicks Demo</title>
    <script src="https://consentsapp.com/termclicks.js"></script>
</head>
<body>
    <form id="signupForm">
        <label>
            <input type="checkbox" id="consentCheckbox">
            I agree to the terms
        </label>
        <a href="#" id="termsLink">View Terms</a>
        <a href="#" id="privacyLink">View Privacy Policy</a>
        <button id="submitButton" disabled>Submit</button>
    </form>

    <script>
        var consentOptions = {
            accountId: "123456",
            agreementTypeIds: [1, 2],
            checkboxEl: "consentCheckbox",
            disableEls: ["submitButton"],
            forceAgreementView: true,
            agreementLinks: [
                { el: "termsLink", ag: 1 },
                { el: "privacyLink", ag: 2 }
            ],
        };
        var termClicksInstance = new TermClicks(consentOptions);
    </script>
</body>
</html>
```

---

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

---

## Contributing

We welcome contributions! If you'd like to improve this project, submit a pull request or open an issue.

---

## Support

For questions, issues, or feature requests, feel free to contact us or open an issue in the repository.
```

This `README.md` file covers the full usage of `termclicks.js`, providing clear explanations and examples for developers. It is designed to be developer-friendly and suitable for public repositories.
