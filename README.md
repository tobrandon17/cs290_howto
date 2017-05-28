# Welcome to the Spotify Web API How To Guide site! 
Spotify is a music and podcast streaming platform that has approximately 100 million users with roughly half of them as paid subscribers to their premium service. Free users have access to basic features while premium members have access to improved streaming quality, offline music downloads, and no advertisements. The platforms that they support are Windows, macOS, Linux, Android, and iOS. 

This API How To Guide (for specifically Spotify Web API) is a tool to help you learn about the functionality of exactly what is happening when the requests for data are submitted and how they are retrieved. This guide is designed for the entry level API users and is meant to be straightforward and concise without compromising detail. 

Let's get started with some background information on APIs!

## What is an API and what does Spotify's API do?
An API is a shortened term for application programming interface. In short, it is defined as a set of routines, protocols, and tools for building software applications. APIs are what make it possible to move data around without the requirement of sharing source code.

Using the Spotify API, other external mediums can gain access to Spotify content. This can include data from albums, playlists, and user data. To access user data, however, authorization will have to be granted first. The basic steps of using the Spotify Web API are as follows. First, you will have to register an application with Spotify, authenticate a user, retrieve access to that user’s data, and then retrieve data from a Web API endpoint. 


To do so, you will first need to log in to your Spotify account.
[Login](https://accounts.spotify.com/en/authorize?response_type=code&client_id=a5429cc04d0b4bf78872d22a60ec4c4b&scope=user-self-provisioning&redirect_uri=https:%2F%2Fdeveloper.spotify.com%2Fmy-applications%2Fcallback&state=fomnZJs3ql)


![GitHub Logo](/images/logo.png)

## Spotify Web API is RESTFul
Spotify notes on their user guide that their API is based on REST principles. The common operations that they use are GET, POST, PUT, and DELETE. GET retrieves resources, POST creates resources, PUT changes and replaces resources, and DELETE is self-explanatory. 
The way that Spotify Web API gains authentication is by sending an OAuth (open authorization) access token in the request header. OAuth is used primarily to allow third party services to access account information without requiring the user’s password to be disclosed. This will require the use of a client id and a client secret.


## How to Set it Up

## Interactions with Spotify Web API
Initial Commit
