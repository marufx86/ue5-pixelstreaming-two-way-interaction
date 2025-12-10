# Car Configurator: Pixel Streaming Data Response

This project demonstrates how to send Pixel Streaming responses from Unreal Engine to the web frontend when a specific event (Seat Color Change) occurs.

![Blueprint Setup](https://github.com/user-attachments/assets/79cfa56c-d8a8-472e-9833-ed017b35a6e6)

## 📥 Downloads

* **Modified Packaged Project (send data unreal to frontend):** [Download from Google Drive](https://drive.google.com/file/d/1gXla3vfHFWJvSH92py8nmU6ji8eelUhc/view?usp=sharing)

---

## ⚙️ Configuration

### 1. Modify the Web Server Logic
To expose the Pixel Streaming instance to the browser console, you need to modify the JavaScript source.

1.  Navigate to the Signalling Web Server directory:
    `Windows\AutomotiveConfigurator53\Samples\PixelStreaming\WebServers\SignallingWebServer\`
2.  Open **`player.js`** (or the relevant script file in your build).
3.  Search for the following minified code snippet:
    ```javascript
    var e=new kt({useUrlParams:!0}),t=new Ot(e),s=new ri
    ```
4.  **Replace** it with the following code (this exposes `window.pixelStreaming`):
    ```javascript
    var e=new kt({useUrlParams:!0}),t=new Ot(e);window.pixelStreaming=t;var s=new ri
    ```

### 2. Configure Launch Arguments
1.  Create a **Shortcut** of your project's `.exe` file.
2.  Right-click the shortcut and select **Properties**.
3.  In the **Target** field, add the following arguments to the end of the line:
    ```text
    -log -AudioMixer -PixelStreamingIP=localhost -PixelStreamingPort=8888 -RenderOffScreen -AllowPixelStreamingCommands
    ```

---

## 🧪 Testing & Usage

1.  Run the **Signaling Server** (usually `run_local.bat` or `start.bat`).
2.  Launch the Unreal Engine application using the **Modified Shortcut** created in the previous step.
3.  Open your web browser and navigate to the local stream (usually `http://127.0.0.1`/`localhost`).
4.  Open the **Developer Console** in your browser (F12).
5.  Paste the following listener code into the console and hit **Enter**:
    ```javascript
    window.pixelStreaming.addResponseEventListener("handle_responses", function(data) {
        console.log("Received from Unreal:", data);
    });
    ```
6.  **Interact:** Change the color of the car seat within the stream.
7.  **Verify:** You should see the confirmation message appear in the browser console.
