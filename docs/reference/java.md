# Java Programming 

Note: The API generally matches the PythonLib. The same concepts and functions apply. The function names are the main thing that is different. In Python, function names are generally `snake_case`, whereas in Java they are generally `camelCase`.

For example:

| Python Name           | Java Name           |
| --------------------- | ------------------- |
| robot_started         | robotStarted        |
| enabled_periodic      | enabledPeriodic     |

Sometimes in Python properties are also used. For example

```python
sensor = MySensor()
position = sensor.value
```

In Java this would use a function called `getValue` instead of a property called `value`

```java
MySensor sensor = new MySensor();
int position = sensor.getValue();
```

More info coming soon...