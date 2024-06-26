# Is my minifridge even working???

### What I made...

Like many students, I've quickly come to notice that the dorm minifridges don't actually get all that cold, and food placed in them doesn't actually last all that long. To test my hypothesis that these minifridges aren't performing as they should be:
* I set up a microcontroller and Raspberry Pi to record data from various temperature probes and send it to a backend. 
* I created a backend to keep track of all this data, and it also requires the Raspberry Pi to authenticate itself via a simple challenge-response protocol. 
* I created a simple frontend that updates in real time to see just how well my minifridge is working. 

The culmination of this is a website NO LONGER viewable at [https://isfridgeworking.repl.co](https://isfridgeworking.repl.co). You can also spin up a server in the `frontend/` directory. 

### More details...

The backend is NO LONGER hosted on Replit at `oa-backend.residualentropy.repl.co`, and interaction with it is done entirely through simple HTTP requests. It was built in Python with FastAPI, and the autogenerated documentation is available at [https://oa-backend.residualentropy.repl.co/docs](https://oa-backend.residualentropy.repl.co/docs). 

The Raspberry Pi code was also written in Python, and simply transfers data between a serial connection (accessed with `pyserial`) and the backend (accessed with `requests`). Both the Raspberry Pi and the backend have a shared secret that they use to authenticate with each other, but this secret is never sent over the network. A challenge-response protocol is used instead. 

Lastly, the microcontroller I am using is ESP8266-based, and is connected to several DS18B20 digital temperature probes, using the `microDS18B20` library. It is simply connected to the Raspberry Pi over a USB cable, and is located inside the minifridge. 

### The Raspberry Pi/ESP8266 code...

You can find all of that code at [https://github.com/residualentropy/rpi-esp8266-ds18b20](https://github.com/residualentropy/rpi-esp8266-ds18b20). I know someone is going to ask why I didn't use the WiFi capabilities of the ESP8266 and skip the Raspberry Pi altogether. The answer is simply that the ESP8266 does not have stable support for WPA2-Enterprise networks like those found on campus. In a home setting this likely would be possible. 

### Food safety...

The frontend takes the *maximum food-safe temperature* to be 4°C. This value was given by the FDA (US Food and Drug Administration) here: [https://www.fda.gov/consumers/consumer-updates/are-you-storing-food-safely](https://www.fda.gov/consumers/consumer-updates/are-you-storing-food-safely). I am well aware that food safety is more complicated than this single number, and that using the average value (over 4 locations and 10 minutes) does not *strictly speaking* tell you whether the fridge is working or not. However, I think saying that it must be on average below the FDA-recommended maximum temperature is fair, even though it doesn't capture the infinite nuances of how exactly you can define whether a fridge is "working". 
