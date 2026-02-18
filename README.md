<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# 🏗️ AWS Three-Tier Web App Series

**Author:** Mack (Saqib Hossain)  
**Email:** saqibh49@gmail.com  
**Series:** Build a Three-Tier Web App - NextWork Cloud Engineering Roadmap

---

## 📋 Series Overview

A hands-on deep-dive into serverless architecture through 4 projects that build on each other to create a fully functional three-tier web application. This series starts with delivering a static site through CloudFront, progresses to writing Lambda functions and building APIs, and culminates in connecting all three tiers — presentation, logic, and data — into a working app that pulls live data from DynamoDB and displays it in the browser.

**What I Built:**
- A CloudFront-delivered static website backed by a private S3 origin with OAC
- A Lambda function that retrieves user data from DynamoDB with least-privilege permissions
- A REST API with API Gateway including resources, methods, stages, and published documentation
- A complete three-tier web app connecting CloudFront → API Gateway → Lambda → DynamoDB

**Duration:** ~7.5 hours across 4 projects  
**Difficulty:** Intermediate

---

## 🎯 Learning Objectives

By completing this series, I gained hands-on experience with:

✅ **Content Delivery** - Configuring CloudFront distributions with S3 origins and OAC  
✅ **Serverless Compute** - Writing Lambda functions that query DynamoDB using AWS SDK  
✅ **API Development** - Building REST APIs with resources, methods, and deployment stages  
✅ **Three-Tier Architecture** - Connecting presentation, logic, and data tiers end-to-end  
✅ **IAM Least Privilege** - Choosing the right permission policies for Lambda execution roles  
✅ **CORS Configuration** - Resolving cross-origin errors between CloudFront and API Gateway  
✅ **API Documentation** - Writing and publishing JSON documentation for API consumers  
✅ **Troubleshooting** - Debugging browser console errors, access denied issues, and CORS blocks

---

## 📚 Project Index

### Phase 1: Presentation Tier (Project 1)

