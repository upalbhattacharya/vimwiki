## Bing Search API

This is a document to highlight how the Bing Search API can be used to pull images and hence make datasets for image classification problems.

In order to make the requests from a python script you will need to import the following:

```python
import requests
```

This can be installed by using the command(if it is not already there):

```python
pip3 install requests
```

**ADVISE**: Since most of this work is related to Data Science and Computer Vision, try having a separate virtual environment defined for these projects where you can install all such requirements.

You will need an API key for using the Bing Image Search API. This can be found in your Microsoft Azure account after creating a resource for the Web Search API.

Having created a resource, the API key can be found under the created resource's **Keys and Endpoint** location (can be found under RESOURCE MANAGEMENT)

Save the API key to a variable(preferably).

```python
subscription_key = "yoursubscriptionkey"
```

Define the search url from where requests will be made.

```python
# For the image search API
search_url = "https://api.bing.microsoft.com/v7.0/images/search"
```

Other Endpoints may be found [here](https://docs.microsoft.com/en-us/bing/search-apis/bing-image-search/reference/endpoints)

Further, define a variable to hold the search terms that are to be passed to the ```get()``` method of the ```requests``` library to fetch the required data(in this case, images).

The API key needs to be passed to the ```headers``` attribute of the ```get()``` method as a dictionary in the following manner:

```python
headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
# The key value in the dictionary is predefined and must be used as is given.
```

Create a dictionary of parameters that define the nature and scope of the search. For example, for images with a particular search term, with a public license, the format will be:

```python
params = {"q" : search_term, "license" : "public", "imageType" : "photo",
        "count" : groupsize, "offset" : offset, "color" : color}
# Once again, the keys names are predefined
```
where **count** defines the number of images to get and the **offset** defines the image offset to start from. These two attributes are useful to pull batches of images.

As an example, instead of making one pull of 250 images, one can run a loop with a count of 50 per get request and to pull 250 images over 5 iterations, resetting the offset to be at the end of the last image pulled in the last get request for each iteration.

One could define the number of images one wants and then run a loop as follows to make get requests:

```python
imageNum = 900 # Enter number of images to be pulled
for offset in range(0, imageNum, groupsize):
    params["offset"] = offset
    search = requests.get(search_url, headers = headers, params = params)
    # The get() method syntax holds the same regardless of the loop or not
    search.raise_for_status()
    results = search.json() # This is where the actual data is.
```

In order to save the pulled images, it may make sense to discard any images that have any exceptions. This can be done in the process of saving the images by handling any exceptions thrown. A small list of exceptions that might be useful in this scenario are:

```python
EXCEPTIONS = set([IOError, FileNotFoundError,
	requests.exceptions.RequestException, requests.exceptions.HTTPError,
	request.exceptions.ConnectionError, requests.exceptions.Timeout])
```

Thereafter, a loop inside the pooling loop can be used so that images pulled in each iteration can be saved properly while handling exceptions as follows

```python
    # Still within the scope of the loop define above
    for v in results["value"] # results is a dictionary with a key "value".
    try:
        # making a request to download the image
        r = requests.get(v["contentUrl", timeout = 30)
        # getting the image extension
        extension = v["contentUrl"][v["contentUrl"].rfind("."):]
        # Creating the path
        path = os.path.sep.join([basePath, '{}{}'.format(str(count).zfill(8), extension)])
        count = count + 1
        f = open(path, "wb")
        f.write(r.content)
        f.close()
    except Exception as e:
        if type(e) in EXCEPTIONS:
             continue
```
The whole code together is given below

```python
# Make the necessary imports

import requests
import os

# API key

subscription_key = "..."

# Search URL to use

search_url = "https://api.bing.microsoft.com/v7.0/images/search"

# Defining the header to pass to the get method of requests
# Containing the API key

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}

# Define parameters to be passed to the get method

params = {"q" : search term, "license": "public",
        "imageType" : "photo", "count" : groupSize,
        "offset" : offset, "color" : color}
        
# Defining number of images to pull

imageNum = 1000
# Defining the exceptions and a basePath

EXCEPTIONS = set([IOError, FileNotFoundError,
requests.exceptions.RequestException, requests.exceptions.HTTPError,
request.exceptions.ConnectionError, requests.exceptions.Timeout])

basePath = "..."

# Looping in batches and getting the data
count = 0
for offset in range(0, imageNum, groupSize):
    params["offset"] = offset
    search = requests.get(search_url, headers = headers, params = params)
    search.raise_for_status()
    resuts = search.json()
    
    for img in results["value"]:
        try:
             r = requests.get(v["contentUrl"], timeout = 30)
             extension = v["contentUrl"][v["contenUrl"].rfind("."):]
             path = os.path.sep.join(
                [basePath, '{}{}'.format(
                str(count).zfill(8), extension)])
             count = count + 1
             f = open(path, "wb")
             f.write(r.content)
             f.close()
        except Exception as e:
             if type(e) in EXCEPTIONS:
                continue
```
        
