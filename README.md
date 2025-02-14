# Enabling Telnet on Netgear D1500
The [`telnetEnable`](https://github.com/davejagoda/NetgearTelnetEnable) method doesn't work, although the port is open, the connection fails.

>it's possible to enable it via the .cfg file but you'd have to find a way to decode it
## Using a Serial (USB-to-TTL) Adapter
A serial conection is required to enable Telnet.
You will need a adapter that supports 3.3V.

### Serial Connection Setup
![D1500 Serial](D1500.jpg)

**Pinout (from the top):**
- 3.3V (Do not connect unless needed)
- GND
- TX
- RX
  
I soldered three jumper cables on GND, TX, RX and then connected the pins accordingly on a USB-to-TTL adapter.

- **Router TX → Adapter RX**  
- **Router RX → Adapter TX**  
- **Router GND → Adapter GND** 
  
Then plug the USB-to-TTL adapter into your computer.
## Establishing a Serial Connection 
### 1. Install Necessary Drivers  
Most USB-to-TTL adapters use a **CH340**, **CP210x**, or **FTDI** chip. If your adapter is not recognized, install the appropriate driver:  

- **Windows:** Check **Device Manager** for missing drivers and install them from the manufacturer's website.  
- **Linux/macOS:** These drivers are usually built-in.
    ```sh
    ls /dev/ttyUSB*  # Linux  
    ls /dev/cu.*      # macOS 
    ```
### 2. Identify the Serial Port  
Once the USB-to-TTL adapter is connected, you need to find the correct serial port assigned to it.  

#### **Windows**  
- Open **Device Manager**  
- Expand **Ports (COM & LPT)**  
- Look for a device labeled **USB Serial Port** (e.g., COM3)  

#### **Linux**  
Run the following command:  
```sh
dmesg | grep tty  
```
Look for `/dev/ttyUSB0` or similar
#### **macOS**
```sh
ls /dev/cu.*
```
Look for `/dev/cu.usbserial-xxxxx`.

### 3. Open a Serial Terminal  

#### **Windows: Using PuTTY**  
- Open **PuTTY**  
- Select **Serial**  
- Enter the **COM port** (e.g., COM3)  
- Set **Speed (baud rate) to 115200**  
- Click **Open**  

#### **Linux/macOS: Using screen or minicom**  
- Using `screen`:  
    ```
    screen /dev/ttyUSB0 115200  
    ``` 

- Using `minicom`:  
    ``` 
    sudo minicom -D /dev/ttyUSB0 -b 115200  
    ```

### 4. Access the Router's Console  
- Power on the router.  
- Once booted, press **Enter** in the terminal window.  
- You should see a login prompt:  
    ```
    Username: admin  
    Password:
    ``` 

### 5. Enable Telnet  
Run the following command:  
```
AP# cfgmib set "TELNET_STATE" "0x1"  
```

At this point, Telnet should be enabled, allowing you to connect via the network. 