**1. [Website Delivery with CloudFront](https://github.com/makdaf1rst/aws-networks-cloudfront)**
- Uploaded index.html, style.css, and script.js to a private S3 bucket
- Created a CloudFront distribution with S3 as the origin
- Configured a default root object to serve index.html automatically
- Resolved access denied errors by setting up Origin Access Control (OAC)
- Updated the S3 bucket policy to grant CloudFront read access through OAC
- ⏱️ Duration: 2 hours

---

### Phase 2: Logic Tier (Projects 2-3)

**2. [Fetch Data with AWS Lambda](https://github.com/makdaf1rst/aws-compute-lambda)**
- Created a DynamoDB table with a userId partition key and sample data
- Built a Lambda function that retrieves user data by userId using AWS SDK
- Tested the function and resolved AccessDenied by attaching DynamoDBReadOnlyAccess
- Evaluated four DynamoDB permission policies and selected the least-privilege option
- Created an inline policy to restrict access to only the specific table
- ⏱️ Duration: 1 hour

**3. [APIs with Lambda + API Gateway](https://github.com/makdaf1rst/aws-compute-api)**
- Built a REST API in API Gateway with a /users resource and GET method
- Connected the GET method directly to the Lambda function
- Deployed the API to a prod stage and tested the invoke URL
- Wrote and published JSON API documentation with endpoint details
- ⏱️ Duration: 45 minutes

---

### Phase 3: Full Stack Integration (Project 4)

**4. [Build a Three-Tier Web App](https://github.com/makdaf1rst/aws-compute-threetier)**
- Connected all three tiers: CloudFront → API Gateway → Lambda → DynamoDB
- Updated script.js with the prod API invoke URL and reuploaded to S3
- Debugged CORS errors by enabling GET permissions and adding CloudFront to the allow list
- Updated Lambda function responses with Access-Control-Allow-Origin headers
- Validated the full pipeline: user enters userId → frontend calls API → Lambda queries DynamoDB → data displays in browser
- ⏱️ Duration: 4 hours

---

## 🛠️ Technologies & Services Used

**AWS Services:**
- Amazon CloudFront (Content Delivery Network)
- Amazon S3 (Static File Storage)
- AWS Lambda (Serverless Compute)
- Amazon API Gateway (REST API Management)
- Amazon DynamoDB (NoSQL Database)
- AWS IAM (Permission Policies & Execution Roles)

**Tools & Concepts:**
- Origin Access Control (OAC)
- CloudFront Distributions & Cache Invalidation
- Lambda Execution Roles & Inline Policies
- AWS SDK (JavaScript)
- REST APIs (Resources, Methods, Stages)
- Lambda Proxy Integration
- CORS (Cross-Origin Resource Sharing)
- API Documentation (JSON/Swagger)
- DynamoDB Partition Keys
- Browser Developer Tools (Console Debugging)

**Frontend Stack:**
- `index.html` - Content and layout
- `style.css` - Styling and design
- `script.js` - API calls and interactivity

---

## 🎓 Key Takeaways

### Three-Tier Architecture
- **Presentation tier** (CloudFront + S3) handles what the user sees
- **Logic tier** (API Gateway + Lambda) handles how data is processed
- **Data tier** (DynamoDB) handles where data is stored
- **Each tier is independent** — you can swap out any layer without breaking the others

### Serverless Patterns
- **Lambda only runs when triggered** — no servers to manage, pay only for execution time
- **API Gateway is the front door** — it receives requests, routes them to Lambda, and returns responses
- **DynamoDB is schemaless** — items can have different attributes, no upfront column definitions needed
- **Stages let you version your API** — dev for testing, prod for the public, without conflicts

### Security & Permissions
- **OAC keeps S3 private** — CloudFront gets access without making the bucket public
- **Least privilege matters** — ReadOnlyAccess over FullAccess protects data if the function is ever compromised
- **Inline policies add granularity** — restricting Lambda to a single table instead of all DynamoDB tables
- **Execution roles define boundaries** — Lambda can only do what its IAM role allows

### CORS & Troubleshooting
- **CORS errors happen when domains don't match** — CloudFront and API Gateway are different origins
- **Both sides need CORS config** — API Gateway allows the origin, Lambda includes headers in responses
- **Browser dev tools are essential** — console errors pointed directly to what was broken
- **Cache invalidation matters** — updating S3 files doesn't update CloudFront automatically

---

## 📊 Architecture Diagrams

### Three-Tier Web App Architecture
```
┌─────────────────────────────────────────────────────────────────┐
│                     THREE-TIER WEB APP                          │
│                                                                 │
│  PRESENTATION TIER                                              │
│  ┌──────────────┐      ┌──────────────────────┐               │
│  │  Amazon S3   │      │  Amazon CloudFront   │               │
│  │              │◄─────│  (CDN)               │               │
│  │ index.html   │ OAC  │                      │◄──── Users    │
│  │ style.css    │      │  Global edge         │               │
│  │ script.js    │      │  locations           │               │
│  └──────────────┘      └──────────┬───────────┘               │
│                                   │                            │
│ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─│─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─  │
│                                   │ CORS                       │
│  LOGIC TIER                       ▼                            │
│  ┌──────────────────────┐      ┌──────────────────────┐       │
│  │  API Gateway         │      │  AWS Lambda           │       │
│  │  (REST API)          │─────►│                       │       │
│  │                      │      │  RetrieveUserData()   │       │
│  │  /users (GET)        │◄─────│                       │       │
│  │  Stage: prod         │      │  Execution Role:      │       │
│  └──────────────────────┘      │  DynamoDB ReadOnly    │       │
│                                └──────────┬───────────┘       │
│                                           │                    │
│ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┼ ─ ─ ─ ─ ─ ─ ─ ─  │
│                                           │                    │
│  DATA TIER                                ▼                    │
│  ┌──────────────────────────────────────────────────────┐     │
│  │  Amazon DynamoDB                                      │     │
│  │                                                       │     │
│  │  Table: UserData                                      │     │
│  │  Partition Key: userId                                │     │
│  │  ┌─────────┬──────────┬─────────┬────────────────┐   │     │
│  │  │ userId  │ name     │ email   │ ...            │   │     │
│  │  ├─────────┼──────────┼─────────┼────────────────┤   │     │
│  │  │ 1       │ "Mack"   │ "m@..." │ attributes    │   │     │
│  │  └─────────┴──────────┴─────────┴────────────────┘   │     │
│  └──────────────────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────────────┘
```

### Request Flow
```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  1. USER ACTION                                                 │
│  ┌──────────────────────┐                                       │
│  │  User enters userId  │                                       │
│  │  Clicks "Get Data"   │                                       │
│  └──────────┬───────────┘                                       │
│             │                                                   │
│  2. FRONTEND REQUEST    ▼                                       │
│  ┌──────────────────────┐                                       │
│  │  script.js calls     │                                       │
│  │  API invoke URL      │                                       │
│  │  /users?userId=1     │                                       │
│  └──────────┬───────────┘                                       │
│             │                                                   │
│  3. API GATEWAY         ▼                                       │
│  ┌──────────────────────┐                                       │
│  │  Routes GET request  │                                       │
│  │  to Lambda function  │                                       │
│  │  (prod stage)        │                                       │
│  └──────────┬───────────┘                                       │
│             │                                                   │
│  4. LAMBDA              ▼                                       │
│  ┌──────────────────────┐                                       │
│  │  Queries DynamoDB    │                                       │
│  │  for userId = 1      │                                       │
│  │  Returns data +      │                                       │
│  │  CORS headers        │                                       │
│  └──────────┬───────────┘                                       │
│             │                                                   │
│  5. DYNAMODB            ▼                                       │
│  ┌──────────────────────┐                                       │
│  │  Returns item:       │                                       │
│  │  { name, email, ... }│                                       │
│  └──────────┬───────────┘                                       │
│             │                                                   │
│  6. RESPONSE            ▼                                       │
│  ┌──────────────────────┐                                       │
│  │  Data displayed in   │                                       │
│  │  browser via DOM     │                                       │
│  └──────────────────────┘                                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### CORS Resolution Flow
```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  ❌ BEFORE: CORS Error                                           │
│  ┌──────────────┐         ┌──────────────────┐                  │
│  │  CloudFront  │────────►│  API Gateway     │                  │
│  │  (domain A)  │  🚫     │  (domain B)      │                  │
│  │              │◄── X ───│  No CORS headers │                  │
│  └──────────────┘         └──────────────────┘                  │
│  Browser blocks cross-origin response                           │
│                                                                 │
│  ✅ AFTER: CORS Resolved                                         │
│  ┌──────────────┐         ┌──────────────────┐                  │
│  │  CloudFront  │────────►│  API Gateway     │                  │
│  │  (domain A)  │         │  ✅ CORS enabled   │                 │
│  │              │         │  ✅ Origin allowed  │                 │
│  │              │◄────────│                  │                  │
│  └──────────────┘         └────────┬─────────┘                  │
│                                    │                            │
│                           ┌────────▼─────────┐                  │
│                           │  Lambda          │                  │
│                           │  ✅ CORS headers   │                 │
│                           │  in all responses│                  │
│                           └──────────────────┘                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 💡 Unexpected Learnings

Throughout this series, several insights stood out:

1. **CloudFront is not storage** — It only caches copies temporarily and always pulls originals from S3, so both services are needed

2. **OAC is the modern way to secure S3 origins** — It replaces the older Origin Access Identity and gives more control without making the bucket public

3. **Lambda permission policies have real tradeoffs** — Choosing between FullAccess and ReadOnlyAccess isn't just preference, it's a security decision that matters if the function is ever compromised

4. **CORS requires changes on both sides** — Enabling it in API Gateway alone wasn't enough; Lambda responses also needed Access-Control-Allow-Origin headers

5. **API stages are like deployment environments** — Dev, test, and prod can all exist simultaneously, letting you make changes without touching what's live

6. **The browser console is the best debugging tool** — Every error I hit in the three-tier project showed up in developer tools before I could figure out what went wrong

7. **Connecting all three tiers is where the real challenge is** — Each tier worked individually, but making them talk to each other required careful attention to URLs, permissions, and headers

---

## 🚀 What's Next

This three-tier series is Phase 5 of the complete Cloud Engineering roadmap. Other phases include:

**Phase 1: Foundations** - S3 hosting and IAM security basics  
**Phase 2: Networking** - VPC architecture, peering, endpoints, and monitoring  
**Phase 3: Databases** - Aurora MySQL, DynamoDB, and application integration  
**Phase 4: Security** - KMS encryption, GuardDuty, Secrets Manager, CloudTrail monitoring  
**Phase 6: DevOps** - CI/CD pipelines, Terraform, CloudFormation, multi-cloud  
**Phase 7: AI on Cloud** - Amazon Lex chatbots, AI transcription, Lambda integration

---

## 📂 Documentation Links

**Individual Project Repositories:**
- [Project 1: Website Delivery with CloudFront](https://github.com/makdaf1rst/aws-networks-cloudfront)
- [Project 2: Fetch Data with AWS Lambda](https://github.com/makdaf1rst/aws-compute-lambda)
- [Project 3: APIs with Lambda + API Gateway](https://github.com/makdaf1rst/aws-compute-api)
- [Project 4: Build a Three-Tier Web App](https://github.com/makdaf1rst/aws-compute-threetier)

**External Resources:**
- [NextWork Cloud Engineering Roadmap](https://learn.nextwork.org/projects/aws-compute-threetier?explore=series:cloud%20engineer)
- [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/)
- [AWS API Gateway Documentation](https://docs.aws.amazon.com/apigateway/)
- [AWS CloudFront Documentation](https://docs.aws.amazon.com/cloudfront/)
- [AWS DynamoDB Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/)

---

## 🤝 Connect With Me

💼 [LinkedIn](https://linkedin.com/in/saqibhos)  
📧 Email: saqibh49@gmail.com  
🐙 GitHub: [@makdaf1rst](https://github.com/makdaf1rst)  
📘 [Facebook](https://facebook.com/mackdaf1rst)  
📷 [Instagram](https://instagram.com/mackdaf1rst)  
🤖 [Reddit](https://reddit.com/user/Mak_Mark_One)

---

## 📝 License

This project documentation is for educational purposes as part of the NextWork Cloud Engineering curriculum.

---

## 🙏 Acknowledgments

Special thanks to **NextWork** for providing a structured, hands-on cloud engineering curriculum that emphasizes real-world scenarios and practical troubleshooting.

---

<div align="center">

**⭐ If you found this helpful, please star this repository!**

*Building secure, scalable cloud infrastructure one project at a time.*

</div>

---

<!-- Proudly created as part of NextWork's Cloud Engineering Roadmap -->
