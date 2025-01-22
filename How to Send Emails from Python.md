This guide explains how to send emails using Python. The steps and examples are based on information from [this video](https://www.youtube.com/watch?v=g_j6ILT-X0k&ab_channel=ThePyCoach).
##### **Generate an App Password**
1. Log in to your Gmail account.
2. Go to **Google Account Settings** > **Security**.
3. Enable **2-Step Verification** (if not already enabled).
4. Under **App Passwords**, generate a new password:
    - Select **Mail** as the app.
    - Select your device type.
5. Copy the generated password (e.g., `abcd efgh ijkl mnop`).
##### **Write and Test the Python Code**
Use the following Python code to send a test email:
```python
import os
from email.message import EmailMessage
import smtplib
import ssl

# Email configuration
EMAIL_SENDER = "daniellopezcano13@gmail.com"
EMAIL_PASSWORD = "xhiu wcqs tsnt gszn"
EMAIL_RECEIVER = "daniellopezcano13@gmail.com"

# Email content
subject = "Test Email"
body = """
This is a test email sent from Python.
If you're reading this, the email setup works correctly!
"""

# Create email message
em = EmailMessage()
em["From"] = EMAIL_SENDER
em["To"] = EMAIL_RECEIVER
em["Subject"] = subject
em.set_content(body)

# Send email
try:
    context = ssl.create_default_context()
    with smtplib.SMTP_SSL("smtp.gmail.com", 465, context=context) as smtp:
        smtp.login(EMAIL_SENDER, EMAIL_PASSWORD)
        smtp.sendmail(EMAIL_SENDER, EMAIL_RECEIVER, em.as_string())
        print("Test email sent successfully!")
except Exception as e:
    print(f"Failed to send email: {e}")

```
##### **Testing the Script**
1. Save the code in a file, e.g., `test_send_email.py`.
2. Run the script:
```bash
    python test_send_email.py
```
3. Check your inbox. You should receive an email with the subject "Test Email".