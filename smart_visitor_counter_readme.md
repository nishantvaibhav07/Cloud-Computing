# Smart Visitor Counter 🚀

A serverless IoT visitor counter application built on **AWS** that tracks real-time entries and exits using a web-based gate sensor controller and displays live metrics on a dynamic dashboard.

---

## 🏗️ Architecture & Tech Stack

* **Frontend**: HTML5, CSS3, JavaScript (`display.html` for live tracking, `controller.html` for gate simulation).
* **Backend**: AWS Lambda (Python 3.x) handling `GET` and `POST` requests.
* **API Layer**: AWS API Gateway (HTTP API v2) with CORS enabled.
* **Database**: Amazon DynamoDB (`SmartVisitorCounter` table with partition key `counter_id`).
* **Storage (Optional)**: Amazon S3 bucket for static JSON synchronization (`count.json`).

---

## 📂 Project Structure

```text
smart-visitor-counter/
│
├── display.html         # Live public visitor counter dashboard
├── controller.html      # Virtual gate sensor controller (+1 / -1 / Reset)
├── lambda_function.py   # AWS Lambda backend logic
└── README.md            # Project documentation
```

---

## ⚙️ Setup & Deployment Guide

### 1. Amazon DynamoDB Setup
1. Create a DynamoDB table named **`SmartVisitorCounter`**.
2. Set the **Partition key** to **`counter_id`** (String).
3. Explore items and create an initial item:
   * `counter_id`: `visitor` (String)
   * `count`: `0` (Number)

### 2. AWS Lambda Configuration
1. Create a Lambda function (Python 3.x) named **`VisitorCounterFunction`**.
2. Attach an execution role (such as **`LabRole`** in AWS Academy environments) with permissions for DynamoDB and S3.
3. Add Environment Variables under **Configuration > Environment variables**:
   * `TABLE_NAME`: `SmartVisitorCounter`
   * `BUCKET_NAME`: *(Optional)* Your S3 bucket name
4. Paste your Python backend code into the Lambda editor and click **Deploy**.

### 3. AWS API Gateway Configuration
1. Create an **HTTP API** in API Gateway.
2. Add two routes under `/visitor`:
   * **`GET /visitor`**: Integrated with `VisitorCounterFunction` to fetch the current count.
   * **`POST /visitor`**: Integrated with `VisitorCounterFunction` to increment the count and update storage.
3. Configure **CORS** to allow `GET` and `POST` methods with `*` origin.

### 4. Frontend Configuration
1. Open `display.html` and `controller.html`.
2. Update the API endpoint URL variable with your unique API Gateway invoke URL (e.g., `https://<api-id>.execute-api.us-east-1.amazonaws.com/visitor`).
3. Host locally using a local development server (such as VS Code Live Server) or deploy to an Amazon S3 static website bucket.

---

## 🔌 API Endpoints

| Method | Endpoint | Description | Sample Response |
| :--- | :--- | :--- | :--- |
| `GET` | `/visitor` | Retrieves the current visitor count from DynamoDB. | `{"count": 5}` |
| `POST` | `/visitor` | Increments the visitor count by 1 and returns the updated value. | `{"count": 6}` |

---

## 💡 Usage

1. Open **`display.html`** in your browser to view the real-time visitor count.
2. Open **`controller.html`** in another tab and click **+1 Visitor Enter** to simulate gate sensors updating the cloud database instantly.