# JQuery 
jQuery is a javascript library with lots of nice functions that are already coded for us. All you gotta do is include it in your HTML file: 

	  <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.js">

This is how you start using it: 

- `$` is the same as typing `jQuery.(“”);`Between those "" you are going to reference your html. Such as your ID's, Classes or HTML. 
- You can call ID’s in jQuery by using the `#` sign. 
- You can also call methods to your code such as: `$(#my-teaser).html()` which will return the html inside the `my-teaser` ID. 

## What is mustache templates? 

Every part you want to be dynamic, wrap in double “mustaches” {{}}. By dynamic, I mean its going to change. Every new card we make will have a new {{image}} and new {{cast}} and a new {{teaser}}.

	<div  class=“card” style=“background-image: url({{image}})”>
	    <div  class=“cast”>{{cast}}</div>
	    <div  class=“teaser”>{{teaser}}</div>
	</div> 

When would you want to inline css or not? 
Well, it would only work when you are trying to be dynamic. In the example above the `style="background-image: url();` is the *inline* css that is going to be dynamically changed by our mustaches. 


After doing that we need to "stamp-out" our html. Now we are going to hide that html in a script tag in order for the browser to not load it yet. (This is the part you want to stamp-out over and over)

	<script id="my-template" type="x-tmpl-mustache">
		<div   class="card" style="background-image: url({{image}})">
		    <div  class="cast">{{cast}}</div>
		    <div  class="teaser">{{teaser}}</div>
		</div> 
	</script>

The script above has an ID of "my-template" and a type of "x-tmpl-mustache" which is just saying that this script is going to be a mustache template. 

This is how you refer to your script from your JS file: 

	function doStuff(){
		var templateText = $('#my-template').html();
		console.log(templateText);
	}




Now would be a good time to include the moustache library under our jQuery call. We should have around 3 files for JS. Two libraries and one personal one. 

	<script src="https://cdnjs.cloudflare.com/ajax/libs/mustache.js/2.1.2/mustache.min.js"></script>


A way to see how this is working is to code 
		
		$('body').append(templateText);

into your `doStuff()` function. This will append your original divs inside the body. Your function would look like this: 

	
	function doStuff(){
		var templateText = $('#my-template').html();
		console.log(templateText);

		$('body').append(templateText);
	}

Then in the console, if you call `doStuff()` then it will be pushing your divs into the `body` tag. The more you call `doStuff()` the more divs you will be generating! 

Now, we need to call the Mustache library. This way we can replace the `{{image}},{{cast}},{{teaser}}` with actual content. 

	
	Mustache.render(templateText,daValues);

`.render()` is going to take the html stored in the `templateText` variable and go ahead and call `daValues` in order to replace them. So where is `daValues`? Here it is, we make it! 

	var daValues = {
	'image': '1.jpg',
	'cast':'Esther Cuan',
	'teaser':'In a WORLD... full of dinosaurs'
	};


so basically `Mustache.render(html,valuesToChange)` takes the html and the values. Remember that render() is a function in the mustache library that takes two arguments. These two arguments are the html and the values to substitute. 


Now, at this point we are still hard coding these values, but we will eventually call a database and change the values of the object! 

You can even make this more dynamic by making `doStuff` take three parameters and substitute your object with the variables: 




	function doStuff(image,cast,teaser){
		//REACH OUT and grab your raw template
		var templateText = $('#my-template').html();
		// Deine your dynamic values
		var daValues = {
			'image': image,
			'cast': cast,
			'teaser': teaser
		};
		var renderedText = Mustache.render(templateText,daValues);
		$('body').append(renderedText);
	}


	

The number one goal of software engineering is to hide functions and hide the implementations from the outside world. When you call a function, you don't know how it works, the truth is that you don't care. This is why libraries such as Mustache or jQuery are great! 

We can now call all these libraries and use their functions! Everything is a function that takes arguments. Remember that and you are well on your way ! 



# Functions to Functions. To function or not to function? 

We sometimes be dealing with functions that take functions as parameters. These are called: **callback functions**. 

Example: 

	function Esther(function(
		alert("hi Esther!");
		);
	); 

The function `Esther` takes an **annonymous** function that does something. You can see examples of libraries that take functions. Sometimes you have to code the annonyous function. Keep this in mind as you are calling functions that are part of libraries. 
My example is quite dumb so lets show you a good one! 


	  $.get('https://ga-movies-lite.firebaseio.com/movies.json', function(data) {
    console.log(data);
	  });

Here we are calling upon the `.get()` function of jQuery `$`. If you look up the `get()` function in the jQuery documentation it says that it takes several  arguments: 

1. a url of where the data is stored 
2. the data that is sent back from that url 
3. success() which is a function (that you write) that is executed when the request for data succeeds. 
4. dataType which is a string of the type of data expected from the server. 


Whoooff. The good thing is that we don't have to write all these arguments. All we really do is pass the url: https://ga-movies-lite.firebaseio.com/movies.json and the success function! This is because the url has the data we want. 

In the example above we pass the url as a string between '' and a function that gets the data. The parameter in this function is `data` because I got to name the data. So even if my function is annonyous, my data isnt. : 

	function (myData) { 
		//do something with that data here! lets log it: 
		console.log(myData);
	}

tada! 

	  $.get('https://ga-movies-lite.firebaseio.com/movies.json', function(data) {
    console.log(data);
	  });
	  // will print out my data in the console. 



