# Automailing-with-alexa
Building an Alexa skill that sends a mail with your resume from an S3 bucket  and generates a cover letter using generative AI involves several steps.


<img width="455" alt="image" src="https://github.com/aadikaa/Automailing-with-alexa/assets/83490101/50e4a98b-257c-4c53-adcf-6b5a7a123980">




# Code used to configure AWS Lambda : 
import boto3

import botocore

import openai

#Set up your AWS credentials for S3 and SES
AWS_ACCESS_KEY_ID = "AKIAT2PGBCRBVOSEJAEB"
AWS_SECRET_ACCESS_KEY = "P2IX3nunnnYVHSCLtPC9eZ1GprLSI4/VAX8f9RrW"
AWS_REGION = "ap-south-1"
S3_BUCKET_NAME = "my-resume2023"
#Set up OpenAI API key
OPENAI_API_KEY = "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"
#Initialize AWS SES client
ses_client = boto3.client('ses', region_name=AWS_REGION)
#Initialize OpenAI API
openai.api_key = OPENAI_API_KEY
def generate_cover_letter():

 # Use the ChatGPT API to generate a cover letter
 response = openai.Completion.create(
 engine="davinci",
 prompt="Generate a cover letter for the job application...",
 max_tokens=150, # Adjust the token limit as needed
 n = 1 # Number of responses to generate
 )
 cover_letter = response.choices[0].text
 return cover_letter
def send_email_with_resume_and_cover_letter():

 # Retrieve the resume from S3
 s3 = boto3.client('s3', aws_access_key_id=AWS_ACCESS_KEY_ID, 
aws_secret_access_key=AWS_SECRET_ACCESS_KEY)
 resume_object = s3.get_object(Bucket=S3_BUCKET_NAME, Key=Suraj - Resume.pdf)
 resume_data = resume_object['Body'].read()
 
 #Generate the cover letter
 cover_letter = generate_cover_letter()
 
 #Compose the email
 subject = 'Job Application'
 sender = "suraj.singhh9968@gmail.com"
 recipient = "surajsingh24@icloud.com"
 email_body = f"Dear Hiring Manager,\n\n{cover_letter}\n\nSincerely,\nYour Name"
 
 #Send the email using SES
 ses_client.send_email(
 Source=sender,
 Destination={'ToAddresses': [recipient]},
 Message={
 'Subject': {'Data': subject},
 'Body': {'Text': {'Data': email_body}}
 }
 )
def lambda_handler(event, context):
 send_email_with_resume_and_cover_letter()
 return {
 'statusCode': 200,
 'body': 'Email sent successfully'
 }


 
<img width="458" alt="image" src="https://github.com/aadikaa/Automailing-with-alexa/assets/83490101/f6c00735-ecd9-4d3e-beb1-4e9eabe0b3b1">




<img width="468" alt="image" src="https://github.com/aadikaa/Automailing-with-alexa/assets/83490101/1e760ef2-1bbb-4660-a8ae-572d5c0689b5">




<img width="460" alt="image" src="https://github.com/aadikaa/Automailing-with-alexa/assets/83490101/89e36bd7-fdc3-492d-84d7-fa47a020ebd3">


