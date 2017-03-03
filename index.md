## Welcome to GitHub Pa

### what is steam api and what do you need

Steam is currently the most popular place for gamers to buy and own games with millions of users. Because of this, there are many websites out there that utilize Steam and their database to further improve their own website. They do this by utilizing Steam’s web API. There are a few requirements that are needed before any user can use Steam’s API. First a steam account will be needed. Second an API key from steam itself. Third access to a coding language that can handle a XMLHTTPrequest. For the purpose of this guide Javascript will be used however, there are other languages that can do this. The concepts should be the same in the other languages yet with different syntax. 

### Implications what is it used for

The steam web API offers information either about a specific app, a specific video game, or a specific steam account. This means that whatever your website is about using the Steam API must deal with one of these 3 things. One way the Steam API can be used is stats. Say you have a website that is about a specific game. Using the Steam API you can provide users with access global stats for this game may it be specific items or maps. Then you can also provide the user with the stats for the game unique to a steam profile. This would let users compare themselves to friends and other players in that specific game. This is just a basic example of how Steam can be used.

### intro to the guide

### Getting Started With Javascript
	
  To start the request we must let the javascript know by setting a variable equal to the XML request. After this the request will then be opened. First we notify what type of request we are sending. Since we are only receiving data and not sending this will be a GET request. If we are sending data to the API to receive something back we would then do a POST request. Next we send the domain of the Steam Web API.
	
### without api key

Some of the options don’t require an API key. For these we just send the domain of the the Steam API plus the arguments in the query string.. An example of this can be seen by trying to get the achievements for an app:
```markdown
 
 http://api.steampowered.com/ISteamUserStats/GetGlobalAchievementPercentagesForApp/v0002/?gameid=440&format=xml

```
### with api key

The others require the API  key to be at the beginning of the query string then the arguments. If you don’t know where to get the Steam API key it can be found here or at the top of the Steam API page.For security reasons you should never give your personal API key as public information. An example of the final domain using player summaries should look like this with the Xs substituted with your personal key:

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

In order to show what it should look like in the console I was use the help of a weather API that is the same exact concept except with weather using this code:

```markdown

var req = new XMLHttpRequest();
      req.open("GET", "http://api.openweathermap.org/data/2.5/weather?q=Corvallis,or&appid=XXXXXXXXXXXXXXXXXX", false);
      req.send(null);
      console.log(JSON.parse(req.responseText));

```
The result in Chrome console  should then look like this displaying the objects and their properties:

![Picture](https://cloud.githubusercontent.com/assets/25128961/23541620/6d5e05c2-ff9d-11e6-9f73-75f780b000d0.png)

This is good but we still would like to display the data to the html page so the user can see it. This can be done by making the response equal to a variable then accessing the properties through that variable. This can be shown by this code here for the weather API
```markdown
  var response = JSON.parse(req.responseText);
     
  document.getElementById('report').textContent = response.weather["0"].description;
```
Here is how it looks when I display it to the page for the user to see:

![Picture](https://cloud.githubusercontent.com/assets/25128961/23541671/b6cf204c-ff9d-11e6-92c2-c9e584710201.png)

Here is the code that would do the same thing but is applied to the Steam Web API:
```markdown
var response = JSON.parse(req.responseText);
     
document.getElementById('achievementName').textContent = response.achievements["3"].name;
document.getElementById('achievementPercent').textContent = response.achievements["3"].percent;
```

What this code is doing is accessing the achievement object and going through it’s array until we get the achievement wanted. After this it is assigning that achievement's properties to an HTML element by that element's ID. Then it will display the name of that achievement and the percentage of people who have completed that achievement to the HTML page. 
	
	

### Outro

With this use of the Steam API should be fairly easy.

You can use the [editor on GitHub](https://github.com/marsellusk/HowToGuide/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```



For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/marsellusk/HowToGuide/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
