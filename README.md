# Kestra Workflows Repository

This repository contains Kestra workflows designed to perform sentiment analysis on social media posts for Instagram and Twitter, as well as analyze custom CSV files. These workflows include both individual flows (for specific user tasks) and mass flows (which manage multiple users).

## Workflow Descriptions

### 1. **custom_csv_file.yaml**
- **Purpose:** Processes a CSV file and performs sentiment analysis for a specific user.
- **Inputs:**
  - `csv_file` (file): The input CSV file.
  - `user_id` (string): The ID of the user for whom the analysis is performed.
- **Output:** Sentiment analysis results are stored in a database.
- **Use Case:** Ideal for ad-hoc analysis of CSV files uploaded by users.

---

### 2. **mass_instagram_manager_workflow.yaml**
- **Purpose:** Runs daily to fetch all users who have connected their Instagram accounts and generates sentiment analysis reports for each user.
- **Key Features:**
  - Fetches user connections and hashtags from the database.
  - Triggers a **subflow** for each user (`instagram_individual_workflow.yaml`).
- **Trigger:** Scheduled to run daily using a cron job.

#### Subflow: **instagram_individual_workflow.yaml**
- **Purpose:** Fetches Instagram posts for a specific user and performs sentiment analysis.
- **Inputs:**
  - `apify_token` (string): Token to fetch posts from Instagram.
  - `hashtag` (string): Hashtag for which posts are fetched.
  - `numberOfPosts` (string): Number of posts to fetch.
  - `user_id` (string): The ID of the user for whom the analysis is performed.
- **Output:** Sentiment analysis results are stored in a database.
- **Use Case:** Executes as a subflow for individual users in the mass Instagram workflow.

---

### 3. **mass_twitter_manager_workflow.yaml**
- **Purpose:** Runs daily to fetch all users who have connected their Twitter accounts and generates sentiment analysis reports for each user.
- **Key Features:**
  - Fetches user connections and hashtags from the database.
  - Triggers a **subflow** for each user (`twitter_individual_flow.yaml`).
- **Trigger:** Scheduled to run daily using a cron job.

#### Subflow: **twitter_individual_flow.yaml**
- **Purpose:** Fetches tweets for a specific user and performs sentiment analysis.
- **Inputs:**
  - `twitter_access_token` (string): Access token for Twitter API.
  - `twitter_refresh_token` (string): Refresh token for Twitter API.
  - `twitter_token_expires_at` (string): Expiry timestamp of the access token.
  - `hashtag` (string): Hashtag for which tweets are fetched.
  - `number_of_posts` (string): Number of tweets to fetch.
  - `user_id` (string): The ID of the user for whom the analysis is performed.
- **Output:** Sentiment analysis results are stored in a database.
- **Use Case:** Executes as a subflow for individual users in the mass Twitter workflow.

---

## Workflow Relationships

1. **Mass Flows:**
   - `mass_instagram_manager_workflow.yaml` and `mass_twitter_manager_workflow.yaml` are designed to handle multiple users at once.
   - They automatically trigger individual subflows for each user.

2. **Individual Subflows:**
   - `instagram_individual_workflow.yaml` and `twitter_individual_flow.yaml` are the subflows triggered by their respective mass flows.
   - They perform user-specific tasks such as fetching posts/tweets and analyzing sentiments.

---

## Example Workflow Execution

- **Mass Instagram Workflow Execution:**
  - Fetches all Instagram-connected users with valid configurations.
  - Triggers `instagram_individual_workflow.yaml` for each user.

- **Mass Twitter Workflow Execution:**
  - Fetches all Twitter-connected users with valid configurations.
  - Triggers `twitter_individual_flow.yaml` for each user.

---

## License
This repository is licensed under the MIT License. See the `LICENSE` file for details.
