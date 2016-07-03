---
title: "Creating your own networked app"
slug: own-networked-app
---

Now that we've seen how all this code works, let's put your knowledge to the test and create a simple, networked app! This is going to be the most open project you've done so far!

If you like, you can download another copy of the API-Sandbox starter project [here](https://github.com/MakeSchool-Tutorials/JSON-API-Swift-Starter/archive/master.zip) or you can just continue from your current _API-Sandbox_. If you continue, make sure to save the code you completed so you can refer back to it if you get lost!

> [action]
> Complete an app from scratch using one of the options listed below. You should spend no more than 3 days on this as part of the App Hackathon. This is your chance to take risks, fail fast, and do something cool before starting on your real app!

# Extend the movies app

You can spend time extending the movies app you created in the last exercise if you would like. Try adding some more features. Improve the layout. Make it pretty! Consider adding next and previous buttons to navigate through the top results.

# Use Other iTunes Data

You can use a different iTunes API to display data other than movies. For example, you could display a random top iOS app, book, or TV show. You'll need to create a new struct to hold this data just like we did with `Movie`.

> [info]
> You can get a lot API endpoints for all sorts of iTunes information [here](https://rss.itunes.apple.com/us/). Unfortunately, this will **not** work for music even though it's a listed media type! See the next section for more information.
>
> Change the drop downs to select the type of content you want, copy the `RSS Feed URL` and replace `xml` at the end with `json`. Use this string as your `apiToContact`.

Make sure to spend some time making the app look nice! Consider adding next and previous buttons to navigate through the top results.

# iTunes Music Data

Music is a bit harder to deal with. You have to use the official [iTunes Search API](https://affiliate.itunes.apple.com/resources/documentation/itunes-store-web-service-search-api/) instead of getting JSON from the RSS feeds. This API requires you to use parameters with your requests.

> [info]
> You can add parameters to your request with the following format:
>
```
// Modify these to be appropriate for your use case
let apiToContact = "https://itunes.apple.com/search"
let parameters = ["term": "jack+johnson", "media": "music"]
>
// This code will call the iTunes Search API
// to search for music containing "Jack Johnson"
Alamofire.request(.GET, apiToContact, parameters: parameters)
  .validate().responseJSON() { response in
    switch response.result {
    case .Success:
        if let value = response.result.value {
            let json = JSON(value)
>
            // Do what you need to with JSON here!
            // The rest is all boiler plate code you'll use for API requests
            print(json)
>
>
        }
    case .Failure(let error):
        print(error)
    }
}
```
>
> This code sample searches iTunes for music by Jack Johnson. See the documentation for more parameters you can use!
>
> The parameters argument must be a `Dictionary` of `[String : String]` type.

# Choose your own API

If you are feeling ambitious, you can find another API and use that. You'll need to study the JSON a bit more since you won't have any starting code provided for you to parse it.

## Reddit

You can add `.json` to any subreddit on Reddit to get a `JSON` copy of it! To get all the posts on `/r/cats` you would request `https://www.reddit.com/r/cats.json`. Maybe you can do something cool with that?

## Mashape

[MashApe](https://market.mashape.com/) provides a huge list of APIs you can use. You'll need to study their documentation. These will all require API keys to use (see _Authentication_ below).

Go to MashApe and create an account. Find an API you want to use and read through the documentation. Add the API to your _Default Application_ or create a new one.

You can find your keys under the settings for your _Application_ (see the `GET THE KEYS` button). For now, use the _Testing_ key and pass it as a parameter as described in _Authentication_ below.

## Programmable web

[Programmable Web](http://www.programmableweb.com/category/all/apis?data_format=21173) provides a huge list of APIs you can use. You'll need to study their documentation and make sure they support JSON. All of these will likely require parameters as discussed in _iTunes Music data_ and some of these will require authentication! See _Authentication_ below for a discussion of some types you might encounter.

## Google

Search for the service you want to use with "JSON API" in the keywords. Maybe you'll find something not listed on _Programmable Web_!

# Authentication

If the API you want to use requires authentication, you'll need to do some more work with each request.

## API Keys

Some APIs require an API key to use. Fortunately API keys are easy. They are usually passed as a parameter as described in _iTunes Search API_ above. See the documentation for your API to learn about how to get an API key. Be careful about sending requests too often if your API requires a key -- a lot of free APIs are rate limited! 

## HTTP Authentication

See Alamofire's [documentation](https://github.com/Alamofire/Alamofire#http-basic-authentication) for information on this. You'll need to add a call to `authenticate` to your request and potentially include the credentials in your `apiToContact`.

## OAuth

OAuth is beyond the scope of this tutorial. We recommend you avoid it for this App Hackathon project!

# Sending Data to APIs

Everything we've done so far has been `GET` requests. We are `GET`ing data from the API. For more advanced apps you might want to send data back to the API! This sort of request is called a `POST` request and will be documented as such in the API documentation. To send a `POST` request, change `.GET` in your Alamofire call to `.POST`. Sometimes you can leave off the trailing closure so it looks something like this:

```
let apiToContact = ""https://httpbin.org/post""
let parameters = [
    "foo": "bar",
    "baz": ["a", 1],
    "qux": [
        "x": 1,
        "y": 2,
        "z": 3
    ]
]

Alamofire.request(.POST, apiToContact, parameters: parameters)
```

# Testing APIs

I highly recommend you check out the free application [Postman](https://www.getpostman.com/apps) so you can test your API calls before trying them out in Swift. Postman will allow you to view nicely formatted JSON responses, test out parameters and do pretty much anything you would want to do before touching any code!

See the [Postman documentation](https://www.getpostman.com/docs), specifically [Sending Requests](https://www.getpostman.com/docs/requests) to get started!

# I'm getting an error about Secure Transport!

`App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure. Temporary exceptions can be configured via your app's Info.plist file.`

If you start your own fresh project instead of using ours, remember that you need to disable Secure Transport if you want to access any `http` endpoints instead of ones that are `https`. See the Friend Search section of Makestagram Part 2 for more information!
