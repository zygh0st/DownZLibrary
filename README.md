DownZ 
===================
[![](https://jitpack.io/v/100rabhkr/DownZLibrary.svg)](https://jitpack.io/#100rabhkr/DownZLibrary) | [View release history](https://github.com/100rabhkr/DownZLibrary/releases)

DownZ is an HTTP library that boosts networking in Android apps and makes it significantly easier and faster.

<p align="center"><img src="https://image.ibb.co/gC5XRa/edgetech_2_1_1.png" alt="downz logo"></p>

DownZ offers the following benefits:

 - Handles HTTP requests.
 - Transparent memory response caching  of JSON and Images with support for request prioritization.
 - Cancel a request of image upload or download at any given time.
 - Images in memory cache are auto removed if not used for a long time.
 - Ease of customization, for example, cancel request and clear cache.
 - Strong requisition that makes it easy to effectively manage UI with data being fetched asynchronously from the network.


DownZ excels at handling HTTP requests.It comes with built-in support for images, JSON and Xml. By providing built-in support for the features you require, DownZ frees you from writing tons of code and finally allows you to concentrate on the logic that is specific to your app.


----------


Download
-------------


You can use Gradle 

**Step 1.** Add the JitPack repository to your build file , Add it in your root build.gradle at the end of repositories:

	allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}

**Step 2.** Add the dependency

	dependencies {
	        compile 'com.github.100rabhkr:DownZLibrary:1.1'
	}


Or Maven:

**Step 1.** Add the JitPack repository to your build file

	<repositories>
		<repository>
		    <id>jitpack.io</id>
		    <url>https://jitpack.io</url>
		</repository>
	</repositories>

**Step 2.** Add the dependency

	<dependency>
	    <groupId>com.github.100rabhkr</groupId>
	    <artifactId>DownZLibrary</artifactId>
	    <version>1.1</version>
	</dependency>

Or Sbt: 

**Step 1.** Add the JitPack repository to your build file Add it in your build.sbt at the end of resolvers:

    	resolvers += "jitpack" at "https://jitpack.io"
       
**Step 2.** Add the dependency
 
    	libraryDependencies += "com.github.100rabhkr" % "DownZLibrary" % "1.1"


Snapshots from Sample App
-------------

<img src="https://media.giphy.com/media/l41JQ8NJPwWR9OMTe/giphy.gif"> <img src="https://media.giphy.com/media/l0EoBrCax7QiJB78c/giphy.gif" style="float:right;">



----------

Download Sample App
-------------
All the source code can be downloaded from [Github's Release page](https://github.com/100rabhkr/DownZLibrary/releases) 

You can also download the sample app from [here](https://drive.google.com/open?id=0BwG2nwOU6FIWLW50V2tCYmVZZXpFc2VOQkhQMTZ1bG16cWZr)


How do I use DownZ?
-------------------

**Make Standard Request**

	…
	 DownZ
                .from(mContext) //context 
                .load(DownZ.Method.GET, “http://yoururl.com”)
                .asBitmap() 	//asJsonArray() or asJsonObject() or asXml() can be used depending on need
                .setCallback(new HttpListener<Bitmap>() {
                    @Override
                    public void onRequest() {

                        //On Beginning of request

                    }

                    @Override
                    public void onResponse(Bitmap data) {

                        if(data != null){

                            // do something

                        }
                    }

                    @Override
                    public void onError() {

                        //do something when there is an error

                    }

                    @Override
                    public void onCancel() {

                        //do something when request cancelled

                    }
                });

**Pass Header or Request Parameters (Optional)**


 
        ...
	DownZ
                .from(MainActivity.this)
                .load(DownZ.Method.GET,mUrl)
                .setHeaderParameter("key","value")
                .setRequestParameter("key1","value1")
                .setRequestParameter("key2","value2")
                .asBitmap()
                .setCallback(new HttpListener<Bitmap>() {
                    @Override
                    public void onRequest() {

                    }

                    @Override
                    public void onResponse(Bitmap bitmap) {
                        if(bitmap != null){

                            //do something
                        }
                    }

                    @Override
                    public void onError() {

                    }

                    @Override
                    public void onCancel() {

                    }
                });


**Enable Cache**


    ...
    public class SomeActivity extends AppCompatActivity {
        ...
        CacheManager<JSONArray> cacheManager; // we can use JSONObject, Bitmap as generic type
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            ...
            cacheManager=new CacheManager<>(40*1024*1024); // 40mb
        }

    public void OnRequestMade(View v){
            DownZ
                .from(this)
                .load(DownZ.Method.GET, "http://www.url.com")
                .asJsonArray()
                .setCacheManager(cacheManager)
                .setCallback(new HttpListener<JSONArray>() {
                    @Override
                    public void onRequest() {
                        //fired when request begins
                    }

                    @Override
                    public void onResponse(JSONArray data) {
                        if(data!=null){
                            // do some stuff here
                        }
                    }

                    @Override
                    public void onError() {

                    }

                    @Override
                    public void onCancel() {

                    }
                });
    }
    }
    

**Load Image into ImageView**


    ...
    public class SomeActivity extends AppCompatActivity {
        ...
        CacheManager<Bitmap> cacheManager; 
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            ...
            cacheManager=new CacheManager<>(40*1024*1024); // 40mb
            imageview = (ImageView) findViewById(R.id.image_profile);
            imageview.setDrawingCacheEnabled(true); //can be used if Image has to be shared afterwards
            imageview.buildDrawingCache();
            ...
            
        }
    //event to make request
    public void btnLoadImageClicked(View v){
        DownZ
                .from(this)
                .load(DownZ.Method.GET, "http://www.url.com/image.jpg")
                .asBitmap()
                .setCacheManager(cacheManager)
                .setCallback(new HttpListener<Bitmap>() {
                    @Override
                    public void onRequest() {
                        //fired when request begin
 

     }

                    @Override
                    public void onResponse(Bitmap data) {
                        if(data!=null){
                            // do some stuff here
                            imageview.setImageBitmap(data);
                        }
                    }

                    @Override
                    public void onError() {

                    }

                    @Override
                    public void onCancel() {

                    }
                });
    }

    }
    
**Removing from Cache**

     ...
     public class SomeActivity extends AppCompatActivity {
        ...
        CacheManager<Bitmap> cacheManager; 
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            ...
            String mUrl = "http://yoururl.com/image.png";
            cacheManager=new CacheManager<>(40*1024*1024); // 40mb
            imageview = (ImageView) findViewById(R.id.image_profile);
            imageview.setDrawingCacheEnabled(true); //can be used if Image has to be shared afterwards
            imageview.buildDrawingCache();
            ...
            
        }
    //event to clear item from cache
    public void btntoClearCache(View v){
    
           cacheManager.removeDataFromCache(mUrl);
    }
    }


Getting Help
------------

To report a specific problem or feature request in DownZ, you can [open a new issue on Github](https://github.com/100rabhkr/DownZLibrary/issues/new). For questions, suggestions, or even to say 'hi', just drop an email at skkumar.sk94@gmail.com

Contributing
------------

If you like DownZ, please contribute by writing at skkumar.sk94@gmail.com. Your help and support would make DownZ even better. Before submitting pull requests, contributors must sign [Google's individual contributor license agreement.](https://cla.developers.google.com/about/google-individual)

Author
------

Saurabh Kumar - [@100rabhkr](https://github.com/100rabhkr) on GitHub, [@NotSaurabhKumar](https://twitter.com/NotSaurabhKumar) on Twitter

License
-------

MIT See the [LICENSE](https://github.com/100rabhkr/DownZLibrary/blob/master/LICENSE) file for details.

Disclaimer
----------

This is not an official Google product.
