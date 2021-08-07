#### Strategy Pattern ####

##### Initial Design #####
* New requiments: Need the duck to fly

![image](https://user-images.githubusercontent.com/5309726/128587171-e35a8a0f-ad77-4593-9ae0-0553a20ed5df.png)

---

##### 1st Design: Inheritance #####

![image](https://user-images.githubusercontent.com/5309726/128587140-feb191c7-45f8-4ad2-ba60-0a1869eaf135.png)

* Problems:
  * Not all subclasses of Duck should fly
  * Changes can unintentionally affect other ducks

---

##### 2nd Design: Apply interface #####

![image](https://user-images.githubusercontent.com/5309726/128587417-0603c027-065e-4883-b7b9-b171a25a18e8.png)

* Problems:
  * Code is duplicated across subclasses
  * Hard to gain knowledge of all duck behaviors
 
---

##### Final Design #####
* Separating what changes from what stays the same
* Program to interface, not implementations

![image](https://user-images.githubusercontent.com/5309726/128587029-1b20fa82-889f-42d1-9643-ba85b18274ca.png)

---

#### Observer Pattern ####

##### Initial Design #####
* New requiments: Can add or remove as many display elements as they want to the application

![image](https://user-images.githubusercontent.com/5309726/128588314-be17ca6f-ad67-4cce-906a-64214eb224a7.png)

```
public class WeatherData
{
    public void MeasurementsChanged()
    {
        var temp = GetTemperate();
        var humidity = GetHumidity();
        var pressure = GetPressure();

        /*
         * //By coding to concrete implementations, we have no way to add or remove other display elements
         * without making changes to the program
         */
        currentConditionsDisplay.Update(temp, humidity, pressure);
        statisticsDisplay.Update(temp, humidity, pressure);
        forecaseDisplay.Update(temp, humidity, pressure);
    }
}
```

##### Final Design #####
* Define 1 to many relationship between a set of objects

![image](https://user-images.githubusercontent.com/5309726/128589395-a804e9c8-bcea-477c-a701-ec25f25c71f0.png)

```
```

---
