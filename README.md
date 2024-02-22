# Watchdog : AI-Powered DDoS Protection Engine

**Watchdog** is an AI-powered DDoS protection engine that analyzes your network packets and notifies you about suspicious addresses connected to your network.

We tested the **Watchdog** engine in one of the largest CDN providers in Iran, which handles 10% of the country's traffic. Initially, the traffic was monitored by humans, but it was so inefficient that we had to create an AI-based solution. It took us about two years to make the project ready to launch. Now, we have decided to launch this project as a free DDoS protection engine.

**Watchdog** system analyzes network behavior through server logs. When deviations from normal patterns are detected, the system identifies and notifies the server about suspicious activities. For technical inquiries or further clarification, see **Contact** section.

# Request
**Watchdog** listens on **5.34.195.41:9001** & uses Transport Layer Security (TLS) to establish a secure connection. Packets gets constructed based on **serialization**.

## Packet Header Data Structure

|Field Size		|Description   |Comment  			  					        |
|---------------|--------------|------------------------------------------------|
| 2				| Length	   | Sequence + Checksum + Timestamp + Payload		|
| 4				| Sequnence    | Packet Sequence Number 			        	|
| 8				| Checksum     | First 8-bytes of SHA2-256 of **Payload**    	|
| 8				| Timestamp	   | Timestamp                                      |
| ?				| Payload	   |                                                |

> **Length**, **Sequence** and **Timestamp** should be little-endian.

> The maximum size of a packet is 65535 - 12. It's worth noting that the **Sequence** header is non-functional and can be filled with random data.

## Payload Header Data Structure
|Field Size		|Description    |Comment  		    	                           |
|---------------|---------------|--------------------------------------------------|
| 2				| Layer	   		| Supports 7th Layer (0x0700)‌, 4th Layer (0x0400) and 3th Layer (0x0300) |
| ?				| Packet Info   |                                                  |

## Packet Info Data Structure
|Field Size|Description     |
|----------|----------------|
| 4		   | Source IPv4    |
| 2	       | Source Port    |
| 4		   | Dest IPv4      |
| 2		   | Dest Port	    |
| 8		   | Unix-Timestamp |
| 2		   | Request Length |
| ?		   | Request	    | 

> **Request Length** & **Request** can get ignored while sending 4th & 3th layer requests by setting the **Request Length** to **0x00**. You can also append multiple **Packet Info** & send them together.

# Response
The **Packet Header** data structure is the same for both the request and the response. The response payload contains a 4-byte data that indicates a suspicious IPv4 address that is connected to your network. If the data structure is invalid, the connection will be closed.

# WatchNet
**WatchNet** feeds data to **Watchdog**. It receives requests from servers and append each server's IPv4 address to it's requests, that's how **Watchdog** identifies each server. The data is stored for 4 to 5 days and then deleted, It is not used for any profitable purposes or shared with third parties.

> **WatchNet** is undergoing daily updates to improve its functionality. As a result, it may experience brief downtimes lasting a few seconds. While Erlang has built-in support for **Hot Code Swapping**, this feature has not been implemented in **WatchNet** at this time.

# Roadmap

Our mission is to continuously enhance and improve the project to better meet the needs of our users. We are committed to delivering a seamless and powerful protection.

In order to sustain and support the ongoing development and maintenance of our project, we are exploring the introduction of a premium feature set. By incorporating premium features, we aim to provide additional value to our users who are looking for enhanced functionality and advanced capabilities.

# Contact

you can hear about new features and plans on [X](https://twitter.com/RoAbraTelecom) and if you had any question feel free to ask on [Telegram](https://t.me/RoAbraDev)