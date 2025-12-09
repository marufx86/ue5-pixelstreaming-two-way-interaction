# carConfigSendData
Sending pixel streaming responses on color change

<img width="1458" height="283" alt="image" src="https://github.com/user-attachments/assets/79cfa56c-d8a8-472e-9833-ed017b35a6e6" />


Modified packaged Project
https://drive.google.com/file/d/1gXla3vfHFWJvSH92py8nmU6ji8eelUhc/view?usp=sharing

Open the player.js and search the following commands under ("Windows\AutomotiveConfigurator53\Samples\PixelStreaming\WebServers\SignallingWebServer\cirrus.js")
```
var e=new kt({useUrlParams:!0}),t=new Ot(e),s=new ri
```
Replace it with this:
```
var e=new kt({useUrlParams:!0}),t=new Ot(e);window.pixelStreaming=t;var s=new ri
```
(Note: I added `;window.pixelStreaming=t;var in the middle`).


Create shortcut of .exe file and put this command 
```
-log -AudioMixer -PixelStreamingIP=localhost -PixelStreamingPort=8888 -RenderOffScreen -AllowPixelStreamingCommands

```
Paste this into the Console on browser and hit Enter
```
window.pixelStreaming.addResponseEventListener("handle_responses", function(data) {
    console.log("Received from Unreal:", data);
});
```
Now change Color on the seat you'll receive console message on your browser.
