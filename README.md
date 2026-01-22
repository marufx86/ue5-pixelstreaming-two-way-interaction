# Car Configurator: Pixel Streaming Data Interaction

This project demonstrates two-way communication between Unreal Engine and a Web Frontend using Pixel Streaming:
1.  **Response:** Sending data from Unreal Engine to the Frontend (e.g., when a seat color changes).
2.  **Commands:** Sending commands from the Frontend to Unreal Engine (e.g., triggering a blueprint event).

> **⚠️ Compatibility Note:** This project is built using **Unreal Engine 5.3**. It is highly recommended to use UE 5.3 to ensure full compatibility with the provided project files and scripts.

---

## 📥 Downloads

Select the project version based on which direction of data flow you are testing.

| Version | Description | Download Links |
| :--- | :--- | :--- |
| **Unreal ➔ Frontend** | Modified project setup to send data **out** from Unreal. | [Google Drive](https://drive.google.com/file/d/1gXla3vfHFWJvSH92py8nmU6ji8eelUhc/view?usp=sharing) / [Backup (Mega)](https://mega.nz/file/yHpjiQ7L#1ya6KuS8p2K1hBsa2KAjvnHfM1wxna_ny8DoGbFVkmk) |
| **Frontend ➔ Unreal** | Modified project setup to receive commands **into** Unreal. | [Google Drive](https://drive.google.com/file/d/1JjUClstrhmhVLQHHc4a_X68UpFe-SUa4/view?usp=sharing) / [Backup (Mega)](https://mega.nz/file/GKwBiSBJ#xkHebupnuc7B8ek5oFrh8KNoTiy5ABbv1UJUt5Qmqy8) |

---

## Scenario 1: Sending Data (Unreal ➔ Frontend)

This setup sends a JSON response to the browser console when a specific event (Seat Color Change) occurs in the engine.

![Blueprint Setup](https://github.com/user-attachments/assets/79cfa56c-d8a8-472e-9833-ed017b35a6e6)

### ⚙️ Configuration

#### 1. Modify the Web Server Logic
To expose the Pixel Streaming instance to the browser console, you must modify the JavaScript source in your packaged build.

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

#### 2. Configure Launch Arguments
1.  Create a **Shortcut** of your project's `.exe` file.
2.  Right-click the shortcut and select **Properties**.
3.  In the **Target** field, add the following arguments to the end of the line:
    ```text
    -log -AudioMixer -PixelStreamingIP=localhost -PixelStreamingPort=8888 -RenderOffScreen -AllowPixelStreamingCommands
    ```

### 🧪 Testing & Usage

1.  Run the **Signaling Server** (usually `run_local.bat` or `start.bat`).
2.  Launch the Unreal Engine application using the **Modified Shortcut**.
3.  Open your web browser and navigate to the local stream (usually `http://127.0.0.1` or `localhost`).
4.  Open the **Developer Console** in your browser (**F12**).
5.  Paste the following listener code into the console and hit **Enter**:
    ```javascript
    window.pixelStreaming.addResponseEventListener("handle_responses", function(data) {
        console.log("Received from Unreal:", data);
    });
    ```
6.  **Interact:** Change the color of the car seat within the stream.
7.  **Verify:** You should see the confirmation message appear in the browser console.

---

## Scenario 2: Receiving Commands (Frontend ➔ Unreal)

This setup allows the web frontend to send commands that execute logic within Unreal Engine.

![Frontend to Unreal Blueprint](https://github.com/user-attachments/assets/4e0a4ced-f7e4-44f6-9994-192cf5fadaf0)

### ⚙️ Configuration

In the provided "Frontend ➔ Unreal" download, the following changes have already been made to the packaged project:

* **Files Added/Modified:**
    * `custom_interface.html` was added.
    * `player.js` was modified to handle the input.
* **Location:**
    These files are located under: `AutomotiveConfigurator53\Samples\PixelStreaming\WebServers\SignallingWebServer\Public`

### 🧪 Testing & Usage

1.  Run the Signaling Server and the Unreal Engine project (as described in Scenario 1).
2.  To test the custom interface, visit the specific HTML page in your browser:
    ```text
    http://127.0.0.1/custom_interface.html
    ```
3.  Use the interface buttons to trigger events in the Unreal Engine scene.
