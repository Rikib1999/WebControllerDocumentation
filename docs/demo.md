To get familiar with the application, you can try the prepared demo. The demo consists of a Unity project and a configuration for the web application.

The demo features a single scene where the player moves around a room collecting coins. When coins are collected, a message is sent to the server with information about the coin collection and the current number of coins collected. This data is displayed in the session detail of the web application. Using the control panel in the session detail, you can reset the player's position, reset the number of collected coins, or spawn any number of new coins. In the Unity project, the CoinManager script sends the current context for the buttons in the control panel, indicating whether the player has collected any coins or if the count is zero. Based on this, the button to reset the number of collected coins will either be displayed or hidden. The PositionSender script sends the player’s position, which is displayed on the map in the session details.

#Instructions

1. Download the Unity project from: `https://github.com/Rikib1999/WebControllerDEMO`
2. Copy the demo application configuration from the config.txt file in the demo to the configuration file of the web application.
3. Download the map image DEMOmap.png and host it online (for example you can use the webpage `https://imgbb.com/upload`).
4. Fill in the address of the hosted image into the web application config in the map section.
5. Run the demo Unity project. The project consists of only one scene.
6. Set up the connection in the WebControllerManager object by specifying the server address and port according to your web application.
7. The project is ready; you can start testing.