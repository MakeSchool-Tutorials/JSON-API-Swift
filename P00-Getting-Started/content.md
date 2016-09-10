---
title: "Parsing JSON"
slug: parsing-json
---

Just about every app you use involves sending information to or receiving information from the internet.

If you want your app to connect to servers owned by other people, then you are going to have to deal with *APIs* (Application Program Interfaces). An API just means a standard way for some piece of software to talk to another piece of software. In this case, a way for your app to talk to some server somewhere. For example, you may use an [API for getting weather information](http://openweathermap.org/api), an [API for detecting faces](http://www.faceplusplus.com/api-overview/) or one of [thousands of others](https://market.mashape.com/explore?sort=developers).

Very often, the response you'll get from a server is formatted in *JSON* (JavaScript Object Notation). This sounds like a whole lot of work, but luckily, other programers have written libraries to make our life a whole lot easier!

# Introducing JSON

We'll start off by getting used to working with JSON -- the format you'll likely receive responses from APIs in. Navigating a JSON file can be pretty messy in Swift, but it's a lot nicer when you use _SwiftyJSON_ (a popular library).

JSON is a way of representing `Arrays` and `Dictionaries` of values (`String`, `Int`, `Float`, `Double`) as a text file. In a JSON file, `Arrays` are denoted by `[ ]` and dictionaries are denoted by `{ }`.

Let's take a look at some JSON!

## JSON Dictionary

Here's an example of what a dictionary looks like in JSON. The keys must be strings, and are separated from their associated values with a colon `:`.

```
{
  "Calories" : 210,
  "Fat" : 7,
  "Protein" : 14,
  "Carbohydrates" : 23
}
```
> [info]
JSON dictionaries are also sometimes called JSON objects.

## JSON Array

Here's what a JSON array looks like:

```
[
	"Isolated Soy Protein",
	"Tapioca Starch",
	"Salt",
	"Sugar"
]
```


## Example JSON content

Here's an example JSON response received from `http://api.randomuser.me/` -- a testing API for generating fake user data.

```
{
  "results" : [
    {
      "id" : {
        "name" : "",
        "value" : null
      },
      "nat" : "NZ",
      "cell" : "(302)-922-2080",
      "phone" : "(091)-223-5617",
      "login" : {
        "username" : "goldenmouse616",
        "password" : "shaker",
        "sha256" : "a53a4df8d7798149e6b75f652d246d0569d5d40c5bf41cff17befd540cee0cc4",
        "sha1" : "49ec83c662c0b0327d1eadec2bf006b93310d601",
        "salt" : "mTw9b4M6",
        "md5" : "a6bac151418455b103a126cfe0529416"
      },
      "registered" : 1109275261,
      "dob" : 401570145,
      "picture" : {
        "large" : "https://randomuser.me/api/portraits/women/84.jpg",
        "thumbnail" : "https://randomuser.me/api/portraits/thumb/women/84.jpg",
        "medium" : "https://randomuser.me/api/portraits/med/women/84.jpg"
      },
      "location" : {
        "state" : "wellington",
        "street" : "3992 north street",
        "city" : "masterton",
        "postcode" : 67068
      },
      "email" : "rose.moore@example.com",
      "gender" : "female",
      "name" : {
        "title" : "mrs",
        "first" : "rose",
        "last" : "moore"
      }
    }
  ],
  "info" : {
    "version" : "1.0",
    "results" : 1,
    "seed" : "536aa150a38f4a9b",
    "page" : 1
  }
}
```

There is a `Dictionary` at the base with two keys: `"results"` and `"info"`. All the stuff we want is in `"results"` which contains an array of dictionaries. In this case, there is only one dictionary in the array, because we only requested one random user. If we had requested more random users, there would have been multiple dictionaries (each containing information about a new random user) inside of the results array.

The dictionary has keys of `"gender"`, `"name"`, `"location"`, `"email"`, `"login"`, and a few more! `"gender"` contains a `String` value of `"female"`, while other keys like `"name"` contain another `Dictionary` with `"title"`, `"first"`, and `"last"` keys. Are you starting to see the pattern?

Now, how can we actually use this data?

# Download the starter project

We've created a starter project for you with the libraries we'll be using already set up.

> [action]
>
1. Download the JSON & APIs starter project [here](https://github.com/MakeSchool-Tutorials/JSON-API-Swift-Starter/archive/swift3.zip).
1. Unzip the project and move it to your projects folder.
1. Go through the necessary steps to get this project on GitHub. Remember to commit often!
1. Make sure that you open *API-Sandbox.xcworkspace* and not *API-Sandbox.xcodeproj*.


# Introducing SwiftyJSON

SwiftyJSON allows us to access this data as if it were regular Swift arrays and dictionaries without having to deal with all sorts of annoying type casting at each level. It still can be messy to navigate JSON data, but SwiftyJSON makes it a lot easier.

In the `Challenges.swift` file you'll see a function named `exerciseOne`. For now, the most important part is these two lines:

```
let userData = JSON(data: jsonData)
let firstName = userData["results"][0]["name"]["first"].stringValue
```

`jsonData` holds the same data shown above. The first line loads the `jsonData` into a `JSON` object called `userData`. The second line navigates through the `JSON` object to get the first name as a string. Do you see what we did there? We navigated by getting:

1. The array of dictionaries from the root with the `"results"` key
1. The first dictionary of that array
1. The dictionary stored with the `"name"` key
1. The value in that dictionary for the `"first"` key
1. Retrieved the value as a string with `.stringValue`

Can you trace that through the JSON above?

> [info]
> The line
> ```
>	let firstName = userData["results"][0]["name"]["first"].stringValue
> ```
> could have also been written like this:
>
```
let results = userData["results"]
let firstRandomUser = results[0]
let nameDictionary = firstRandomUser["name"]
let firstName = nameDictionary["first"].stringValue
```

# Your turn!

> [challenge]
> Save the other necessary values from this `userData`, and use them to print the following to the console:
>
```
<first name> <last name> lives at <street name> in <city>, <state>, <post code>. If you want to contact <title>. <last name>, you can email <email address> or call at <cell phone number>.
```

You'll need to pay close attention to the formatting of the JSON data above. Remember, some values are deeper in other dictionaries! We needed to get the `"name"` dictionary before we could access the first name.

> [info]
> Pay attention to the final values. Most are strings, but some are not - notice that all values do not have double quotes around them. You can access `Int` values with `.intValue`, `Double` values with `.doubleValue`, `String` values with `.stringValue`, etc.
