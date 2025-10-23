# TEC-284-lab17

from gpiozero import LED
import paho.mqtt.client as mqtt
# Set up the LED on pin 12
led = LED(12)
def on_connect(client, userdata, flags, resultCode, properties):
    print("Connected with result code: " + str(resultCode))
    client.subscribe("tLights")
def on_message(client, userdata, msg):
    topic = msg.topic
    payload = msg.payload.decode("utf-8")
    print("Message received:", topic, payload)
    if topic == "tLights":
       tLights(payload) 
def tLights(payload):
    if payload == "1":    
        led.on()
        print("LED on")
    elif payload == "0":
        led.off()
        print("LED off")
# Set up the MQTT client
client = mqtt.Client(mqtt.CallbackAPIVersion.VERSION2)
client.on_connect = on_connect
client.on_message = on_message
# Connect to the MQTT broker (replace with your broker's address)
client.connect("10.1.0.59", 1883)
# Start the MQTT client loop
client.loop_forever()
