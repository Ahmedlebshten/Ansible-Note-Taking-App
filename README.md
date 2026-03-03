# 📝 Ansible AWS Note-Taking App:

## 📌 Overview
This project is a **DevOps demonstration** that automates the deployment of a simple **Flask-based Note-Taking App** on an **AWS EC2 instance** using **Ansible**.

-----

📦 Technologies Used
- Application: Flask (Python) + SQLite database
- Configuration Management: Ansible
- Cloud Provider: AWS (EC2)
- Purpose: Show how to deploy, configure, and manage an application in the cloud using Infrastructure as Code (IaC).

-----

## 🏗️ Project Structure
```
ansible_aws_note_app/
│
├── ansible/
│   ├── ansible.cfg               # Ansible configuration file
│   ├── aws_ec2.yml               # Dynamic AWS EC2 inventory plugin
│   ├── site.yml                  # Main playbook
│   │
│   └── roles/
│       └── note_app/
│           ├── files/
│           │   └── app.py        # Flask application code
│           │
│           ├── tasks/
│           │   └── main.yml      # Deployment tasks
│           │
│           ├── handlers/
│           │   └── main.yml      # Handlers (if needed)
│           │
│           ├── defaults/
│           │   └── main.yml      # Default variables
│           │
│           ├── vars/
│           │   └── main.yml      # App-specific variables
│           │
│           └── meta/
│               └── main.yml      # App meta data
│
├── README.md                    # Project documentation
└── .gitignore                   # Secrets files
```
____

## ⚙️ Requirements:

- AWS account with access keys (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`)
- IAM user with permissions:
  - `ec2:DescribeInstances`
  - `ec2:StartInstances`
  - `ec2:StopInstances`
- Security group allowing:
  - Port `22` (SSH)
  - Port `5000` (Flask app)
- Installed locally (on Controller machine):
  - `python3`
  - `pip3`
  - `ansible`
  - `boto3`, `botocore` (for AWS dynamic inventory)

____

## 🚀 Deployment Steps:

### 1️⃣ Configure AWS Credentials
- Create file:
```
mkdir -p ~/.aws
nano ~/.aws/credentials
```
____

- Content:
```
[default]
aws_access_key_id = YOUR_ACCESS_KEY
aws_secret_access_key = YOUR_SECRET_KEY
region = us-east-1
```
____

- Clone the Repository:
```
git clone https://github.com/Ahmedlebshten/ansible-note-taking-app.git
cd ansible-note-taking-app/ansible
```
____

- Run the Playbook:
```
ansible-playbook -i aws_ec2.yml site.yml
```
____

- This will:
  
- Connect to EC2 instance(s) tagged as ansible_agent01
- Install Python3, pip, SQLite
- Install Flask
- Deploy app.py

____

- Start the Application:

- SSH into the EC2 instance:
```
ssh -i ansible.pem ec2-user@<public-ip>
```
____

- Run Flask app:
```
python3 /home/ec2-user/app.py
```
____

- Access it in your browser:
```
http://<EC2_PUBLIC_IP>:5000
```
____

- 📦 Database:
  
- Using SQLite (notes.db)
- Table: notes
- Fields:
- id (Primary Key, Integer)
- content (Text)
- created_at (Timestamp)

____

- To explore DB inside instance:
```
sqlite3 /home/ec2-user/notes.db
sqlite> .tables
sqlite> .schema notes;
sqlite> SELECT * FROM notes;
```
____

## 📦 Install role via Ansible Galaxy:
You can install this role from Ansible Galaxy with:
```
ansible-galaxy install Ahmedlebshten.ansible-note-app-role
```
Then call it in your playbook:
```
- hosts: my-ec2
  roles:
    - role: Ahmedlebshten.ansible-note-app-role
```

