# Local Development Guide for WarungKu POS

Follow these steps to run your Point of Sale application on your local computer.

## Prerequisites

Before you begin, ensure you have the following installed:
- [Node.js](https://nodejs.org/) (version 18 or higher is recommended)
- `npm` (which is included with Node.js)

## Step 1: Set Up Your Google Sheet

Your application uses a Google Sheet as its database.

1.  Create a new Google Sheet in your Google account.
2.  Create the following tabs (sheets) at the bottom. The names must be exact:
    - `Products`
    - `Pending Transactions`
    - `Completed Transactions`
    - `Expenditures`
    - `Settings`
3.  **Copy the Sheet ID**: The Sheet ID is the long string of characters in the URL of your sheet.
    - Example URL: `https://docs.google.com/spreadsheets/d/THIS_IS_THE_SHEET_ID/edit`
    - You will need this for your environment variables.

## Step 2: Configure Environment Variables

Your app needs credentials to securely connect to Google Sheets and Google AI. You will create a `.env.local` file in the root of your project to store these.

#### 1. Create the File
Create a new file named `.env.local` in the main directory of your project.

#### 2. Get Google Service Account Credentials

A service account is an identity your application uses to access Google services.

1.  **Go to the Google Cloud Console**: Navigate to the [Service Accounts page](https://console.cloud.google.com/iam-admin/serviceaccounts).
2.  **Select Your Project**: Choose the Google Cloud project associated with your app.
3.  **Enable APIs**:
    - Go to the [API Library](https://console.cloud.google.com/apis/library).
    - Search for and enable both the **Google Sheets API** and the **Vertex AI API**.
4.  **Create a Service Account**:
    - Click **"+ CREATE SERVICE ACCOUNT"**.
    - Give it a name (e.g., "WarungKu POS Sheet Access") and an optional description.
    - Click **"CREATE AND CONTINUE"**.
    - For permissions, grant the **"Editor"** role to this service account for simplicity in a development environment.
    - Click **"CONTINUE"**, then **"DONE"**.
5.  **Create a Key**:
    - Find the service account you just created in the list.
    - Click the three dots (⋮) under "Actions" and select **"Manage keys"**.
    - Click **"ADD KEY"** -> **"Create new key"**.
    - Choose **JSON** as the key type and click **"CREATE"**. A JSON file will be downloaded to your computer.
6.  **Share Your Google Sheet**:
    - Open the downloaded JSON file. Find the `client_email` value (it looks like an email address).
    - Go back to your Google Sheet. Click the **"Share"** button in the top right.
    - Paste the `client_email` into the sharing dialog and grant it **Editor** permissions. Click **"Share"**.

#### 3. Get Your Gemini API Key

1.  Navigate to the [Google AI Studio](https://aistudio.google.com/app/apikey).
2.  Click **"Create API key in new project"**.
3.  Copy the generated API key.

#### 4. Populate Your `.env.local` File

Open the `.env.local` file and add the following variables.

```
# .env.local

# -- Google Sheets --
# The ID of your Google Sheet (from the URL)
SHEETS_ID="YOUR_GOOGLE_SHEET_ID"

# -- Service Account Credentials (from the downloaded JSON file) --
# The client_email from your service account JSON file
GOOGLE_SERVICE_ACCOUNT_CLIENT_EMAIL="your-service-account-email@your-project.iam.gserviceaccount.com"
# The private_key from your service account JSON file.
# IMPORTANT: Wrap the entire key in double quotes, including the -----BEGIN and -----END parts.
GOOGLE_SERVICE_ACCOUNT_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nYOUR_PRIVATE_KEY_CONTENT_HERE\n-----END PRIVATE KEY-----\n"

# -- Google AI (Genkit) --
# The API key you generated from Google AI Studio
GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
```

**Note on the Private Key**: The `private_key` in the JSON file contains `\n` characters for newlines. Make sure these are preserved when you copy the key into your `.env.local` file. Wrapping it in double quotes helps ensure it's read correctly.

## Step 3: Install Dependencies and Run the App

1.  **Open a Terminal**: Navigate to the root directory of your project in your terminal or command prompt.
2.  **Install Dependencies**: Run the following command to install all the necessary packages.
    ```bash
    npm install
    ```
3.  **Run the Development Server**: Start the application.
    ```bash
    npm run dev
    ```

Your application should now be running locally! You can access it in your browser at **http://localhost:3000**.
