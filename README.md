# Learning api.data.gov
[api.data.gov](https://api.data.gov/)
> is a free API management service for federal agencies. [Their] aim is to make it easier for agencies to release and manage APIs.

In other words, there are United States agences that have public APIs that can be used.

# Getting Started
This section walks you through what I learned about api.data.gov.

Their page has a simple and concise [explanation](https://api.nasa.gov/) on authentication and rate limits.

The following is an abridged response from their API. The `X-RateLimit-Limit` is the maximum number of calls that can be made. And `X-RateLimit-Remaining` is the number of calls remaining. So here 33 more calls can be made before the rate limit is exceeded.

```bash
curl -I https://api.nasa.gov/mars-photos/api/v1/rovers/curiosity?api_key=DEMO_KEY
HTTP/1.1 200 OK
X-RateLimit-Limit: 40
X-RateLimit-Remaining: 33
```

So how to increase the rate limit? Register for an API Key.

## Get an API Key
The registration is simple. [Sign Up](https://api.data.gov/signup/) to get a key. It will be sent to your email.

## Use the API Key
To demonstrate the use of the key, we will use NASA's [API](https://api.nasa.gov/)!

Repeat the same query but use your own key.
```bash
export MY_API_KEY=your-key-here

curl -I https://api.nasa.gov/mars-photos/api/v1/rovers/curiosity?api_key=$MY_API_KEY
HTTP/1.1 200 OK
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 998
```

Note the increase rate limit to 1000.

### Cool NASA APIs
You can see the pictures from the MARS rovers!

#### Run this query to get max date information
Gives info to help with more queries, e.g. max_date or max_sol

```bash
curl -s https://api.nasa.gov/mars-photos/api/v1/rovers/curiosity?api_key=$MY_API_KEY | jq
{
  "rover": {
    "id": 5,
    "name": "Curiosity",
    "landing_date": "2012-08-06",
    "launch_date": "2011-11-26",
    "status": "active",
    "max_sol": 2540,
    "max_date": "2019-09-28",
    "total_photos": 366206,
    "cameras": [
      {
        "name": "FHAZ",
        "full_name": "Front Hazard Avoidance Camera"
      },
      {
        "name": "NAVCAM",
        "full_name": "Navigation Camera"
      },
      {
        "name": "MAST",
        "full_name": "Mast Camera"
      },
      {
        "name": "CHEMCAM",
        "full_name": "Chemistry and Camera Complex"
      },
      {
        "name": "MAHLI",
        "full_name": "Mars Hand Lens Imager"
      },
      {
        "name": "MARDI",
        "full_name": "Mars Descent Imager"
      },
      {
        "name": "RHAZ",
        "full_name": "Rear Hazard Avoidance Camera"
      }
    ]
  }
}
```

#### Then run this query
These example queries provide a specific date and camera. Be sure to set the environment variable for `MY_API_KEY`.

```bash


curl -s "https://api.nasa.gov/mars-photos/api/v1/rovers/curiosity/photos?api_key=$MY_API_KEY&earth_date=2019-09-28&camera=fhaz" | jq --raw-output '.photos[] | .img_src'

curl -s "https://api.nasa.gov/mars-photos/api/v1/rovers/curiosity/photos?api_key=$MY_API_KEY&earth_date=2012-12-14&camera=navcam" | jq --raw-output '.photos[] | .img_src'

curl -s "https://api.nasa.gov/mars-photos/api/v1/rovers/opportunity/photos?api_key=$MY_API_KEY&earth_date=2015-6-3&camera=pancam" | jq --raw-output '.photos[] | .img_src'


```

Check out these pictures


![alt text](https://mars.nasa.gov/msl-raw-images/proj/msl/redops/ods/surface/sol/00127/opgs/edr/ncam/NLA_408775302EDR_D0051576NCAM00313M_.JPG)

https://mars.nasa.gov/msl-raw-images/proj/msl/redops/ods/surface/sol/00127/opgs/edr/ncam/NLA_408775302EDR_D0051576NCAM00313M_.JPG


![alt text](https://mars.nasa.gov/msl-raw-images/proj/msl/redops/ods/surface/sol/02540/opgs/edr/fcam/FLB_622990123EDR_F0763002FHAZ00341M_.JPG)

https://mars.nasa.gov/msl-raw-images/proj/msl/redops/ods/surface/sol/02540/opgs/edr/fcam/FLB_622990123EDR_F0763002FHAZ00341M_.JPG

![alt text](https://mars.nasa.gov/msl-raw-images/proj/msl/redops/ods/surface/sol/00127/opgs/edr/ncam/NLA_408775302EDR_D0051576NCAM00313M_.JPG)

https://mars.nasa.gov/msl-raw-images/proj/msl/redops/ods/surface/sol/00127/opgs/edr/ncam/NLA_408775302EDR_D0051576NCAM00313M_.JPG

![alt text](https://mars.nasa.gov/msl-raw-images/msss/00127/mcam/0127MR0007840010200794C00_DXXX.jpg)

https://mars.nasa.gov/msl-raw-images/msss/00127/mcam/0127MR0007840010200794C00_DXXX.jpg

# References
* https://api.data.gov/
* https://github.com/chrisccerami/mars-photo-api


