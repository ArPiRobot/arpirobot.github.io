# Network Testing

Sometimes network issues can occur and it helps to have certain information about the network conditions when solving them. The main two issues with networking have to do with high latency or poor bandwidth. Often the first step to fixing network reliability issues is to change the network's channel to one with less interference (there are smartphone apps that show which channels nearby networks are on that can be helpful with this). If it still seems like there are issues bandwidth and latency tests can provide helpful information.

## Bandwidth Testing
One easy way to test bandwidth is by using `iperf`. You will need to install [iperf](https://iperf.fr/iperf-download.php) on both the Raspberry Pi and your PC.

You'll need to download the iperf sources and transfer them to the Raspberry Pi and extract the achive. Then, login via ssh (or by using UART consol cable) and run the following commands in the directory the sources were extracted to

```
./configure
make
```
 
Then (still via ssh connection on the Pi) start an iperf server

```
src/iperf3 -s
```

While leaving the ssh connection running (so iperf stays running on the Pi) run an iperf client on your PC.

```
iperf3 -c 192.168.10.1
```

## Latency testing
The easy way is by using ping

```
ping 192.168.10.1
```

It will show the latency in milliseconds.