* a streaming protocol is a standardized method of delivering different types of media over the internet.
* live streamign protocol is different from codec
* codec stands for coder/decoder. it is a tool for making video files smaller.
* if a corner of the video remains black for a few seconds, the individual pixel data can be tossed and just include a reference instead.

## Common Streamign protocol
### HTTP Live Streaming(HLS)
* developed by Apple
* is an adaptive bitrate protocol (automatically detect changes in the internet connection speed of the viewer and serve the best available quality video at any given time) and uses HTTP servers.
* latency:
* **Low LAtency HLS**: version of HLS that enables latency at 2 seconds or less.
* HLS breaks video into small segments, which are served over standard HTTP as downloadable files, allowing easy traversal of firewalls and compatibility with CDNs.
* Clients download a playlist file (.m3u8) that indexes available video segments at different bitrates, enabling adaptive streaming based on network conditions.
* Playback software assembles the segments for smooth viewing, with the option for low-latency streaming (2 seconds or less) in newer versions

### Real-Time Messaging Protocol (RTMP)
* previously used to deliver videos to the Adobe Flash Player.
* RTMP is now used for ingestion from the encoder to the online video platform
* Uses persistant TCP connection.
* RTMP splits the audio, video, and metadata streams into small chunks or fragments, These fragments are then sent over the network, and the receiving server/player reassembles them into full frames or messages.
* RTMP uses multiplexing, it interleaves audio, video, and control data streams together. These streams are tagged with stream/channel IDs and sent over a single TCP connection.

### Secure Reliable Transport (SRT)
* open source protocol build on UDP
* low-latency video transmission by handling packet loss and reordering at the application layer
* It uses selective retransmission and conditional packet dropping to maintain stream quality even over unstable networks
* SRT supports AES encryption and is designed for secure, high-performance live video delivery

### Dynamic Adaptive Streaming over HTTP (MPEG-DASH)
* MPEG-DASH is also an adaptive bitrate (ABR) protocol
* MPEG-DASH divides media into small segments, served over HTTP, with a manifest file (MPD) describing available qualities and segment URLs. A segment is a small chunk of the media.
* A segment URL is the HTTP path to a specific media segment file. These URLs are used by the player to download segments as needed.
* **This allows for HTTP-based delivery using CDNs and web servers**
* The client selects the optimal segment quality in real time based on current network conditions, ensuring smooth, adaptive playback
* MPEG-DASH is codec-agnostic and widely supported, similar in architecture to HLS but standardized by MPEG
  
### WebRTC
* WebRTC enables real-time, peer-to-peer audio, video, and data streaming directly between browsers, using a combination of protocols for media transport and NAT traversal.
* It negotiates codecs and connection details using the Session Description Protocol (SDP) and establishes media paths with ICE, STUN, and TURN servers
* WebRTC is designed for ultra-low-latency, interactive communication, often used for video calls and live chat
* This uses:

             ### ICE (Interactive Connectivity Establishment)
              * Framework used in WebRTC to find the best connection path between peers.
              * Gathers multiple **network candidates** (local IPs, STUN, TURN).
              * Tries all combinations of candidates.
              * Selects the most **efficient and reliable path**.
              * Works with both STUN and TURN.
              
              ---
              
              ### STUN (Session Traversal Utilities for NAT)
              
              * Helps peers discover their **public IP address and port**.
              * Used when devices are behind **NAT or firewalls**.
              * Lightweight and fast.
              * Enables **direct peer-to-peer (P2P) connections**.
              * Works in most home and office network setups.
              
              ---
              
              ### TURN (Traversal Using Relays around NAT)
              
              * **Relay server** that forwards traffic between peers.
              * Used **only when direct connection fails** (e.g., strict NAT, firewalls).
              * Acts as a **fallback** to STUN.
              * **Adds latency** and **uses server bandwidth**.
              * More **reliable**, but also more **costly**.

### Real Time Streaming Protocol
* RTSP acts as a "remote control" for media servers, allowing clients to set up, play, pause, and stop media streams over a persistent TCP connection
* RTSP is commonly used for IP cameras and surveillance but is less suited for web streaming without conversion to protocols like HLS
* Doesn't actually deliver media, instead, it controls the delivery.
