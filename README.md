📂 Project Overview
This project automatically organizes files uploaded to an S3 bucket into folders based on their file types (like images, documents, videos, etc.) using AWS Lambda. When a file is uploaded to the S3 bucket, a Lambda function is triggered to sort and move the file to the appropriate folder.

🚀 Features
Automatically organizes files by type (e.g., .jpg, .pdf, .mp4).

Uses AWS Lambda for serverless execution.

Triggered by S3 ObjectCreated events.

Supports custom file type mappings.

Logs execution details to CloudWatch.

🛠️ Architecture
text
Copy code
User Uploads File to S3 Bucket
           |
           v
   S3 Event Notification (ObjectCreated)
           |
           v
      AWS Lambda Function
           |
           v
Organizes File Based on Type (Moves to correct folder)
           |
           v
        CloudWatch Logs
📦 Project Structure
text
Copy code
Automatic-File-Type-Organizer-in-S3
│
├── lambda_function.py        # Lambda function handler
├── requirements.txt          # Python dependencies (if any)
└── README.md                 # Project documentation
⚙️ Prerequisites
AWS Account

IAM Role with:

AmazonS3FullAccess

AWSLambdaBasicExecutionRole

S3 Bucket

🛠️ Setup Instructions
1. Create an S3 Bucket
Go to AWS S3 → Create a new bucket.

2. Create a Lambda Function
Go to AWS Lambda → Create function.

Runtime: Python 3.x

Assign an IAM role with S3 Full Access and CloudWatch Logs permissions.

3. Add S3 Trigger to Lambda
Configure the Lambda trigger for the ObjectCreated (All) event on your S3 bucket.

4. Deploy the Code
Upload your lambda_function.py to AWS Lambda.

5. Test the Setup
Upload files of different types to your S3 bucket.

Check if the files are moved to their respective folders.

📂 File Type Mapping Example
python
Copy code
file_type_mapping = {
    'images': ['.jpg', '.jpeg', '.png', '.gif'],
    'documents': ['.pdf', '.docx', '.txt'],
    'videos': ['.mp4', '.mov', '.avi'],
    # Add more types if needed
}
📈 Monitoring
Go to CloudWatch > Log Groups > /aws/lambda/{Your Lambda Function Name}

Check logs for:

Trigger events

Success and error messages

File movements

🛡️ IAM Policy Example
Make sure your Lambda role has the following permissions:

json
Copy code
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject",
    "s3:PutObject",
    "s3:DeleteObject"
  ],
  "Resource": "arn:aws:s3:::your-bucket-name/*"
}
📚 Useful AWS Resources
AWS Lambda Documentation

Amazon S3 Event Notifications

AWS CloudWatch Logs

✨ Future Enhancements
Add support for additional file types.

Include file size-based categorization.

Send email notifications on file movements.

Build a dashboard for file organization statistics.

🤝 Contribution
Feel free to raise issues or contribute improvements to this project
