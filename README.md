Utilities for NodeJS
====================

var utils = require("./node-utils");

String
-------

-	utils.string.**trim**(str) or String.**trim**()

-	utils.string.**ltrim**(str) or String.**trim**()

-	utils.string.**rtrim**(str) or String.**trim**()

-	utils.string.**dropDiacritics**(str) or String.**dropDiacritics**()

-	utils.string.**isNumber**(str) or String.**isNumber**()

-	utils.string.**stripHtml**(str) or String.**stripHtml**()

-	utils.string.**count**(str,substr,flags) or String.**count**(substr,flags). 

	+	It counts the number of ocurrences of substr. Flags can be "i" (ignore case) and/or "d" (drop accents)<br />                                                                              
-	utils.string.**reverse**(str) or String.**reverse**()

-	utils.string.**toHtml**(str) or String.**toHtml**() 

	+	Converts diacritics and almost all chars into html entities<br />  

-	utils.string.**fromHtml**(str) or String.**fromHtml**() 

	+	Converts into diacritics html encoded entities

Numeric
--------

-	utils.number.**stoi**(str) or String.**stoi**()

	+	Converts to integer<br />  

-	utils.number.**stof**(str,decimals) or String.**stof**(decimals)

	+	Converts to float, with number of decimals<br />  

-	utils.number.**round**(num,decimals) or Number.**round**(decimals)

	+	Rounds number to the given number of decimals

Date
-----

-	utils.date.**diff**(date1,date2,unit) 

	+	Unit = "d": days,"h": hours,"m": minutes,"s": seconds. Default unit is millis<br />  

-	utils.date.**millis**(date) 

	+	Returns a timestamp in millis from current date or date passed as param (string or date object)<br />  

-	utils.date.**frommillis**(millis) 

	+	Returns a date object from millis passed as parameter


File
-----

-	utils.file.**write**(file,data,options,callback) 

	+	options = "w" write or "a" append<br />  

-	utils.file.**read**(file,encoding,callback) 

	+	encoding is optional<br />  

-	utils.file.**exists**(file,callback) 

-	utils.file.**getModTime**(file,callback)

	+	Date object is given to the callback as an argument<br />  

-	utils.file.**remove**(file,callback)

-	utils.file.**createpath**(path, callback)

URL
----

-	utils.url.**get**(url, options, callback) 

-	utils.url.**post**(url, options, callback) 

	*	Support for **http** and **https**
	*	Support for **proxy requests** (in url. E.g: "url"=http://www.proxy.com:8080/www.urltobeproxied.com)
	*	It is possible to set only url or options, but options need to set host, path, ...
	*	Options is an object with some props:

		+	"encoding" default is "utf-8"
		+	"post_data" (for post()) is an object 

			with the vars (post_data:{a:1,b:2})  

			or with the body in "data" key (post_data:{data:"whatever"})  

		+	"headers" (object)
		+	"auth" (string)
		+	"forceparse": if the url is with proxy data is better to set to true

Cache
------

-	utils.cache.**getPath**() 

	+	Get the current cache dir (default is "./cache")<br />  

-	utils.cache.**setPath**(path,callback)

	+	Set the cache dir (and create if it doesn't exists)
	
	+	It's recommended to use absolute paths ("/apps/myapp/cache")<br />  

-	utils.cache.**setOptions**({path : "/mypath", size : 1}}, callback)

	+	Set the cache dir (and create if it doesn't exists) and cache max size
	
	+	Cache size is in MB<br />  

-	utils.cache.**set**(key, data, expiretime, callback) 

	+	Expiretime is in seconds. It is optional<br />  

-	utils.cache.**get**(key, callback)

-	utils.cache.**remove**(key, callback)


Properties
----------

- utils.props.**load**(path_to_file, callback)

	+ properties in the file are loaded into a JSON object passed in callback
	
	+ Format of the properties file is 

				key1=value1
				key2=value2
				key3=value3
				key4=123456789

- utils.props.**save**(path_to_file, properties, callback)
	
	+ properties param is a json object. You can save properties dynamically


SAMPLES
--------

-	Caching twitter request due to twitter api limits 

		var utils = require("./node-utils");
		var twitterquery = "davidayalas";
		var twitterurl = "http://api.twitter.com/1/statuses/user_timeline.json?screen_name=";

		utils.cache.get(twitterquery, function(content){
			if(!content){
				utils.url.get(twitterurl+twitterquery,function(result){
					utils.cache.set(twitterquery,result,300);
					console.log(result);
				});
			}else{
				console.log(content);
			}
		});
