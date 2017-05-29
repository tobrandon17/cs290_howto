# Welcome to the Spotify Web API How To Guide site! 
Spotify is a music and podcast streaming platform that has approximately 100 million users with roughly half of them as paid subscribers to their premium service. Free users have access to basic features while premium members have access to improved streaming quality, offline music downloads, and no advertisements. The platforms that they support are Windows, macOS, Linux, Android, and iOS. 

This API How To Guide (for specifically Spotify Web API) is a tool to help you learn about the functionality of exactly what is happening when the requests for data are submitted and how they are retrieved. This guide is designed for the entry level API users and is meant to be straightforward and concise without compromising detail. 

Let's get started with some background information on APIs!

## What is an API and what does Spotify's API do?
An API is a shortened term for application programming interface. In short, it is defined as a set of routines, protocols, and tools for building software applications. APIs are what make it possible to move data around without the requirement of sharing source code.

Using the Spotify API, other external mediums can gain access to Spotify content. This can include data from albums, playlists, and user data. To access user data, however, authorization will have to be granted first. The basic steps of using the Spotify Web API are as follows. First, you will have to register an application with Spotify, authenticate a user, retrieve access to that user’s data, and then retrieve data from a Web API endpoint. 


To do so, we will first need to log in to a Spotify account. I will be demonstrating on an existing account that I have created for myself. Feel free to follow along with your own Spotify account using the link below.
[Login](https://accounts.spotify.com/en/authorize?response_type=code&client_id=a5429cc04d0b4bf78872d22a60ec4c4b&scope=user-self-provisioning&redirect_uri=https:%2F%2Fdeveloper.spotify.com%2Fmy-applications%2Fcallback&state=fomnZJs3ql)

After you have logged in, the "Create an Application" option will be available to you under the My Applications tab. Hitting the Create button will then register the app to you on the Spotify Developer Page.
![Spotify My Application](/images/Spotify1.png)

After the application has been registered, you will be issued a Client ID as well as a Client Secret. The following is shown below. Please note that the Client Secret should be kept to yourself and that if you believe it has been compromised, you must regenerate it otherwise your application's identity can be stolen and other applications can masquerade as your own.

![Spotify Client ID](/images/Spotify2.png)


<html>
  <head>
    <title>Example of the Authorization Code flow with Spotify</title>
    <link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap.min.css">
    <style type="text/css">
      #login, #loggedin {
        display: none;
      }
      .text-overflow {
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        width: 500px;
      }
    </style>
  </head>

  <body>
    <div class="container">
      <div id="login">
        <h1>This is an example of the Authorization Code flow</h1>
        <a href="/login" class="btn btn-primary">Log in with Spotify</a>
      </div>
      <div id="loggedin">
        <div id="user-profile">
        </div>
        <div id="oauth">
        </div>
        <button class="btn btn-default" id="obtain-new-token">Obtain new token using the refresh token</button>
      </div>
    </div>

    <script id="user-profile-template" type="text/x-handlebars-template">
      <h1>Logged in as {{display_name}}</h1>
      <div class="media">
        <div class="pull-left">
          <img class="media-object" width="150" src="{{images.0.url}}" />
        </div>
        <div class="media-body">
          <dl class="dl-horizontal">
            <dt>Display name</dt><dd class="clearfix">{{display_name}}</dd>
            <dt>Id</dt><dd>{{id}}</dd>
            <dt>Email</dt><dd>{{email}}</dd>
            <dt>Spotify URI</dt><dd><a href="{{external_urls.spotify}}">{{external_urls.spotify}}</a></dd>
            <dt>Link</dt><dd><a href="{{href}}">{{href}}</a></dd>
            <dt>Profile Image</dt><dd class="clearfix"><a href="{{images.0.url}}">{{images.0.url}}</a></dd>
            <dt>Country</dt><dd>{{country}}</dd>
          </dl>
        </div>
      </div>
    </script>

    <script id="oauth-template" type="text/x-handlebars-template">
      <h2>oAuth info</h2>
      <dl class="dl-horizontal">
        <dt>Access token</dt><dd class="text-overflow">{{access_token}}</dd>
        <dt>Refresh token</dt><dd class="text-overflow">{{refresh_token}}></dd>
      </dl>
    </script>

    <script src="//cdnjs.cloudflare.com/ajax/libs/handlebars.js/2.0.0-alpha.1/handlebars.min.js"></script>
    <script src="http://code.jquery.com/jquery-1.10.1.min.js"></script>
    <script>
      (function() {
        /**
         * Obtains parameters from the hash of the URL
         * @return Object
         */
        function getHashParams() {
          var hashParams = {};
          var e, r = /([^&;=]+)=?([^&;]*)/g,
              q = window.location.hash.substring(1);
          while ( e = r.exec(q)) {
             hashParams[e[1]] = decodeURIComponent(e[2]);
          }
          return hashParams;
        }
        var userProfileSource = document.getElementById('user-profile-template').innerHTML,
            userProfileTemplate = Handlebars.compile(userProfileSource),
            userProfilePlaceholder = document.getElementById('user-profile');
        var oauthSource = document.getElementById('oauth-template').innerHTML,
            oauthTemplate = Handlebars.compile(oauthSource),
            oauthPlaceholder = document.getElementById('oauth');
        var params = getHashParams();
        var access_token = params.access_token,
            refresh_token = params.refresh_token,
            error = params.error;
        if (error) {
          alert('There was an error during the authentication');
        } else {
          if (access_token) {
            // render oauth info
            oauthPlaceholder.innerHTML = oauthTemplate({
              access_token: access_token,
              refresh_token: refresh_token
            });
            $.ajax({
                url: 'https://api.spotify.com/v1/me',
                headers: {
                  'Authorization': 'Bearer ' + access_token
                },
                success: function(response) {
                  userProfilePlaceholder.innerHTML = userProfileTemplate(response);
                  $('#login').hide();
                  $('#loggedin').show();
                }
            });
          } else {
              // render initial screen
              $('#login').show();
              $('#loggedin').hide();
          }
          document.getElementById('obtain-new-token').addEventListener('click', function() {
            $.ajax({
              url: '/refresh_token',
              data: {
                'refresh_token': refresh_token
              }
            }).done(function(data) {
              access_token = data.access_token;
              oauthPlaceholder.innerHTML = oauthTemplate({
                access_token: access_token,
                refresh_token: refresh_token
              });
            });
          }, false);
        }
      })();
    </script>
  </body>
</html>


## Spotify Web API is RESTFul
Spotify notes on their user guide that their API is based on REST principles. The common operations that they use are GET, POST, PUT, and DELETE. GET retrieves resources, POST creates resources, PUT changes and replaces resources, and DELETE is self-explanatory. 
The way that Spotify Web API gains authentication is by sending an OAuth (open authorization) access token in the request header. OAuth is used primarily to allow third party services to access account information without requiring the user’s password to be disclosed. This will require the use of a client id and a client secret.


## How to Set it Up

## Interactions with Spotify Web API

## Recap

In this guide, we have covered topics regarding the authorization of Spotify Web API, what happens when a GET request is made, and what kind of response we will would expect to see. This was meant to be a brief introduction to demonstrate a few of the endpoints within Spotify Web API. There are many more things that can be accomplished with this API. Feel free to explore more within Spotify's [Official Documentation](https://developer.spotify.com/web-api/)!
