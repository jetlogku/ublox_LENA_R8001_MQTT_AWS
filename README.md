# ublox_LENA_R8001_MQTT_AWS
I’m seeking assistance from u-blox or others regarding this issue. I am using Adafruit Itsybitsy M4 and LENA R8001 to publish MQTT data to AWS IoT Core
I am currently experiencing an MQTT error:+UMQTTER:13,4 which occurs during the `AT+UMQTTC=1` connection attempt
## 🧰 Hardware

- **Feather Itsybitsy M4** (ATSAMD51)
- **u-blox LENA-R8001M10** Cat 1 bis modem. Firmware:02.00,A01.40
- SIM card with LTE access (APN: `xxxxnet`)
- Connected via `Serial1` at 9600 baud
## 🧪 Software
- Custom sketch using AT commands
- MQTT/TLS setup via AT commands (no library)
- M-centre for uploading certs and key
- TLS Profile 0 configured with:
  - Root CA: `lena_ca`
  - Client Cert: `lena_cc`
  - Private Key: `lena_pk`---

## 📡 AWS IoT Configuration

- Thing name: `Feather_Lena`
- MQTT Endpoint: `xxxxxxxxxx-ats.iot.us-east-1.amazonaws.com`
- Port: `8883`
- Client ID: `Feather_Lena`
- TLS: Enabled with security profile 0

- security Profile configuration
AT+USECPRF=0,0,1  // set validation level 1.
AT+USECPRF=0,1,3 //set TLS ver 1.2.
AT+USECPRF=0,2,99,C0,30 // set cipher suite.
AT+USECPRF=0,3,"lena_ca"  // set CA.
AT+USECPRF=0,5,"lena_cc"  //set CC.
AT+USECPRF=0,6,"lena_pk"  //set Pk.
AT+USECPRF=0,10," sni_endpoint"  //set SNI.

MQTT configuration
AT+UMQTT=0," Feather_Lena"  //client ID.
AT+UMQTT=2," endpoint"," 8883"  //endpoint and port.
AT+UMQTT=4,"",""  // No username and password.
AT+UMQTT=6,1  //Qos set to 1.
AT+UMQTT=10,3600,60  // keep alive.
AT+UMQTT=11,1,0   // use Profile ID 0.
AT+UMQTT=12,1 //clean session.
AT+UMQTT=20,1  // use pdp context 1.

Issue Summary
MQTT Setup (AT+UMQTT) works fine

TLS Profile 0 configured with valid cert/key

PDP context activated (IP assigned)

AT+UMQTTC=1 fails with:+UMQTTER: 13,4







