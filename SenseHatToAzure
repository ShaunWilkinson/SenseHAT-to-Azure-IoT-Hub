import time
from datetime import datetime

# Importing the sense_emu rather than sense_hat due to hardware failure
from sense_emu import SenseHat
from azure.iot.device import IoTHubDeviceClient, Message

sense = SenseHat()

# Connection string for the IoT HUB
CONNECTION_STRING = 'HostName=ServerEnvironment.azure-devices.net;DeviceId=RaspberryPi;SharedAccessKey=rGTA2Ii03OQB585BTF4jM4hnnHu6BlBShZ/QuqYwjT4='
MSG_TXT = '{{"temperature": {temperature},"humidity": {humidity}}}'

# Returns a connection to the IoT Hub
def client_init():
    client = IoTHubDeviceClient.create_from_connection_string(CONNECTION_STRING)
    return client

try:
    # Initiate a connection with the IoT Hub
    client = client_init()
    
    while True:
        # Get sensor data
        temp = round(sense.temperature)
        humidity = round(sense.humidity)
        
        # Create a formatted Message
        msg_txt_string = MSG_TXT.format(temperature=temp, humidity=humidity)
        message = Message(msg_txt_string)
        
        # Verify whether alert levels have been breached
        # TODO would be better to implement this in the message routing in IoT Hub but can't due to bug
        if temp > 35:
            message.custom_properties["temperatureAlert"] = "true"
        else:
            message.custom_properties["temperatureAlert"] = "false"
            
        if humidity > 75 or humidity < 20:
            message.custom_properties["humidityAlert"] = "true"
        else:
            message.custom_properties["humidityAlert"] = "false"
        
        # Send the message to the IoT Hub
        print("%s Submitting Data -" %datetime.now())
        print("{}".format(message))
        client.send_message(message)
        
        # Wait for 5 seconds before looping
        time.sleep(5)
    
except KeyboardInterrupt:
    print("Cancelled")
