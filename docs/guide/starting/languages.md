# ArPiRobot Programming Languages

This section is about the programming language that will be used to write the robot program that will run on the Raspberry Pi. Before you setup your PC, it would be a good idea to know which programming language you want to use to program your robot as there are different setup tasks for each language.

In general, it does not matter too much which language you use for robot code, as the same core library is used in all cases. Therefore, the biggest thing you should consider is familiarity with the language. If you're new to programming, it is recommended to use python as it is easy to learn and uses simpler syntax than many other languages.

In addition to familiarity with the language you should consider the complexity of setting up to use the language. More details can be found in the [Setup your Computer](./devsetup.md#other-tools) section.

In general, C++ will yield the best performance (if you have a lot of computationally demanding or multi-threaded user code), but Python will be the easiest to setup and use.

The difference, though will not often be noticeable as user code is not generally going to be performing computationally demanding tasks (this usually happens in the builtin library, which is the same for all languages). If your robot code will be running multiple computationally demanding threads, python may not be the best choice because of the [GIL](https://wiki.python.org/moin/GlobalInterpreterLock). In this case C++ would work better, but in most cases there is not much difference.

