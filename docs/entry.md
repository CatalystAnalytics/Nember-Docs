# Entry Predictions

## **/entry/enter-data** - <mark>POST</mark>

**`https://api.nember.catalystanalytics.io/entry/enter-data`**

To enter data into the machine learning process for your models, you can access the route via the `/entry/enter-data`. The data must be in a structured format which can be found below. The API key must also be included in the header. Entering data does not incur any usage charges. Once the data is pushed, any previous data will be overwritten. We do not store multiple different sessions of data. There is a 6-hour timer on the data, meaning after 6 hours, the data will be cleared.

---

### **Request**

#### **Headers**
| Key         | Value              | Description                       |
|-------------|--------------------|-----------------------------------|
| `api_key`   | `<your-api-key>`   | Required. Your API authentication key. |

#### **Body**

**data*** (string): Name of key for format.

---

> **StratName*** (string): The name of the strategy.

> ---

> **Open*** (boolean): Indicates if the trade is currently open.

> ---

> **EnterValue*** (number): The entry value for the trade.

> ---

> **ExitValue*** (number): The exit value for the trade.

> ---

> **EntryTime*** (number): The timestamp of the entry in Unix format.

> ---

> **ExitTime*** (number): The timestamp of the exit in Unix format.

> ---

> **Target*** (number, required): The target value for the strategy.

> ---

> **StopLoss** (number): The stop-loss value for the strategy.

> ---

> **Outcome*** (number): The outcome of the strategy.

> ---

> **Position*** (string): The position type (`long` or `short`).

> ---

> **DataPoints*** (array of objects): Contains key-value pairs data points which is vital and used in training the X values. Each object in this array must include:

> ---

>> **Key*** (string): The name of the data point, can be indicator name or something else.

>> --- 

>> **Value*** (number): The value associated with the data point, like sma value.

### **Request Example**
``` json
{
    "data": [
        {
            "StratName": "example_strategy",
            "Open": true,
            "EnterValue": 1.25,
            "ExitValue": 1.35,
            "EnterTime": 1629187200,
            "ExitTime": 1629190800,
            "Target": 1.40,
            "StopLoss": 1.20,
            "Outcome": 1,
            "Position": "Long",
            "DataPoints": [
                {"point1": 1.0, "point2": 2.5}, 
                {"point1": 2.0, "point2": 3.0}
            ]
        },
        {
            "StratName": "example_strategy",
            "Open": true,
            "EnterValue": 1.25,
            "ExitValue": 1.35,
            "EnterTime": 1629187200,
            "ExitTime": 1629190800,
            "Target": 1.40,
            "StopLoss": 1.20,
            "Outcome": 1,
            "Position": "Long",
            "DataPoints": [
                {"point1": 1.0, "point2": 2.5}, 
                {"point1": 2.0, "point2": 3.0}
            ]
        }
    ]
}
```

### **Response Object**
``` json
{ "message":  "Data received successfully"}
```

### **Code Sample**

=== "Python"

    ``` Python
    import requests

    url = "https://api.nember.catalystanalytics.io/entry/enter-data"
    headers = {"api_key": "your_api_key"}
    data = {
        "data": [
            {
                "StratName": "example_strategy",
                "Open": true,
                "EnterValue": 1.25,
                "ExitValue": 1.35,
                "EnterTime": 1629187200,
                "ExitTime": 1629190800,
                "Target": 1.40,
                "StopLoss": 1.20,
                "Outcome": 1,
                "Position": "Long",
                "DataPoints": [
                    {"point1": 1.0, "point2": 2.5}, 
                    {"point1": 2.0, "point2": 3.0}
                ]
            },
            {
                "StratName": "example_strategy",
                "Open": true,
                "EnterValue": 1.25,
                "ExitValue": 1.35,
                "EnterTime": 1629187200,
                "ExitTime": 1629190800,
                "Target": 1.40,
                "StopLoss": 1.20,
                "Outcome": 1,
                "Position": "Long",
                "DataPoints": [
                    {"point1": 1.0, "point2": 2.5}, 
                    {"point1": 2.0, "point2": 3.0}
                ]
            }
        ]
    }

    response = requests.post(url, json=data, headers=headers)
    print(response.json())
    ```

=== "C#"

    ``` C#
    using System;
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;

    class Program {
        static async Task Main(string[] args) {
            string url = "https://api.nember.catalystanalytics.io/entry/enter-data";
            string apiKey = "your_api_key";
            string jsonPayload = @"{
                \"data\": [
                    {
                        \"StratName\": \"example_strategy\",
                        \"Open\": true,
                        \"EnterValue\": 1.25,
                        \"ExitValue\": 1.35,
                        \"EnterTime\": 1629187200,
                        \"ExitTime\": 1629190800,
                        \"Target\": 1.40,
                        \"StopLoss\": 1.20,
                        \"Outcome\": 1,
                        \"Position\": \"Long\",
                        \"DataPoints\": [
                            { \"point1\": 1.0, \"point2\": 2.5 },
                            { \"point1\": 2.0, \"point2\": 3.0 }
                        ]
                    }
                ]
            }";

            using HttpClient client = new HttpClient();
            var request = new HttpRequestMessage(HttpMethod.Post, url);
            request.Headers.Add("api_key", apiKey);
            request.Content = new StringContent(jsonPayload, Encoding.UTF8, "application/json");

            HttpResponseMessage response = await client.SendAsync(request);
            Console.WriteLine(await response.Content.ReadAsStringAsync());
        }
    }
    ```

=== "cURL"

    ``` curl
    curl -X POST "https://yourapi.com/api/entry/enter-data" \
    -H "Content-Type: application/json" \
    -H "api_key: your_api_key" \
    -d '{
    "data": [
        {
        "StratName": "example_strategy",
        "Open": true,
        "EnterValue": 1.25,
        "ExitValue": 1.35,
        "EnterTime": 1629187200,
        "ExitTime": 1629190800,
        "Target": 1.40,
        "StopLoss": 1.20,
        "Outcome": 1,
        "Position": "Long",
        "DataPoints": [
            {"point1": 1.0, "point2": 2.5}, 
            {"point1": 2.0, "point2": 3.0}
        ]
        }
    ]
    }' 
    ```
