# MusicPlayer-MicroService
Microservice for a music player

This microservice will receive the path to an mp3 (and some other formats) audio file and will return an object containing the desired information.

# Microservice Code

The microservice will use ZeroMQ to receive the data, as well as music-tag to increase the compatibility between multiple audio formats.
The service can be used as shown below:

```Python
import music_tag
import zmq

context = zmq.Context()
socket = context.socket(zmq.REP)
socket.bind("tcp://*:7854")
```
Of course any port can be used, but for the purpose of testing I chose 7854.

Below you can see where the message sent over is decoded. The message is the path to the file.
```Python
message = socket.recv().decode()
    print(f"File received! Storing mp3 data into an object for use.")

    message should be path to the file
    f = music_tag.load_file("sample-6s.mp3")

    trackTitle = f["tracktitle"]
    trackAlbum = f["album"]
    trackArtist = f["artist"]
    trackNumber = f["tracknumber"]

    socket.send_pyobj([trackTitle, trackAlbum, trackArtist, trackNumber])
```
There are more options available to be shown and sent, which can be seen here: https://pypi.org/project/music-tag/

# Sending a Request

When sending the first request, you need to make sure you're using the same port. Also need to make sure the path to your audio file is correct.

```Python
import zmq

context = zmq.Context()
print("Connecting to microservice...")
socket = context.socket(zmq.REQ)
socket.connect("tcp://localhost:7854")

fileLocation = "sample-6s.mp3"

#Send the fileLocation and then receive the metadata
socket.send(b"{fileLocation}")
trackData = socket.recv_pyobj()
```

# UML Diagram
![image](https://github.com/cokerd1/MusicPlayer-MicroService/assets/116856391/a190f8a0-7fbe-4c2a-ad55-04990095b923)
