# ArPiRobot Programming Languages

This section is about the programming language that will be used to write the robot program that will run on the Raspberry Pi. There are currently a couple of options provided

## Python
The Python Library (ArPiRobot-PythonLib) was the main focus of this project for many reasons:

- It is an easy to learn language, which means it is easy for beginners to understand and use
- It is widely used in the Raspberry Pi community
    - Many Raspberry Pi users may already be familiar with it
    - Skills learned with the robot framework are applicable to other Raspberry Pi projects
- Good device support
    - Many devices have "official" demo code written in Python. This makes devices easy to integrate
    - There are various I/O libraries available to work with
- It is not a compiled language
    - While this does mean that programs are slower, it makes it easier to use (the primary goal of this project is ease of use not to make everything as efficient as possible)
    - No cross compilation.

For most use cases Python is not an issue and will be the preferred language for ArPiRobot robots. However, there are some limitations. In addition to the fact that it is an interpreted language (which means it is often slower than compiled languages and can't take advantage of compile-time optimizations), there is the problem of the Global Interpreter Lock (GIL). Simply put, the GIL means it is harder to make two things run at the same time, even if your device's CPU has multiple cores (like the Raspberry Pi 3A+). For many use cases this will never be an issue, however for more complex robots it could. Some work has been done with the ArPiRobot-PythonLib to work around this problem in some common scenarios (such as repeated math), but it is far from perfect. This is why the JavaLib exists.

## Java

There is no equivalent to the GIL in Java, which means that it is easier to take advantage of available resources on multi-core devices, but that is true of many other languages as well. So why Java?

- It is compiled, but targets a runtime
    - Java code is compiled for a "Java machine" instead of for a specific kind of device. Then, on any device that will run the code there is a "Java Virtual Machine" or "Java Runtime" that is used to run the Java code. 
    - This means you can compile the Java program on one type of computer (a laptop running Windows for example) and run the compiled program on any type of computer (such as a Raspberry Pi running Linux) as long as a Java Runtime is installed.
    - Java is a "compile once, run anywhere" language
    - Since it is compiled, this also means that compile-time optimizations can be used, but there is still no need to worry about cross compilation.
- It is widely used (Android apps, PC programs, server backends, etc) and is a useful language to learn
- Existing bindings to I/O libraries for the Raspberry Pi
    - This means it is easy to support the same kinds of devices as are supported by the Python Library


## Others

Currently no work has been done to support other languages. C# has been looked into as an additional language (possibly a replacement for the Java library eventually), however there is no C# library implemented at this time.

Many people may be wondering why C or C++ is not in use. This comes down to the ease of use and education use goals. C/C++ are not easy to use. Part of this is due to the low-level nature of the language and part of this is because cross-compilation is involved. There's no technical reason a C/C++ library could not be implemented, it just doesn't align well with the goals of this project. 

This also leads to the question: Why not implement everything in C/C++ and wrap it in high level languages (Python, Java, C#, etc).? Wouldn't this make things more efficient and make it easier to maintain?

The answer to both of these last questions is yes, however there is the "education use" goal. This framework is designed not only to be easy to use for education projects, but to be easy to understand if users want to dive deeper and look "under the hood". Implementing everything in the language that is used to write the robot code makes it easier for users to learn from the code and understand what is happening if they are interested.