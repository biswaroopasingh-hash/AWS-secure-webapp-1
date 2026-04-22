# AWS-secure-webapp-1
🛡️ AWS Secure Web Application
A secure, serverless, two-page web application built entirely on Amazon Web Services (AWS). The system features a Cognito-authenticated login page and a JWT-protected dashboard, deployed automatically via GitHub CI/CD using AWS Amplify.

🌐 Live Demo
Hosted on AWS Amplify — automatically deployed from this repository.

📋 Project Overview
This project demonstrates how multiple AWS cloud services can be integrated to build a secure, scalable, and fully automated web application. It consists of two frontend pages:

Login Page (index.html) — Authenticates users using Amazon Cognito with email and password. On successful login, JWT tokens are stored in sessionStorage and the user is redirected to the dashboard.
Dashboard Page (dashboard.html) — A protected AWS architecture dashboard. Every page load checks for a valid JWT token. Users without a valid token are shown an Access Denied screen and automatically redirected to login.


🏗️ Architecture
User → AWS WAF → AWS Amplify → Login Page
                                    ↓
                             Amazon Cognito
                            (JWT Tokens issued)
                                    ↓
                             Dashboard Page
                            (JWT Guard Check)
                                    ↓
                           Amazon API Gateway
                          (Cognito Authorizer)
                                    ↓
                            AWS Lambda (Python)
                                    ↓
                           Amazon DynamoDB
                                    ↓
                          Amazon CloudWatch

⚙️ AWS Services Used
ServicePurposeAWS AmplifyHosts both HTML pages with automatic CI/CD from GitHubAmazon CognitoUser authentication using USER_PASSWORD_AUTH flowAmazon API GatewayExposes POST /submit endpoint with Cognito JWT AuthorizerAWS LambdaPython 3.12 serverless function that processes and saves login recordsAmazon DynamoDBNoSQL database storing login records with email as partition keyAWS WAFWeb Application Firewall blocking SQL injection, XSS, and malicious inputsAmazon CloudWatchLogs, monitoring dashboard, and SNS error alertsAWS IAMRole-based access control following Principle of Least PrivilegeGitHubVersion control and CI/CD trigger for Amplify auto-deployment

🔒 Security Features

✅ JWT token authentication via Amazon Cognito
✅ Client-side route protection on dashboard — no token = Access Denied
✅ API Gateway rejects requests without valid JWT (401 Unauthorized)
✅ AWS WAF blocks SQL injection, XSS, and known bad inputs
✅ Tokens stored in sessionStorage — cleared automatically on tab close
✅ Logout clears all tokens and terminates session immediately
✅ IAM roles follow Principle of Least Privilege — no hardcoded credentials


📁 Project Structure
aws-secure-webapp/
│
├── index.html          # Login page — Cognito authentication
├── dashboard.html      # Protected dashboard — JWT guard + AWS architecture
└── README.md           # Project documentation

🚀 How It Works
1. User opens the Login Page
The page is served by AWS Amplify over HTTPS. AWS WAF inspects all incoming traffic before it reaches the application.
2. User enters email and password
JavaScript calls the Amazon Cognito InitiateAuth API directly from the browser using USER_PASSWORD_AUTH flow.
3. Cognito authenticates the user
On success, Cognito returns IdToken, AccessToken, and RefreshToken. All three tokens plus the user email are saved to sessionStorage.
4. User is redirected to the Dashboard
After 1.5 seconds the browser redirects to dashboard.html. The dashboard checks sessionStorage for a valid token on every load.
5. Dashboard renders for authenticated users
A sticky navbar shows the logged-in user's email and a Logout button. The dashboard displays the full AWS architecture of the system.
6. Unauthenticated access is blocked
Opening dashboard.html directly without a token shows an Access Denied overlay with a 3-second countdown before redirecting to login.
7. Logout terminates the session
Clicking Logout clears all tokens from sessionStorage. The dashboard becomes inaccessible immediately.

🗄️ Database

Service: Amazon DynamoDB
Table Name: LoginData
Partition Key: email (String)
Records: Each login saves the user's email and name


📊 Monitoring

CloudWatch Log Group: /aws/lambda/LoginFormHandler
CloudWatch Dashboard: LoginAppMonitor — tracks Invocations, Errors, Duration
CloudWatch Alarm: LoginErrorAlarm — sends SNS email alert when Lambda errors exceed 0


🛠️ Implementation Steps
The project was implemented in 3 sittings across 15 steps:
Sitting 1 — GitHub + Amplify + Lambda + API Gateway
Sitting 2 — DynamoDB + Cognito + Connect Everything + Full System Test
Sitting 3 — WAF + CloudWatch Logs + Dashboard + Error Alarm

🧪 Test Scenarios Verified
TestExpected ResultStatusCorrect login credentialsJWT saved, redirect to dashboard✅ PassWrong password"❌ Incorrect email or password."✅ PassUnknown email"❌ No account found with this email."✅ PassEmpty fields"❌ Please fill in all fields"✅ PassDirect dashboard URL (no login)Access Denied + redirect to login✅ PassLogoutTokens cleared, dashboard blocked✅ PassSQL injection via API403 Forbidden from WAF✅ PassRequest without JWT token401 Unauthorized from API Gateway✅ Pass

📚 Academic Context
This project was developed as a Minor Project for the Master of Computer Applications (MCA) programme. It demonstrates practical implementation of cloud computing, serverless architecture, authentication, security, and monitoring using industry-standard AWS services.
Keywords: AWS Amplify, Amazon Cognito, JWT Authentication, AWS Lambda, API Gateway, DynamoDB, AWS WAF, CloudWatch, Serverless Architecture, CI/CD, Route Protection, sessionStorage

👨‍💻 Author
MCA Minor Project
Secure Cloud-Based Web Application Using AWS Services
