# Websocket

The Nember websocket connection can be used to provide insanely fast predictions up to 20ms response time. We recommend using this endpoint if the trading bot/algo needs to make fast predictions in a small interval (ticks, seconds, and lower 1 - 10 minute intervals). Below is a detailed guide on how to use this endpoint properly. With one API Key, any number of connections can be made but the usage charge will occur for each connection made. You can access you connections in the web app. If for any reason something happens you can close the connection on the web app.

## **/ws** - <mark>Websocket</mark>

**`wss://api.nember.catalystanalytics.io/ws`**

### **Step 1: Connect**
=== "wscat"

    ``` wscat
    wscat -c wss://api.nember.catalystanalytics.io/ws
    ```

=== "Python"

    ``` Python
    import asyncio
    import websockets

    async def connect():
        uri = "wss://api.nember.catalystanalytics.io/ws"
        async with websockets.connect(uri) as websocket:
            pass

    # Run the event loop
    asyncio.get_event_loop().run_until_complete(connect())
    ```

### **Step 2: Authenticate**

Every connection needs to be authenticated with a API Key which can be found in your dashboard. To authenticate a connection you just need to send the api key as your first text to the server by itself. If the key is valid the connection will be sustained if invalid the connection will be terminated. 

#### **Response Object**

=== "Valid"

    ``` json
    { 
        "status": "success",
        "message": "Valid API key." 
    }
    ```

=== "Invalid"

    ``` json
    { 
        "status": "error",
        "message": "Invalid API key." 
    }
    ```

#### **Code Sample**

=== "Python"

    ``` Python
    import asyncio
    import websockets

    async def connect():
        uri = "wss://api.nember.catalystanalytics.io/ws"
        async with websockets.connect(uri) as websocket:
            await websocket.send("your_api_key")

            response = await websocket.recv()
            response_data = json.loads(response)

            if "message" in response_data and response_data["message"] == "valid":
                print("API key successfully verified!")
            else:
                print("API key verification failed.")

    # Run the event loop
    asyncio.get_event_loop().run_until_complete(connect())
    ```

### **Step 3: Make Predictions**

After your key is authenticated you can make predictions with your models you trained in the web app dashbaord. This is pretty straight foward, just need to specify the model you want with the same amount of data points used during training.

#### **Request Example**
``` json
{
    "api_key": "your_api_key",
    "strat_name": "example_strategy",
    "model_name": "DecisionTree",
    "model_type": "class",
    "DataPoints": [
        { "point1": 3, "point2": 2 },
        { "point1": 1, "point2": 3 }
    ]
}
```

#### **Response Object**
=== "Excisting Model Prediction"

    ``` json
    {
        "type": "success",
        "seq_num": 1,
        "message": "Existing model data used for prediction",
        "prediction": "<YOUR PREDICTION VALUE>"
    }
    ```

=== "New Model Prediction"

    ``` json
    {
        "type": "success",
        "seq_num": 1,
        "message": "New model configuration added, prediction made",
        "prediction": "<YOUR PREDICTION VALUE>"
    }
    ```

=== "No Model Found"

    ``` json
    {
        "type": "error",
        "seq_num": 1,
        "message": "Model configuration not found in the backend."
    }
    ```

#### **Code Sample**
=== "Python"

    ``` Python
    import asyncio
    import websockets

    async def connect():
        uri = "wss://api.nember.catalystanalytics.io/ws"
        connected = False
        async with websockets.connect(uri) as websocket:
            if not connected:
                await websocket.send("your_api_key")
            elif:
                data = {
                    "api_key": "your_api_key",
                    "strat_name": "example_strategy",
                    "model_name": "DecisionTree",
                    "model_type": "class",
                    "DataPoints": [
                        { "point1": 3, "point2": 2 },
                        { "point1": 1, "point2": 3 }
                    ]
                }
                await websocket.send(json.dumps(data))

            response = await websocket.recv()
            response_data = json.loads(response)

            if "message" in response_data and response_data["message"] == "valid" and not connected:
                connected = True
            elif connected:
                if response_data["type"] == "success":
                    print(response_data["prediction"])
                elif response_data["type"] == "error":
                    print(response_data["message"])
            else: 
                print("API key verification failed.")

    # Run the event loop
    asyncio.get_event_loop().run_until_complete(connect())
    ```