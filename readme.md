KIV for now
Issue with CORS when upload image to s3

🧪 Phase 1: Problem Definition & Architecture
Goal: Recommend 3 skincare products based on user selfie + survey.

Input:

📸 A selfie (face with acne, oily/dry indicators)

📝 Survey: skin type, skin concerns (acne, dryness, pigmentation), budget

Output:

🎯 3 recommended products (from your curated list)

🏗️ Architecture (Simplified E2E Flow)
rust
Copy
Edit
[User Uploads Data]
     |
     +--> [S3] <-- stores image + survey
     |
     +--> [Lambda] --> trigger inference logic
             |
     +--> [SageMaker endpoint]
             |
     +--> [Recommendation logic]
             |
     +--> [Frontend: shows top 3 products]
Optional: Use SageMaker Feature Store to store survey answers for future analysis.

⚙️ Phase 2: Tools & AWS Services
Function	AWS Service
Image Classification Model	SageMaker JumpStart (e.g. ResNet)
Store Image + Survey	Amazon S3
Trigger Inference	Lambda
Store user answers	SageMaker Feature Store
Deploy Static Frontend	S3 + CloudFront
Orchestration / Auth (optional)	API Gateway + Cognito

🛠️ Phase 3: MVP Plan
1. Prepare Your Dataset (for training or testing)
Find a public acne image dataset (e.g., Acne04 Dataset)

Label them (e.g. mild, moderate, severe, clear)

OR use JumpStart pre-trained classification model and skip training for now.

2. Build Survey Form (Static Site on S3)
Fields:

Skin type: [Dry, Oily, Combo, Sensitive]

Skin concerns: [Acne, Redness, Pigmentation, Dryness]

Budget: [<RM50, RM50–RM150, >RM150]

Uploads selfie + JSON-formatted survey to S3

3. Deploy Image Classifier via SageMaker
Option A: Use JumpStart model (e.g. ResNet, MobileNet)

Option B: Fine-tune a smaller model using your dataset (if needed later)

Deploy as real-time endpoint

4. Inference Lambda
Triggered when new selfie + survey uploaded

Calls image classifier endpoint

Reads survey answers

Uses simple rules to select 3 products

python
Copy
Edit
if severity == "moderate" and skin_type == "oily":
    return ["COSRX BHA", "CeraVe Foaming", "La Roche Effaclar"]
5. Frontend to Display Recommendation
Shows product name, image, link to Shopee/Lazada

Use Bootstrap or TailwindCSS for clean UI

🌱 Bonus (Later Phases)
Add Bedrock Claude for product explanation: “Why we recommend this for your skin”

Store survey results to Feature Store for training future ML models

Schedule monthly training with SageMaker Pipelines

Create user login with Cognito

💼 What This Shows in Your Portfolio
✅ Image + text input processing
✅ Integration of S3, Lambda, SageMaker, Feature Store
✅ Frontend skills
✅ Real-world ML use case (commerce + beauty)
✅ Ability to architect full ML pipelines