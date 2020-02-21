---
uid: Meadow.Foundation.RTCs.Ds323x
remarks: *content
---

| DS323x        |               |
|---------------|---------------|
| Status        | Untested      |
| Source code   | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/master/Source/Meadow.Foundation.Peripherals/RTCs.DS323x) |
| NuGet package | Not published |

The **DS323x** is a low-cost and accurate real-time clock with a temperature compensation crystal oscillator. This range of chips offers the following functionality:

* Temperature compensation
* Battery backup
* I2C (DS3231) and SPI (DS3234) interfaces.
* Two programmable alarms
* 32.768 KHz square wave output

## Purchasing

A variety of modules are available including low cost modules with integrated EEPROM:

* [DS3231 with integrated EEPROM](https://www.amazon.com/s/ref=nb_sb_noss?url=search-alias%3Daps&field-keywords=ds3231)
* [Sparkfun DS3234 Breakout board](https://www.sparkfun.com/products/10160)

The following application sets an alarm to trigger at when the current second is equal to 15.  The interrupt routine displays the time and then clears the interrupt flag:

```csharp
public class MeadowApp : App<F7Micro, MeadowApp>
{
    public MeadowApp()
    {
        Ds3231 rtc = new Ds3231(0x68, 100, Device.Pins.D08);
        rtc.ClearInterrupt(DS323x.Alarm.BothAlarmsRaised);
        rtc.SetAlarm(DS323x.Alarm.Alarm1Raised, new DateTime(2017, 10, 29, 9, 43, 15), DS323x.AlarmType.WhenSecondsMatch);
        rtc.OnAlarm1Raised += RtcOnAlarm1Raised;
    }

    static void RtcOnAlarm1Raised(object sender)
    {
        Ds3231 rtc = (Ds3231) sender;
        Debug.Print("Alarm 1 has been activated: " + rtc.CurrentDateTime.ToString());
        rtc.ClearInterrupt(DS323x.Alarm.Alarm1Raised);
    }
}
```

[Sample projects available on GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/master/Source/Meadow.Foundation.Peripherals/RTCs.DS323x/Samples/) 

### Wiring Example

The DS3231 real time clock module (see image below) requires only four (for simple timekeeping) or five (for alarms) connections

<img src="../../API_Assets/Meadow.Foundation.RTCs.DS323x/DS323x.svg" 
    style="width: 60%; display: block; margin-left: auto; margin-right: auto;" />

| Meadow Pin | Sensor Pin        | Wire Color |
|--------------|-------------------|------------|
| 3.3V         | V<sub>cc</sub>    | Red        |
| GND          | GND               | Black      |
| SC           | SCK               | Blue       |
| SD           | SDA               | White      |
| SQW          | Digital Interrupt | Orange     |

The 32K pin outputs the 32,768 Hz clock signal from the module.  This signal is only available when power is supplied by V<sub>cc</sub>, it is not available when the module is on battery power.

The orange wire is only required if the alarms are being used to interrupt the Meadow.