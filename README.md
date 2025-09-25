# CS554-lbs-to-kgs

A lightweight, stateless HTTP API that converts pounds to kilograms. Implemented with Node.js/Express, reverse proxied by Nginx, and ran on a single AWS e2e instance.

## Use instructions

To use the API, simply call the public endpoint in this format:

http://18.222.139.95/convert?lbs=[integer]

## Setup instructions

### Use

1. Install curl: `sudo apt update && sudo apt install curl`

2. Call the endpoint: `curl 'http://18.222.139.95/convert?lbs=[integer]`

Response should be in the following format if successful (status code 200):
```
{
    "lbs": <int>,
    "kg": <int>,
    "formula": "kg = lbs * 0.45359237"
}
```

### Test

To test if the instance is running, either visit the HTTP in your browser or attempt to curl into it.
If it does not respond in the above format, there is an error.



### Setup Instructions:

