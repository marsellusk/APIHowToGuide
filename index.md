## A How to Guide for Steam Web API

Link to the [API](https://developer.valvesoftware.com/wiki/Steam_Web_API)

### What is Steam API and What do you Need

Steam is currently the most popular place for gamers to buy and own games with millions of users. Because of this, there are many websites out there that utilize Steam and their database to further improve their own website. They do this by utilizing Steam’s web API. There are a few requirements that are needed before any user can use Steam’s API. First a steam account will be needed. Second an API key from Steam itself. Third access to a coding language that can handle a XMLHTTPrequest. For the purpose of this guide Javascript will be used however, there are other languages that can do this. The concepts should be the same in the other languages yet with different syntax. 

### Implications what is it used for

The steam web API offers information either about a specific app, a specific video game, or a specific steam account. This means that whatever your website is about using the Steam API must deal with one of these 3 things. One way the Steam API can be used is stats. Say you have a website that is about a specific game. Using the Steam API you can provide users with access global stats for this game may it be specific items or maps. Then you can also provide the user with the stats for the game unique to a steam profile. This would let users compare themselves to friends and other players in that specific game. This is just a basic example of how Steam can be used.

### Introduction to the Guide

The Steam API does a very good job of showing what is necessary to access their API and the data you will get back by doing so. However, Steam doesn't show how this should look when implementing the API into code. They do provide links to implementations of their API at the bottom of the page, but they can either be confusing or hard to traverse through. This guide will hopefully show how to use the Steam Web API in code. This way you can use it on your own

### Getting Started With Javascript
	
  To start the request we must let the javascript know by setting a variable equal to the XML request. After this the request will then be opened. First we notify what type of request we are sending. Since we are only receiving data and not sending this will be a GET request. If we are sending data to the API to receive something back we would then do a POST request. Next we send the domain of the Steam Web API.
	
### Without API Key

Some of the options don’t require an API key. For these we just send the domain of the the Steam API plus the arguments in the query string.. An example of this can be seen by trying to get the achievements for an app:

```markdown
 
 http://api.steampowered.com/ISteamUserStats/GetGlobalAchievementPercentagesForApp/v0002/?gameid=440&format=xml

```
### With API Key

The others require the API  key to be at the beginning of the query string then the arguments. If you don’t know where to get the Steam API key it can be found [here](https://steamcommunity.com/login/home/?goto=%2Fdev%2Fapikey) or at the top of the Steam API page. For security reasons you should never give your personal API key as public information. An example of the final domain using player summaries should look like this with the Xs substituted with your personal key:

```markdown

http://api.steampowered.com/ISteamUser/GetPlayerSummaries/v0002/?key=XXXXXXXXXXXXXXXXXXXXXXX&steamids=76561197960435530

```

It should be noted that different sections of the Steam API use v0001 or v0002 in the link. Pay attention to which one is needed because it can cause errors. The final thing to put in the open request is true or false. True is for asynchronous  request an false is for synchronous request. It is better to use asynchronous request in case there is an error in receiving the data from the Steam API. The final code to produce an output should look like this:

```markdown

 var req = new XMLHttpRequest();
  
req.open('GET', 'http://api.steampowered.com/ISteamUserStats/GetGlobalAchievementPercentagesForApp/v0002/?gameid=440&format=xml', true);
   
    req.addEventListener('load',function(){
      if(req.status >= 200 && req.status < 400){
	  console.log(JSON.parse(req.responseText))
    } else {
        console.log("Error in network request: " + req.statusText);
      }});
    
    req.send(null);
	
    event.preventDefault()


```

If there is no error in interacting with the API the code will enter the function and do what you set it to do. In this case I just request it to display the data received in the console. Because we are only receiving data and not sending any to the api req.send() will be set to null. Although asynchronous is better for use sometimes it can cause frustrating errors that are hard to debug. If you run into trouble you can try doing a synchronous request just to make sure you are able to interact with the Steam API. That will look like this:

```markdown

      var req = new XMLHttpRequest();
      req.open("GET", 'http://api.steampowered.com/ISteamUserStats/GetGlobalAchievementPercentagesForApp/v0002/?gameid=440&format=xml', false);
      req.send(null);
      console.log(JSON.parse(req.responseText));


```

One last thing to note for the code is that the API key and the query string arguments don't have to be hard coded in. In fact it is better if they are not sometimes because this allows the request to the API server to change without having to rewrite the code all over again. This can be done by setting the key and arguments to variables. This is shown here:

```markdown
var APIkey = XXXXXXXXXXXXXXXXXXXXXXX;
var steamID = (steam ID searched by user);

var req = new XMLHttpRequest();
  
req.open('GET', 'http://api.steampowered.com/ISteamUser/GetPlayerSummaries/v0002/?key=' + APIkey + '&steamids=' + steamID, true);
   
    req.addEventListener('load',function(){
      if(req.status >= 200 && req.status < 400){
	  console.log(JSON.parse(req.responseText))
    } else {
        console.log("Error in network request: " + req.statusText);
      }});
    
    req.send(null);
	
    event.preventDefault()


```

### How to Handle the Output
	
Now that you know how to make a request we need to be able to deal with the response from the Steam server. This is easily done by using the line of code:

```markdown

JSON.parse(req.responseText)

```

Here is what happens when I log the response to the console from the asynchronous code above. Unfortunately because Chrome doesn’t like Cross-Origin Requests when there is no server involved I was forced to use Internet Explorer. However Internet Explorer console isn't able to deal with objects and just displays them as undefined. For the purpose of this guide I displayed the results without parsing it which means it will be displayed as HTML code.(Instructor side note: I would have switched to an API that worked but it was too close to the deadline to do so.) The results for showing achievements percentages in the app look like this:



![Picture](https://cloud.githubusercontent.com/assets/25128961/23541487/7875a6aa-ff9c-11e6-94c2-eb4087a3ceb6.png)



In order to show what it should look like in the console I used the help of a weather API that is the same exact concept except with weather using this code:

```markdown

var req = new XMLHttpRequest();
      req.open("GET", "http://api.openweathermap.org/data/2.5/weather?q=Corvallis,or&appid=XXXXXXXXXXXXXXXXXX", false);
      req.send(null);
      console.log(JSON.parse(req.responseText));

```
The result in Chrome console  should then look like this displaying the objects and their properties:



![Picture](https://cloud.githubusercontent.com/assets/25128961/23541620/6d5e05c2-ff9d-11e6-9f73-75f780b000d0.png)



What is shown here is all the data I received back from the API. It also shows the pathing I took to be able to access the weather description. This is good but we still would like to display the data to the html page so the user can see it. This can be done by making the response equal to a variable then accessing the properties through that variable. I display this in this code here for the weather API
```markdown
  var response = JSON.parse(req.responseText);
     
  document.getElementById('report').textContent = response.weather["0"].description;
```
Here is how it looks when I display the above code to the page for the user to see:



![Picture](https://cloud.githubusercontent.com/assets/25128961/23541671/b6cf204c-ff9d-11e6-92c2-c9e584710201.png)



Here is the code that would do the same thing but is applied to the Steam Web API:
```markdown
var response = JSON.parse(req.responseText);
     
document.getElementById('achievementName').textContent = response.achievements["3"].name;
document.getElementById('achievementPercent').textContent = response.achievements["3"].percent;
```

What this code is doing is accessing the achievement object and going through it’s array until we get the achievement wanted. After this it is assigning that achievement's properties to an HTML element by that element's ID. Then it will display the name of that achievement and the percentage of people who have completed that achievement to the HTML page. 
	
	

### Outro

With this use of the Steam API should be fairly easy. Most APIs function in a similar fasion meaning this logic can be applied to many other APIs out there. Again the Steam API is perfect for use when making a website about specific games and players that play the games. Being Able to access the Steam API is the simple part. How you use the data and display it to your HTML page for your user is where stuff should get more complex since that is beyond just using the API. 


