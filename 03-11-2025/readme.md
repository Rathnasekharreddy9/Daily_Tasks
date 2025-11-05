<img width="1360" height="565" alt="image" src="https://github.com/user-attachments/assets/81915dfa-fb48-430d-ab37-9ce5c4d484bb" />DynamoDB, EC2, API Gateway and Lambda
Assignment Overview
Create a serverless web application that does CRUD  operation on data through an HTML form hosted on EC2, processes it via Lambda functions, and stores it in DynamoDB.
Architecture Components
EC2 Instance: Hosts a static HTML form
API Gateway: REST API endpoint
Lambda Functions: Process form data
DynamoDB: Stores submitted form data
IAM Roles: Secure permissions between services
Detailed Requirements
1. DynamoDB Table Design
Create a table named UserSubmissions with:
Partition Key: submissionId (String)
Attributes: name, email, message, submissionDate, status
2. Lambda Functions
Create two Lambda functions
Submission Lambda:
Triggered by API Gateway POST request
Validates input data
Generates unique submissionId
Stores data in DynamoDB
Returns success/error response
Query Lambda:
Triggered by API Gateway GET request
Retrieves submissions from DynamoDB
Supports querying by email or fetching all records
3. API Gateway
REST API with two resources:
POST /submit → Submission Lambda
GET /submissions → Query Lambda
Enable CORS for EC2 domain
4. EC2 Instance
Launch t2.micro instance with Amazon Linux
Install web server (Apache/Nginx)
Host static HTML form with fields:
Name (text, required)
Email (email, required)
Message (textarea, required)
JavaScript to handle form submission to API Gateway
5. IAM Roles
Lambda execution role with DynamoDB read/write permissions
EC2 instance profile (if needed)
Note: The HTML form should use CSS and Bootstrap.
<img width="1360" height="565" alt="image" src="https://github.com/user-attachments/assets/d24305e5-e848-4995-a70a-3f8966ce584b" />

Creating dynamodb
<img width="1365" height="544" alt="image" src="https://github.com/user-attachments/assets/a0c4233e-1e69-4d97-a1d3-2635811dcb10" />

 
Submission lambda function python code

import json
import boto3
import uuid
from datetime import datetime
from botocore.exceptions import ClientError

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('UserSubmissions')

def lambda_handler(event, context):
    """
    Handles form submissions from API Gateway
    Validates input, generates ID, and stores to DynamoDB
    """
    try:
        # Parse request body
        if isinstance(event.get('body'), str):
            body = json.loads(event['body'])
        else:
            body = event.get('body', {})

        # Extract and validate form data
        name = body.get('name', '').strip()
        email = body.get('email', '').strip()
        message = body.get('message', '').strip()

        # Validate inputs
        validation_errors = []
        if not name:
            validation_errors.append("Name is required")
        if not email or '@' not in email:
            validation_errors.append("Valid email is required")
        if not message:
            validation_errors.append("Message is required")

        if validation_errors:
            return build_response(400, {
                'success': False,
                'errors': validation_errors
            })

        # Generate unique submission ID
        submission_id = str(uuid.uuid4())
        submission_date = datetime.utcnow().isoformat() + 'Z'

        # Prepare item for DynamoDB
        item = {
            'submissionId': submission_id,
            'name': name,
            'email': email,
            'message': message,
            'submissionDate': submission_date,
            'status': 'received'
        }

        # Store in DynamoDB
        table.put_item(Item=item)

        return build_response(201, {
            'success': True,
            'submissionId': submission_id,
            'message': 'Submission received successfully'
        })

    except ClientError as e:
        print(f"DynamoDB Error: {e}")
        return build_response(500, {
            'success': False,
            'error': 'Database error occurred'
        })

    except Exception as e:
        print(f"Unexpected Error: {e}")
        return build_response(500, {
            'success': False,
            'error': str(e)
        })


def build_response(status_code, body):
    """Helper to build consistent API responses"""
    return {
        'statusCode': status_code,
        'headers': {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*'
        },
        'body': json.dumps(body)
    }
<img width="1348" height="481" alt="image" src="https://github.com/user-attachments/assets/42892a53-1ab1-40d6-b2ad-d8a656061e56" />

Query lambda code:
import json
import boto3
from botocore.exceptions import ClientError

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('UserSubmissions')

def lambda_handler(event, context):
    """
    Retrieves submissions from DynamoDB
    Supports filtering by email or fetching all records
    """
    try:
        query_params = event.get('queryStringParameters', {}) or {}
        email = query_params.get('email')

        if email:
            # Query by email using GSI if available
            try:
                response = table.query(
                    IndexName='EmailIndex',
                    KeyConditionExpression='email = :email',
                    ExpressionAttributeValues={':email': email}
                )
            except Exception:
                # Fallback to scan with filter if GSI not configured
                response = table.scan(
                    FilterExpression='email = :email',
                    ExpressionAttributeValues={':email': email}
                )
        else:
            # Get all submissions (limit for performance)
            response = table.scan(Limit=100)

        items = response.get('Items', [])
        items.sort(key=lambda x: x.get('submissionDate', ''), reverse=True)

        return build_response(200, {
            'success': True,
            'count': len(items),
            'submissions': items
        })

    except ClientError as e:
        print(f"DynamoDB Error: {e}")
        return build_response(500, {
            'success': False,
            'error': 'Database query failed'
        })

    except Exception as e:
        print(f"Unexpected Error: {e}")
        return build_response(500, {
            'success': False,
            'error': str(e)
        })


def build_response(status_code, body):
    """Helper to build consistent API responses"""
    return {
        'statusCode': status_code,
        'headers': {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*'
        },
        'body': json.dumps(body)
    }




API GATEWAY
<img width="1365" height="596" alt="image" src="https://github.com/user-attachments/assets/4283e43d-6e7c-47ef-8a36-bf0bc7ad76a3" />



    Output
    <img width="1365" height="544" alt="image" src="https://github.com/user-attachments/assets/62a0f3d6-51dd-4fac-80b7-8a000d6807da" />
<img width="700" height="337" alt="image" src="https://github.com/user-attachments/assets/e9d8f0d6-6cbf-431e-8f3c-58a491803359" />

