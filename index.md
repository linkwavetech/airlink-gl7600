# GL7600 with Windows 10

The Sierra Wireless GL7600 modem is a compact, LTE CAT-1 external modem, featuring both USB and serial ports. Connection with the serial port is straightforward, providing access to the AT command interface, and facilitating the establishment of PPP over serial data connections.

Use of the USB port on the GL7600 is dependent on operating system and use case. By default, the GL7600 presents three CDC-ACM ports and four CDC-NCM ports over USB. One of the ACM ports is identified as an “IMC Mobile Phone Modem”. Checking modem properties for this device show it to be COM35 in this case.
 
![Device Manager](img/DEVMGR.png)

USB endpoint composition is configured using the AT+KUSBCOMP command. By default, this is set to 0.

![Parameter](https://github.com/linkwavetech/airlink-gl7600/blob/master/Images/2PARAMETER.png)

To use the GL7600 with Windows 10, the simplest way is to change the USB endpoints to support MBIM (Mobile Broadband Interface Model). MBIM is natively supported by Windows 8 and Windows 10. Windows 10’s connection manager will recognise an MBIM device and control the connection accordingly.

To set up the GL7600 for MBIM, connect to an AT command port using Teraterm or similar. In this example, this can be COM35 (the modem), COM37 of the hardware serial port on the GL7600. By default, this is set to 115200bps, 8 bit, no parity, no handshaking.

Type:		 AT <Enter>
 
Response:	OK

Type: 		AT+USBCOMP=2 <Enter>
 
Response:	OK

Type:		 AT+CFUN=1,1 <Enter>
 
Response:	OK


The module will then reboot. Device Manager will then show
 
 ![Device Manager after reboot](https://github.com/linkwavetech/airlink-gl7600/blob/master/Images/3DEVMAN.png)
 
There is now only one device associated with the USB port, the CDC NCM/MBIM device. This is all that is needed to control the GL7600 under Windows 10. The AT Command Manual for the GL7600 (HL7692) suggests that serial (ACM) endpoints should be available, but this does not happen in Windows 10.

Opening the Network list in Windows 10 now shows a button for “Cellular”, together with an entry showing the cellular network that the GL7600 is attached to. 
 
  ![Networks](https://github.com/linkwavetech/airlink-gl7600/blob/master/Images/4NETWORKS.png)
 
It is good practice at this stage to add APN details manually, to ensure that the correct APN for the SIM is being used. Go to “Network and Internet Settings”, select “Cellular” and then “Advanced Settings”. Click “Add an APN”, fill in the relevant details for your APN and Save. If there is a username and password associated with your APN, it is likely that you will need to set “Type of sign-in info” to PAP or CHAP. 
 
![APN](https://github.com/linkwavetech/airlink-gl7600/blob/master/Images/5APN.png)

Once saved, this profile should be applied to the cellular connection by Windows. Go back to the Network list and turn off Wi-Fi if enabled. If the cellular connection has not established, toggle the Cellular button. 
 
![Cellular 2](https://github.com/linkwavetech/airlink-gl7600/blob/master/Images/5NETWORK.png)

 The cellular connection is now established.

Note that, as no CDC-ACM endpoints are available, changing the USB endpoint setting requires a connection to be made to the GL7600 using the physical serial port. Once connected, 

Type:		 AT <Enter>
 
Response:	OK

Type: 		AT+USBCOMP=0 <Enter>
 
Response:	OK

Type:		 AT+CFUN=1,1 <Enter>
 
Response:	OK
 
 
The GL7600 will reboot and the USB endpoint settings will be default.
