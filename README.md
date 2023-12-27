# Door-Status-Monitoring-with-Door-Sensor-and-Twilio
 Sending SMS notifications through Twilio when the door is opened.
from twilio.rest import Client
import RPi.GPIO as GPIO
import time

door_sensor_pin = 17

GPIO.setmode(GPIO.BCM)
GPIO.setup(door_sensor_pin, GPIO.IN)

account_sid = 'your_account_sid'
auth_token = 'your_auth_token'
client = Client(account_sid, auth_token)

def send_sms_notification():
    message = client.messages.create(
        body='Door opened!',
        from_='+1234567890',  # Your Twilio phone number
        to='+9876543210'  # Recipient's phone number
    )
    print(message.sid)

try:
    while True:
        door_status = GPIO.input(door_sensor_pin)

        if door_status == GPIO.HIGH:
            send_sms_notification()
            time.sleep(60)  # Send notification once per minute to avoid spam
        else:
            time.sleep(1)
except KeyboardInterrupt:
    GPIO.cleanup()
