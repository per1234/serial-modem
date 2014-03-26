Introduction
==================

SerialModem is a c++ support library for WCDMA/HSPA modems over a serial interface. Currently supported modems are:
* Cooking Hacks SIM5218E Shield
* MultiTech MTSMC-H5-IP Shield

SerialModem was initially designed for [Arduino](http://www.arduino.cc/) based devices using the [Arduino 3g GPRS/GSM/GPS Shield](http://www.cooking-hacks.com/documentation/tutorials/arduino-3g-gprs-gsm-gps); but has since evolved to support additional modems.

**NOTE**: Unless stated otherwise, the following instructions are specific to [Arduino Uno](http://arduino.cc/en/Main/arduinoBoardUno) board. If you are using other boards, the exact steps may vary.

How to Install the library
==========================

1. Clone the latest source or download the latest zip.
2. Open the Arduino IDE, click `Sketch->Import Library...->Add Library...`, then navigate to `SerialModem/SerialModem`
3. Now you can find SerialModem examples under `File->Examples->SerialModem`
3. Enjoy coding!

Using the SerialModem library with M2X
=========================

You will need to include the libaries main header to gives access to the SerialModem class as well as the needed libraries for M2X using the following example:

```
#include "SerialModem.h"
#include "jsonlite.h"
#include "M2XStreamClient.h"
```

Then before your programs **setup** block add a SerialModem client as well as the M2X StreamClient and provide the SerialModem client to the M2X StreamClient as follows:

```
SerialModemClient client;
char m2xFeedId[] = "feed-id";    // Feed you want to post to
char m2xKey[] = "access-key";    // Your M2X access key
M2XStreamClient m2xClient(&client, m2xKey);

void setup() {
```

Then simply start the modem and point it to the correct APN which is typically done in the microcontroller startup phase using the following code:

```
void setup() {
  ...
  SerialModem.setHardwareInterface(INTERFACE_SIM5218);
  SerialModem.begin(&Serial, 115200);
  while (!SerialModem.ready()) {
    Serial.println("waiting for modem ready");
    delay(2000);
  }

  while (SerialModem.getNetworkStatus() < NETWORK_STATUS_ROAMING) {
    Serial.println("waiting for network registration");
    delay(2000);
  }

  while (!SerialModem.setAPN("m2m.com.attz")) {
    Serial.println("setting APN");
    delay(2000);
  }
  ...
}
```

Now you can continue to use the M2X StreamClient as normal as it will make use of the SerialModem client for it's communications.

LICENSE
=======

This library is released under the MIT license. See [`LICENSE`](LICENSE) for the terms.