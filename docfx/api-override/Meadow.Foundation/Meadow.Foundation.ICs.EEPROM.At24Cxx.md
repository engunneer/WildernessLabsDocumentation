---
uid: Meadow.Foundation.ICs.EEPROM.At24Cxx
remarks: *content
---

| AT24Cxx       |               |
|---------------|---------------|
| Status        | <img src="https://img.shields.io/badge/Working-brightgreen" style="width: auto; height: -webkit-fill-available;" /> |
| Source code   | [GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/master/Source/Meadow.Foundation.Peripherals/ICs.EEPROM.AT24Cxx) |
| NuGet package | <a href="https://www.nuget.org/packages/Meadow.Foundation.ICs.EEPROM.At24Cxx/" target="_blank"><img src="https://img.shields.io/nuget/v/Meadow.Foundation.ICs.EEPROM.At24Cxx.svg?label=Meadow.Foundation.ICs.EEPROM.At24Cxx" style="width: auto; height: -webkit-fill-available;" /></a> |

The **AT24Cxx** series of chips provide a mechanism for storing data that will survive a power outage or battery failure.  These EEPROMs are available in varying sizes and are accessible using the I2C interface.

### Purchasing

* [DS3231 with integrated EEPROM](https://www.amazon.com/s/ref=nb_sb_noss?url=search-alias%3Daps&field-keywords=ds3231)

### Code Example

```csharp
public class MeadowApp : App<F7Micro, MeadowApp>
{
    public MeadowApp()
    {
        var eeprom = new AT24Cxx(0x57);

        var memory = eeprom.Read(0, 16);
        for (ushort index = 0; index < 16; index++)
            Console.WriteLine("Byte: " + index + ", Value: " + memory[index]);

        eeprom.Write(3, new byte[] { 10 });
        eeprom.Write(7, new byte[] { 1, 2, 3, 4 });
        
        memory = eeprom.Read(0, 16);
        for (ushort index = 0; index < 16; index++)
            Console.WriteLine("Byte: " + index + ", Value: " + memory[index]);
        

        Thread.Sleep(Timeout.Infinite);
    }
}
```

[Sample projects available on GitHub](https://github.com/WildernessLabs/Meadow.Foundation/tree/master/Source/Meadow.Foundation.Peripherals/RTCs.DS1307/Samples) 

### Wiring Example

The chip used to develop this library is one that is available on a common DS3231 RTC module with EEPROM memory module:

<img src="../../API_Assets/Meadow.Foundation.ICs.EEPROM.AT24Cxx/AT24Cxx.svg" 
    style="width: 60%; display: block; margin-left: auto; margin-right: auto;" />