# Temperature Sensor

The Senor Hub support the DS18B20 digital temperature sensors. You can find this sensor in several configurations.  The most common a waterproof configuration.

- [Adafruit](https://www.adafruit.com/product/381)
- [SparkFun](https://www.sparkfun.com/products/11050)
- [Amazon](https://www.amazon.com/s/field-keywords=DS18B20+waterproof)

## Connect Temperature Sensor

The temperature sensor has three wires: Sensor Data (S), Ground (GND), Power (VCC).  Typically the power is red, and the ground is black.  The sensor color varies.  Verify the colors with the sensor documentation.  You need to connect all three to the sensor hub.  

To connect the wires press the wire into the bottom hole below the label.  You can connect the ground and power to any of the GND and VCC ports on the sensor hub.  Select your sensor port and sensor wire.  All sensor ports support the temperature sensor.


![Temperature Connections](/images/sensors/temp_connections.svg)

## Temperature Sensor Configuration

Login in to the Web App, <a href="https://app.automategreen.com" target="_blank">app.automategreen.com</a>.

Navigate to the device page and locate the sensor. Click on the edit icon.

<img src="/images/sensors/click_edit_icon.png" alt="Click edit icon" width="490">

Update the sensor name.  Set the sensor mode to *Temperature Sensor*. Then click *Update Device*.

<img src="/images/sensors/set_mode_temp.png" alt="Set sensor mode to temp" width="490">

It can take several minutes for the temperature to be updated. Once it updates, the sensor overview shows the current temperature.

<img src="/images/sensors/temp_widget.png" alt="Temperature widget" width="490">

<aside class="notice">
  If the temperature does not update within 15 minutes, there is most likely an issue.  Verify the sensor connections are correct and that the Hub is online.
</aside>


## Temperature Sensor Chart

The temperature charts can be used to view the past temperature changes.  The graphs support four type of temperature:

  - Daily average
  - 6-hour average
  - Hourly average
  - 15-minute average
  - Raw Data

To view the temperature chart, click on the chart icon.

<img src="/images/sensors/temp_widget_chart.png" alt="Click temp chart icon" width="490">

The chart displays the hourly average for the current day by default.  Changing the date and type updates the chart.  Click or hover over the points to view the details.

<img src="/images/sensors/temp_chart.png" alt="Temp chart" width="490">

## Temperature Sensor Actions

The temperature sensor can be used to trigger actions based on temperature change.

To view and add the temperature actions, click on the action icon.

<img src="/images/sensors/temp_widget_actions.png" alt="Click temp actions icon" width="490">

To add a new action, click *Add action*

<img src="/images/sensors/add_action.png" alt="Add action" width="490">

There is only a single option for the trigger type.  Click on *temperature change*.

<img src="/images/sensors/action_on_temp_change.png" alt="Action on temp change" width="490">

Once you select the action to perform, you can configure the trigger.  The temperature change trigger support five configurations:

- All temperature changes -  Every time the temperature changes, the action is triggered.
- Temperature change goes above - The action is triggered once when the temperature goes above the set threshold.
- Temperature change goes below - The action is triggered once when the temperature goes below the set threshold.
- Temperature change is above - The action is triggered every time the temperature changes, and the temperature is above the set threshold.
- Temperature change is below - The action is triggered every time the temperature changes, and the temperature is below the set threshold.

<img src="/images/sensors/action_on_temp_change_config.png" alt="Action on temp change config" width="490">

