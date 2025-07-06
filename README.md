# S3 Automatic File Organizer with AWS Lambda and SNS Notifications

This project automatically organizes files uploaded to an Amazon S3 bucket based on their file type using AWS Lambda. It also sends an SNS email notification whenever a file is moved.

## ✅ Project Workflow

1. **File Upload to S3 Bucket**
2. **S3 Event Trigger activates Lambda Function**
3. **Lambda Function categorizes file into appropriate folder:**
   - `images/` for image files (.jpg, .jpeg, .png)
   - `documents/` for document files (.pdf)
   - `spreadsheets/` for spreadsheet files (.xlsx, .xls, .csv)
   - `others/` for all other file types
4. **Lambda moves the file to the categorized folder.**
5. **SNS Email Notification is sent confirming the file movement.**

---

## 🛠️ AWS Services Used
- Amazon S3
- AWS Lambda
- Amazon SNS
- Amazon IAM
- Amazon CloudWatch (for logs)

---

## 📂 Folder Structure in S3
darwin-file-organizer-bucket/
│
├── images/
│ └── *.jpg, *.jpeg, *.png
├── documents/
│ └── *.pdf
├── spreadsheets/
│ └── *.xlsx, *.xls, *.csv
└── others/
└── other files

python
Copy code

---

## 🧩 Key Components

### ✅ Lambda Function Code:
```python
import boto3
import os

s3 = boto3.client('s3')
sns = boto3.client('sns')

SNS_TOPIC_ARN = 'arn:aws:sns:us-east-1:913524935515:S3FileNotification'

def lambda_handler(event, context):
    bucket_name = event['Records'][0]['s3']['bucket']['name']
    file_key = event['Records'][0]['s3']['object']['key']

    print(f"Copying {file_key} to organized folder...")

    file_extension = file_key.split('.')[-1].lower()

    if file_extension in ['jpg', 'jpeg', 'png']:
        folder = 'images/'
    elif file_extension in ['pdf']:
        folder = 'documents/'
    elif file_extension in ['xlsx', 'xls', 'csv']:
        folder = 'spreadsheets/'
    else:
        folder = 'others/'

    new_key = folder + os.path.basename(file_key)

    try:
        s3.copy_object(Bucket=bucket_name, CopySource={'Bucket': bucket_name, 'Key': file_key}, Key=new_key)
        s3.delete_object(Bucket=bucket_name, Key=file_key)
        message = f'File {file_key} moved to {new_key}'
        print(message)
        sns.publish(TopicArn=SNS_TOPIC_ARN, Message=message, Subject='S3 File Organizer Notification')
    except Exception as e:
        print(f"Error occurred: {str(e)}")

    return {'statusCode': 200, 'body': 'File organized successfully.'}
✅ Steps to Deploy
1. Create an S3 Bucket
Name: darwin-file-organizer-bucket

2. Create an SNS Topic
Name: S3FileNotification

Subscribe your email to this topic.

3. Create IAM Role with Permissions:
AmazonS3FullAccess

AmazonSNSFullAccess

AWSLambdaBasicExecutionRole

4. Create the Lambda Function
Runtime: Python 3.12

Attach the above IAM role.

5. Add Trigger to Lambda
Trigger: S3

Bucket: darwin-file-organizer-bucket

Event type: All object create events

6. Test the Workflow
Upload files to your S3 bucket.

Check if files are automatically moved to correct folders.

Check your email for SNS notifications.

✅ Architectural Diagram

Diagram Summary:

File upload to S3 → Triggers Lambda → Organizes file → Sends SNS email

✅ Conclusion
This project helped automate file organization in S3 using Lambda and improved notification workflows using SNS.

You can extend this project further by:

Adding support for other file types.

Adding CloudWatch alarms for failures.

Using S3 bucket versioning.

🔗 Author
Darwin | AWS Cloud Resource


🤝 Contribution
Feel free to raise issues or contribute improvements to this project
