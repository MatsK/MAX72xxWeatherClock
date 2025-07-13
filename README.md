# MAX72xx Weather Clock a.k.a. Marquee Scroller
Forked from [https://github.com/rob040/LEDmatrixClock](https://github.com/rob040/LEDmatrixClock)

## Features:
* Show accurate time that is syncronised with NTP ( Internet Time Servers)
* Display Local Weather and conditions (refreshed every 10 - 30 minutes, configurable)
* Configured through Web Interface
  * Basic Authorization to access Configuration web interface
  * Scroll speed and LED brightness
  * Data display and data update interval frequency
  * Sleep / wake times
  * Number of LED-matrix tiles (4 to >8) (compile option) and
    types of Clock Displays on larger panels, e.g. also display seconds or temperature
  * Change number of display modules, without the need to re-compilation (device will reboot upon change)
  * Update firmware through web interface over WiFi (OTA)

## Feature Enhancements
* Automatic timezone (from Local weather), hence no need for TimeZoneDB registration requirement
* Added Time NTP and more efficient time strings instead. Actual time zone information is used from OpenWeather API
* Update to new OpenWeatherMap.org API. Newly requested Free Service API-keys can no longer use the older call and structure
* Reduce RAM usage. The ESP8266 is an older WiFi processor with limited RAM (80kB), especially when compared to its newer ESP32 members
* Added MQTT support with basic Authentication, to send message to be displayed. Message is displayed immediately when display shows the time, and repeated every minute with default configuration
* Use VScode IDE with PlatformIO / PIOarduino, for better development and maintenance experience and much better build environment and library version control
* Improved time synchronisation and weather data update at start-up
* Improved weather data display on webpage (minor changes)
* Added a favicon for easy recognition in your browser
* Using the 'LittleFS' filesystem in stead of the depecated 'SPIFFS' filesystem
* Using ArduinoJson upgraded to version 7
* Webpage now has switchable dark-mode view
* Webpage now has switchable automatic page update
* Instead of scrolling text, there is also a static display mode for short messages, date, temperature or humidity only next to the time display

### Known issues
* Webpage update does halt the scrolling display for a moment. This cannot be prevented, only shortened with optimizations, such as reduction of 'String' usage.
* Scrolling text appears to have some 'flex' in it, due to variations in display data update. This is being worked on.
* When using the LED display at lowest intensity, some pixel flicker might be visible. It varies with display module hardware. This is being worked on.

## Parts:
| Part                        |          |
| ---                         | ---      |
| ESP8266 (ex. Wemos D1 Mini) | Required |
| MAX7219 LED Matrix Module   | Required |
| USB Power supply            | Required |
| Case                        | Optional |

## Wiring the Wemos D1 Mini to the MAX7219 LED Dot Matrix Display

| MAX7219 | D1 mini          | Comment     |
| ---     | ---              | ---         |
| VCC     | 5V               | + 5 volt    |
| GND     | G (GND)          | Ground      |
| CLK     | D5 (SCK)         | Clock       |
| CS      | D6               | Chip Select |
| DIN     | D7 (MOSI)        | Data In     |

## Compiling and Loading to ESP8266 (Wemos D1 Mini)

### Building with PlatformIO.

Use [**VScode**](https://code.visualstudio.com/docs) with [**PlatformIO**](https://platformio.org/) or better, its desendant fork [**PIOarduino**](https://marketplace.visualstudio.com/items?itemName=pioarduino.pioarduino-ide) extension.

Please refer to the provided links on how to install VScode on your OS.

Then, open this project with `Visual Studio Code`, via File --> Open Folder: select folder containing `platformio.ini` file.

The `platformio.ini` file contains the references to the required external libraries and version numbers.

To build, open the `pioarduino` or `platformio` extension (icon on left hand side bar), then under *PROJECT TASKS* -> *Default* -> *General* : select **Build** and then **Upload**

### Initial Configuration
Editing the **Settings.h** file is totally optional and not required.

All settings and API Keys are managed from the Web Interface.

* Open Weather Map free service API key: http://openweathermap.org/ -- Open Weather Map is used to get weather data and the current time zone from the selected City. This API key is required for correct time.

**NOTE:** The settings in the Settings.h are the default settings for the first loading. After loading you will manage changes to the settings via the Web Interface. If you want to change settings again in the settings.h, you will need to erase the file system on the Wemos or use the `Reset Settings` option in the Web Interface.

## Web Interface
The project uses the **WiFiManager** so when it can't find the last network it was connected to, it will become an **Access Point Hotspot** -- connect to it with a web browser at http://192.168.4.1/ and you can then enter your WiFi connection information.

After connected to your WiFi network it will display the IP address assigned to it and that can be used to open a browser to the Web Interface. Now you will be able to manage your API Keys through the web interface.

The default user / password for the configuration page is: `admin / password`

The Clock will display the local time of the City selected for the weather after a short synchronization period.

## Contributors
David Payne <br>
Nathan Glaus <br>
Daniel Eichhorn -- Author of the TimeClient class (in older versions) <br>
yanvigdev <br>
nashiko-s <br>
magnum129 <br>
rob040  --  author of this repo and these [feature enhancements listed above](#feature-enhancements)<br>

Contributing to this software is warmly welcomed. You can do this basically by forking from master, committing modifications and then making a pulling requests against the latest DEV branch to be reviewed (follow the links above for operating guide). Detailed comments are encouraged. Adding change log and your contact into file header is encouraged. Thanks for your contribution.

When considering making a code contribution, please keep in mind the following goals for the project:
* User should not be required to edit the Settings.h file to compile and run. This means the feature should be simple enough to manage through the web interface.
* Changes should support ESP8266.


## Using Arduino 2.x IDE

It is no longer recommended to use the Arduino IDE. It might be difficult to get the right library versions together, especially when using it also for other Arduino projects.

Still, the source directory structure is kept such that this project can be build with little to no effort. This might change in the future.
* Support for ESP8266 Boards is included in Arduino v2.x
* Select Board:  "ESP8266" --> "LOLIN(WEMOS) D1 R2 & mini"
* Set Flash Size: 4MB (FS:1MB OTA:~1019KB)
* Select the **Port** from the tools menu.

### Loading Supporting Library Files in Arduino

Use the Arduino guide for details on how to installing and manage libraries https://www.arduino.cc/en/Guide/Libraries

**Packages** -- the following packages and libraries are used (download and install):
| Library                 | Link to source                                   | Comment              |
|------------------------ | ------------------------------------------------ | -------------------- |
| <WiFiManager.h>         | https://github.com/tzapu/WiFiManager             | Latest               |
| <TimeLib.h>             | https://github.com/PaulStoffregen/Time           |                      |
| <Adafruit_GFX.h>        | https://github.com/adafruit/Adafruit-GFX-Library |                      |
| <Max72xxPanel.h>        | https://github.com/markruys/arduino-Max72xxPanel |                      |
| <JsonStreamingParser.h> | https://github.com/squix78/json-streaming-parser | To be discarded soon |
| <ArduinoJson>           | https://github.com/bblanchon/ArduinoJson         | v7.4.2+              |
