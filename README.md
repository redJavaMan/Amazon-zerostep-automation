# Amazon ZeroStep AI Automation

A comprehensive Playwright-based test automation framework for Amazon with multi-environment support and AI-powered testing capabilities using ZeroStep.

## 📋 Table of Contents

- [Features](#features)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [ZeroStep Playwright Setup](#zerostep-playwright-setup)
- [Configuration](#configuration)
- [Running Tests](#running-tests)
- [Available Scripts](#available-scripts)
- [Environment Variables](#environment-variables)
- [Test Scenarios](#test-scenarios)
- [Page Object Model](#page-object-model)
- [Troubleshooting](#troubleshooting)

## ✨ Features

- **Multi-Environment Support**: Test across 16+ Amazon marketplaces (US, UK, AU, IN, DE, FR, JP, CN, IT, NL, ES, MX, BR, CA, RU)
- **Page Object Model**: Clean, maintainable test architecture with fixtures
- **AI-Powered Testing**: Integration with ZeroStep Playwright for natural language test automation
- **Comprehensive Test Coverage**: Account creation, address setup, payment card management
- **Cross-Browser Testing**: Support for Chromium, Firefox, and WebKit
- **Parallel Execution**: Run tests in parallel for faster feedback
- **Detailed Reporting**: HTML reports with screenshots and videos on failure
- **Dynamic User Generation**: Automatic test user data generation per environment with timestamp-based unique emails
- **Automated Language Detection**: Automatically changes language to English for non-English marketplaces

## 📁 Project Structure

```
amazon-zerostep-automation/
├── env/                          # Environment configuration files
│   ├── .env.us                   # US marketplace config
│   ├── .env.uk                   # UK marketplace config
│   ├── .env.au                   # Australia marketplace config
│   ├── .env.in                   # India marketplace config
│   └── ...                       # Other marketplace configs (16 total)
├── fixtures/                     # Test fixtures
│   └── baseFixture.ts           # Base fixture with page objects
├── pages/                        # Page Object Models
│   ├── homePage.ts              # Home page actions (navigation, popups, language)
│   ├── loginPage.ts             # Login/signup page actions with OTP polling
│   ├── contentLibraryPage.ts    # Content library navigation
│   └── preferencesPage.ts       # Preferences (address, payments)
├── tests/                        # Test specifications
│   └── createAccount.spec.ts    # Account creation and sign-in tests
├── tests-examples/               # Example tests
│   └── demo-todo-app.spec.ts    # Demo test example
├── utils/                        # Utility functions
│   ├── env.ts                   # Environment variable handler
│   └── userInformations.ts      # Test user data generator
├── .gitignore                    # Git ignore configuration
├── playwright.config.ts          # Playwright configuration
├── package.json                  # Project dependencies
├── package-lock.json             # Dependency lock file
├── zerostep.config.json         # ZeroStep configuration
└── README.md                     # This file
```

## 🔧 Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v18 or higher)
- **npm** (v9 or higher)
- **Git** (for version control)

## 📦 Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd amazon-zerostep-automation
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Install Playwright browsers**
   ```bash
   npx playwright install
   ```

4. **Verify installation**
   ```bash
   npx playwright --version
   ```

## 🤖 ZeroStep Playwright Setup

ZeroStep enables AI-powered test automation using natural language commands. Follow these steps to set it up:

### Step 1: Install ZeroStep

ZeroStep is already included in `package.json` dependencies:

```json
"@zerostep/playwright": "^0.1.5"
```

### Step 2: Get Your ZeroStep API Token

1. Visit [ZeroStep Dashboard](https://app.zerostep.com)
2. Sign up or log in to your account
3. Navigate to API Settings or Project Settings
4. Generate a new API token
5. Copy the token (format: `0step:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`)

### Step 3: Configure ZeroStep

The `zerostep.config.json` file is configured to use an environment variable:

```json
{
  "TOKEN": "${ZEROSTEP_TOKEN}"
}
```

Set the environment variable:

**Linux/Mac:**
```bash
export ZEROSTEP_TOKEN="0step:your-api-token-here"
```

**Windows (Command Prompt):**
```cmd
set ZEROSTEP_TOKEN=0step:your-api-token-here
```

**Windows (PowerShell):**
```powershell
$env:ZEROSTEP_TOKEN="0step:your-api-token-here"
```

**⚠️ Security Note**: Never commit your actual API token to version control.

### Step 4: Import and Use ZeroStep in Tests

In your test files, import the `ai` function:

```typescript
import { ai } from '@zerostep/playwright';

test('example test', async ({ page }) => {
  const aiArgs = { page, test };
  
  // Use natural language commands
  await ai('Hover over Account & Lists and click Sign in', aiArgs);
  await ai('Enter "user@example.com" in the email field', aiArgs);
  await ai('Click the Continue button', aiArgs);
});
```

## ⚙️ Configuration

### Environment Files

Each environment file (`.env.*`) contains marketplace-specific configuration:

```env
ENV="US"
BASE_URL="https://www.amazon.com"
CITY="ALASKA"
STATE="ALASKA"
COUNTRY="United States"
POSTAL_CODE="99546"
```

**Available Environments:**
- `us` - United States (amazon.com)
- `uk` - United Kingdom (amazon.co.uk)
- `au` - Australia (amazon.au)
- `in` - India (amazon.in)
- `de` - Germany (amazon.de)
- `frfr` - France (amazon.fr)
- `frca` - French Canada (amazon.ca)
- `jp` - Japan (amazon.co.jp)
- `cn` - China (amazon.cn)
- `it` - Italy (amazon.it)
- `nl` - Netherlands (amazon.nl)
- `esmx` - Mexico (amazon.com.mx)
- `ptbr` - Brazil (amazon.com.br)
- `ru` - Russia (amazon.com with Russian locale)
- `esar` - Argentina Spanish (amazon.es)
- `eses` - Spain (amazon.es)

### Playwright Configuration

Key settings in `playwright.config.ts`:

- **Timeout**: 280 seconds (4.6 minutes) for long-running operations
- **Parallel Execution**: Enabled by default (`fullyParallel: true`)
- **Retries**: 2 retries on CI, 0 locally
- **Screenshots**: Captured on failure only
- **Videos**: Recorded on failure only
- **Slow Motion**: 200ms delay between actions for better visibility
- **Headless Mode**: Disabled by default (`headless: false`)
- **Base URL**: Loaded from environment configuration

### User Information Generator

The `UserGenerator` class in `utils/userInformations.ts` generates unique test data:

```typescript
{
  name: "Mohammed Lukmanudhin",
  email: "xyzabbc+{country}{timestamp}@gmail.com",
  password: "abc123",
  phone: "9876543210",
  card: {
    number: "1234 5678 9012 3456",
    expiry: "01/2029"
  }
}
```

## 🚀 Running Tests

### Basic Test Execution

```bash
# Run all tests (default: US environment)
npm test

# Run tests for specific environment
npm run test:us
npm run test:uk
npm run test:au
npm run test:in
npm run test:de
```

### Headed Mode (See Browser)

```bash
# Run tests in headed mode (browser visible)
npm run test:headed:us
npm run test:headed:uk
npm run test:headed:in
```

### Debug Mode

```bash
# Run tests in debug mode with Playwright Inspector
npm run test:debug:us
npm run test:debug:uk
npm run test:debug:in
```

### Run Specific Test

```bash
# Run specific test file
cross-env ENV=us npx playwright test tests/createAccount.spec.ts

# Run specific test by name
cross-env ENV=us npx playwright test -g "Create Account"

# Run with ZeroStep AI
cross-env ENV=us npx playwright test -g "Create Account with Zero Step AI"

# Run sign-in test
cross-env ENV=us npx playwright test -g "Sign In with Zero Step AI"
```

### View Test Report

```bash
npm run report
```

## 📜 Available Scripts

### Test Execution Scripts

| Script | Description |
|--------|-------------|
| `npm test` | Run all tests (default environment) |
| `npm run test:us` | Run tests for US marketplace |
| `npm run test:uk` | Run tests for UK marketplace |
| `npm run test:au` | Run tests for Australia marketplace |
| `npm run test:in` | Run tests for India marketplace |
| `npm run test:de` | Run tests for Germany marketplace |
| `npm run test:frfr` | Run tests for France marketplace |
| `npm run test:frca` | Run tests for French Canada marketplace |
| `npm run test:jp` | Run tests for Japan marketplace |
| `npm run test:cn` | Run tests for China marketplace |
| `npm run test:it` | Run tests for Italy marketplace |
| `npm run test:nl` | Run tests for Netherlands marketplace |
| `npm run test:esmx` | Run tests for Mexico marketplace |
| `npm run test:ptbr` | Run tests for Brazil marketplace |
| `npm run test:ru` | Run tests for Russia marketplace |
| `npm run test:esar` | Run tests for Argentina (Spanish) marketplace |
| `npm run test:eses` | Run tests for Spain marketplace |

### Headed Mode Scripts

All environments support headed mode with the pattern: `npm run test:headed:{env}`

### Debug Mode Scripts

All environments support debug mode with the pattern: `npm run test:debug:{env}`

### Code Generation Scripts

Generate test code using Playwright's codegen tool:

```bash
npm run codegen:us       # Generate code for US marketplace
npm run codegen:uk       # Generate code for UK marketplace
# ... (similar pattern for all environments)
```

## 🌍 Environment Variables

### Per-Environment Configuration

Each marketplace has its own `.env` file with the following variables:

| Variable | Description | Example |
|----------|-------------|---------|
| `ENV` | Environment identifier | `"US"` |
| `BASE_URL` | Amazon marketplace URL | `"https://www.amazon.com"` |
| `CITY` | Default city for address | `"ALASKA"` |
| `STATE` | Default state/region | `"ALASKA"` |
| `COUNTRY` | Country name | `"United States"` |
| `POSTAL_CODE` | Default postal code | `"99546"` |
| `ZEROSTEP_TOKEN` | ZeroStep API token (set separately) | `"0step:xxxxx"` |

### Adding a New Environment

1. Create a new `.env` file in the `env/` directory:
   ```bash
   touch env/.env.newmarket
   ```

2. Add configuration:
   ```env
   ENV="NEWMARKET"
   BASE_URL="https://www.amazon.newmarket"
   CITY="Default City"
   STATE="Default State"
   COUNTRY="Country Name"
   POSTAL_CODE="12345"
   ```

3. Add npm scripts in `package.json`:
   ```json
   {
     "scripts": {
       "test:newmarket": "cross-env ENV=newmarket playwright test",
       "test:headed:newmarket": "cross-env ENV=newmarket playwright test --headed",
       "test:debug:newmarket": "cross-env ENV=newmarket playwright test --debug",
       "codegen:newmarket": "cross-env ENV=newmarket playwright codegen"
     }
   }
   ```

## 🧪 Test Scenarios

### 1. Create Account (Traditional Playwright)

**Test Name:** `Create Account`

Creates a new Amazon account with complete profile setup using traditional Playwright selectors:

**Flow:**
1. Navigate to Amazon marketplace
2. Dismiss any popups (cookie consent, location, promotions)
3. Change language to English (if not US/UK/Brazil/Mexico/Russia)
4. Navigate to Sign In page
5. Proceed to create account
6. Fill in account details (name, email, password)
7. Verify OTP (waits for manual entry with polling mechanism)
8. Navigate to Content Library
9. Access Preferences
10. Set address with location-specific data
11. Add payment card

**Usage:**
```bash
npm run test:us -- -g "Create Account"
```

**Features:**
- Automated popup dismissal
- Language detection and switching
- OTP polling mechanism (waits up to 3 minutes)
- Dynamic user data generation with unique email per run
- Complete address and payment setup

### 2. Create Account with ZeroStep AI

**Test Name:** `Create Account with Zero Step AI`

Same functionality as above but uses natural language commands via ZeroStep AI for element interaction:

**AI Commands Used:**
- `'Hover over Account & Lists and click Sign in'`
- `'Enter the email in the {email} field and click Continue'`
- `'Click "Proceed to create an account"'`
- `'Enter the name "{name}" in the name field'`
- And more...

**Usage:**
```bash
npm run test:us -- -g "Create Account with Zero Step AI"
```

**Benefits:**
- More resilient to UI changes
- Natural language commands are easier to read and maintain
- Automatically handles element location

### 3. Sign In with ZeroStep AI

**Test Name:** `Sign In with Zero Step AI`

Tests existing account sign-in flow using ZeroStep AI and performs address and payment setup:

**Flow:**
1. Navigate to sign-in page
2. Sign in with existing credentials (hardcoded in test)
3. Navigate to Content Library → Preferences
4. Update address information
5. Add payment card

**Usage:**
```bash
npm run test:us -- -g "Sign In with Zero Step AI"
```

**Note:** Update the email and password in the test file for your test account.

### Test Output

All tests log account details after successful creation:

```
Account created successfully
Account Details:
  Email: xyzabbc+us1234567890@gmail.com
  Password: abc123
  Address: ALASKA, ALASKA, United States, 99546,
  Phone: 9876543210,
  Card: 1234 5678 9012 3456, Expiry: 01/2029
```

## 📦 Page Object Model

### Base Fixture (`fixtures/baseFixture.ts`)

Extends Playwright's base test with custom fixtures for all page objects:

```typescript
type baseFixtures = {
  loginPage: LoginPage;
  homePage: HomePage;
  contentLibraryPage: ContentLibraryPage;
  preferencesPage: PreferencesPage;
};
```

### HomePage (`pages/homePage.ts`)

Handles main navigation and initial page interactions:

**Methods:**
- `goto()` - Navigate to marketplace and handle initial popups
- `goToSignIn()` - Navigate to sign-in page with retry logic
- `goToMenu(menuName)` - Navigate to account menu items
- `goToContentLibrary()` - Navigate to content library
- `dismissPopupIfPresent()` - Dismiss various popups (cookies, location, promotions)
- `changeLanguageToEnglishIfNot()` - Auto-detect and change language to English

**Features:**
- Polling mechanism for dynamic elements
- Multi-popup handling
- Language detection (excludes US, UK, Brazil, Mexico, Russia)

### LoginPage (`pages/loginPage.ts`)

Manages authentication flow:

**Methods:**
- `createAccount(name, email, password)` - Handle account creation flow
- `proceedToCreateAccount()` - Navigate to account creation form
- `verifyOTP()` - Wait for manual OTP entry with polling (up to 3 minutes)

**Features:**
- Intelligent OTP polling mechanism
- Handles both direct signup and email-first flows
- Configurable timeout (180 seconds default)

### ContentLibraryPage (`pages/contentLibraryPage.ts`)

Manages content library navigation:

**Methods:**
- `goToTab(tabName)` - Navigate between library tabs
- `goToPreferences()` - Navigate to preferences page

### PreferencesPage (`pages/preferencesPage.ts`)

Handles account preferences:

**Methods:**
- `setAddress(name, address1, city, state, postalCode, phoneNumber)` - Set/update address
- `addCard()` - Add payment card (with iframe handling)

**Features:**
- Iframe handling for secure payment input
- Pre-configured test card details
- Address validation

## 🛠️ Troubleshooting

### Common Issues

**1. Tests fail with timeout errors**

```bash
# Increase timeout in playwright.config.ts
timeout: 480000  // 8 minutes instead of 280 seconds
```

**2. ZeroStep commands not working**

- Verify your API token is set: `echo $ZEROSTEP_TOKEN`
- Check your ZeroStep account has available credits at https://app.zerostep.com
- Ensure you have internet connectivity
- Check ZeroStep service status

**3. OTP verification timeout**

The OTP polling mechanism waits up to 3 minutes. To extend:

```typescript
// In loginPage.ts, modify the timeout
timeout: 300000  // 5 minutes
```

**4. Element not found errors**

- Check if the marketplace UI has changed
- Update selectors in respective Page Object files
- Use Playwright Inspector: `npm run test:debug:us`
- Try using ZeroStep AI tests which are more resilient

**5. Environment not loading correctly**

```bash
# Verify env file exists
ls env/.env.us

# Check environment variable is set
echo $ENV

# Verify content
cat env/.env.us
```

**6. Language not changing automatically**

The framework automatically changes language to English for most marketplaces. If issues occur:
- Check the exclusion list in `homePage.ts` (`changeLanguageToEnglishIfNot()`)
- Add your marketplace to the exclusion list if it's already in English
- Verify language dropdown is visible

**7. Popup handling issues**

Multiple popups are handled automatically. If new popups appear:
- Add new selectors to `homePage.ts` in `dismissPopupIfPresent()`
- Increase wait time before dismissal: `await this.page.waitForTimeout(3000)`

**8. Payment card iframe issues**

If payment card addition fails:
- Check iframe selector in `preferencesPage.ts`
- Verify iframe loading: `await page.waitForTimeout(5000)`
- Try using ZeroStep AI for payment flow

### Debug Tips

1. **Use Playwright Inspector**
   ```bash
   npm run test:debug:us
   ```

2. **Enable verbose logging**
   ```bash
   DEBUG=pw:api npm run test:us
   ```

3. **Take screenshots manually**
   ```typescript
   await page.screenshot({ path: 'debug.png', fullPage: true });
   ```

4. **Use slow motion**
   ```typescript
   // In playwright.config.ts
   launchOptions: {
     slowMo: 500  // Increase for slower execution
   }
   ```

5. **Check network requests**
   ```typescript
   page.on('request', request => console.log('>>', request.method(), request.url()));
   page.on('response', response => console.log('<<', response.status(), response.url()));
   ```

6. **Use headed mode**
   ```bash
   npm run test:headed:us
   ```
   This helps visualize what's happening in the browser.

7. **Check element visibility**
   ```typescript
   console.log('Element visible:', await element.isVisible());
   console.log('Element enabled:', await element.isEnabled());
   ```

### Known Limitations

1. **OTP Verification**: Requires manual entry as SMS cannot be automated
2. **Payment Processing**: Uses test card numbers only
3. **Geographic Restrictions**: Some marketplaces may require VPN
4. **Rate Limiting**: Amazon may rate-limit account creation attempts
5. **CAPTCHA**: May require manual intervention if triggered

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the ISC License.

## 👤 Author

**Mohammed Lukmanudin M**
- GitHub: [@redJavaMan](https://github.com/redJavaMan)

## 🙏 Acknowledgments

- [Playwright](https://playwright.dev/) - Browser automation framework
- [ZeroStep](https://zerostep.com/) - AI-powered test automation
- [Amazon](https://amazon.com/) - Test target platform

---

**Happy Testing! 🚀**

**Need Help?** Open an issue on GitHub or check the [Playwright Documentation](https://playwright.dev/docs/intro)
