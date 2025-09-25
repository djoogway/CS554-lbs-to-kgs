# CS554-lbs-to-kgs

A lightweight, stateless HTTP API that converts pounds to kilograms. Implemented with Node.js/Express, reverse proxied by Nginx, and ran on a single AWS e2e instance.

## Use instructions

To use the API, simply call the public endpoint with an HTTP GET in this format:

http://18.222.139.95/convert?lbs=

With the integer at the end you wish to convert.

## Setup instructions

### Install

There is no installation required to use this endpoint. It's entirely public (as long as the instance is running).

### Test

You may perform a fast GET test by using curl.

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

If the connection fails, the instance is likely not running.
