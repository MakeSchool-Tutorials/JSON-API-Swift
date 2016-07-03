---
title: "API calls with Alamofire"
slug: api-calls-alamofire
---

In this exercise, we'll be dealing with _live data_ from iTunes' top 25 _movie_ feed. We'll make a real network request and get the current data!

Your mission now is to use the `Movie` struct we created to show information about a random movie from iTunes' top 25!

> [info]
> We won't be providing any layout hints beyond the skeleton already in `Main.storyboard`, that part is up to you. If you complete everything without making any layout changes, you'll end up with something like this.
>
> ![Completed challenge](./basic_completion.png)

# Making API calls

Open up `ViewController.swift` and take a look at the code provided.

> [action]
> Comment out or remove the calls to `exerciseOne()`, `exerciseTwo()`, and `exerciseThree()` in `viewDidLoad()`.

The code we care most about is this:

```
let apiToContact = "https://itunes.apple.com/us/rss/topmovies/limit=25/json"
// This code will call the iTunes top 25 movies endpoint listed above
Alamofire.request(.GET, apiToContact).validate().responseJSON() { response in
    switch response.result {
    case .Success:
        if let value = response.result.value {
            let json = JSON(value)

            // Do what you need to with JSON here!
            // The rest is all boiler plate code you'll use for API requests


        }
    case .Failure(let error):
        print(error)
    }
}
```

We are using [Alamofire](https://github.com/Alamofire/Alamofire), a Swift networking library to manage our network requests. We've already included Alamofire in this project, but it's easy to install in any iOS project - just add `Alamofire` to your Cocoapods *Podfile*, then run `pod install`. 

The above code follows the general format you will use for all network requests in Alamofire! It does the following:

1. Creates a `GET` request to `apiToContact`
1. Validates the request to ensure it worked
1. Passes the JSON response to a closure
1. In the closure we handle success and failure with a switch statement
1. If successful, we create a `JSON` object from the response's result's value
1. After that, you can work with it just like you did on the previous exercises!

Remember that the response handling is done inside a closure. Network requests are asynchronous. This means that after executing `Alamofire.request()`, execution will skip past the closure entirely. Once the data is received, the code within the closure runs. Depending on your network connection, this can be a tiny delay or even seconds long.

> [info]
> `GET` requests are calls to APIs that ask for data. Conversely, `POST` requests send data to the server. You'll learn a bit more about `POST` requests on the next page.

# Getting started

This will be the most open-ended tutorial you've gone through so far. Fear not, we have faith that you'll be able to complete it!

> [action]
> Inside of the `Alamofire.request()` closure, parse the JSON to to grab a random movie from the response, and use it to create a new `Movie` object. Use that `Movie` object to populate the `ViewController`'s labels so that it looks like the one above.

<!-- html comment to break boxes -->

> [info]
>  You'll want to add an instance variable to `ViewController` which holds a `Movie` struct. You should store the random `Movie` you create in that instance variable. We'll need that `Movie` object later to make the _View on iTunes_ button work. 
> 
> You can use `arc4random_uniform(upperBound)` to generate a random number from 0 up to, but not including `upperBound`. `arc4random_unfiform` only works with `UInt32` values, not normal Swift `Int`s, so you'll have to do some casting!

If you get lost, refer back to exercises two and three in `Challenges.swift`.

# Adding the poster

Thus far, we haven't parsed the poster from the JSON object. Look back at `iTunes Movie.json` to figure out where it's located. The link to the poster image is the third entry in the `im:image` array. In Zootopia's case, it's the value `"http://is3.mzstatic.com/image/thumb/Video30/v4/89/c0/0a/89c00a09-016b-6297-102d-6f451310ee17/pr_source.lsr/170x170bb-85.jpg"`. 

> [action]
> Add a poster property to your `Movie` struct and update your `init` to populate with a link to the poster.

Once your `Movie` object is parsing a link to the poster, you can use the `loadPoster()` method in `ViewController` to add it to your view. `loadPoster()` uses the `AlamofireImage` extension on `UIImageView`, which has a method called `af_setImageWithURL()`.  This asynchronously downloads the image from the URL we provide it, and puts it in the `posterImageView` for us! :)

# Programming the button

> [action]
> The _View on iTunes_ button is connected to `viewOniTunesPressed()`. Fill in this method to open up the movie in iTunes.

<!-- html comment to break boxes -->

> [info]
> Here's a hint. You can open StackOverflow.com in Safari with:
>
```
UIApplication.sharedApplication().openURL(NSURL(string: "http://www.stackoverflow.com")!)
```

Unfortunately, the simulator blocks access to iTunes! You'll need to test this one on device to see if it is working.
