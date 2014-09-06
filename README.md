Android-Live-Camera-Video-To-Web-Page
=====================================
Android live camera video to a webpage over internet (not wifi). After doing some research I came up with a working solution by combining multiple components together.

If you are beginner, this article will introduce you to lot new things like real time streaming protocols (RTSP, RTMP), Wowza Media Engine, libstreaming, WAMP and jwplayer.

Below are the two protocols on which real time streaming really works. Go through the wikipedia links to get enough knowledge on underlaying technology behind real time streaming.

RTSP Protocol
Real Time Streaming Protocol is a networking protocol mainly used to stream real time media data like audio or video. It establishes a streaming session between client and server. In this tutorial we use this protocol while sending video stream from android mobile to streaming server.

Lean more about Real Time Streaming Protocol-http://en.wikipedia.org/wiki/Real_Time_Streaming_Protocol

RTMP Protocol
Real Time Messaging Protocol was developed by Adobe for Flash Player to transmit the realtime media (audio, video) between server and flash player. This protocol we use to receive video stream from server to flash player.

Lean more about Real Time Messaging Protocol-http://en.wikipedia.org/wiki/Real_Time_Messaging_Protocol

Downloading Libstreaming library
Building a RTSP library involves deep understanding of real time streaming protocols and good command over multiple java media APIs which is not easy for every beginner. Luckly Fyhertz made our lives easier by providing an excellent RTSP library called libstreaming for android. Using this library, streaming video / audio from android mobile can done with very few lines of code.

Download the library and keep it aside.https://github.com/fyhertz/libstreaming

Installing & Configuring Wowza Media Engine
Wowza Media Engine is very popular streaming engine which can stream high quality video and audio. In our project it acts as server side streaming framework which receives video from android device and starts a streaming service which will be again consumed by webpage to display the video.

Wowza also comes with an admin panel called Wowza Media Engine Manager to control streaming channels, publishers and other stuff. Unfortunately wowza is not a free software, you will have to buy a commercial license. But don’t worry, it comes with a trail period of 6 months which is more than enough for testing.

 Registration
In order to use Wowa media engine you need to register and get a license key first. The key will be sent to your email address after the registration. So make sure that you entered a valid email address instead of abc@abc.com :)

Once you complete registration here, you can find the license key in the email.
You may registration the wowza media engine here-http://www.wowza.com/pricing/trial

 Downloading & Installing
1. Download Wowza Media Engine from here-http://www.wowza.com/pricing/installer
2. Run the installer and enter the license key when it asks for it.

2.3 Creating publisher username and password
Once wowza installed, open Wowza Streaming Engine Manager from Start => All Programs. This opens up an admin panel in the browser. While streaming from android mobile, the streaming video needs to be authenticated with Wowza before start decoding it. So we need to create a publisher username and password first. These credentials we will use in our android app later.

To create a publisher, click on Server ⇒ Publisher ⇒ Add Publisher and enter credentials.


Creating streaming channel
You can create your own streaming url in the admin panel. But Wowza already creates a channel(app) for you named live. I am going to use the same app in this tutorial. If you want, you can create your own.

Now import the downloaded libstreaming project into your Eclipse workspace. File ⇒ Import ⇒ Android ⇒ Existing Android Code Into Workspace and browse to libstreaming.

Now add the libstreaming project as a library project to our project. Right click on our project ⇒ Properties. It will open up a dialog window. On the left select Android and on the Right under Library add libstreaming project.

Now we are done with our android application. Let’s create a web application which displays the streaming video on web page.

Installing WAMP:
I have already explained lot of times what is WAMP and it’s use (here & here). Download and Install WAMP from http://www.wampserver.com/en/ and start the program from Start => All Programs. Once started, you should be able to access http://localhost/ in the browser.

WPlayer
On the web page to play streamed I am using jwplayer which supports RTMP protocol. jwplayer is a flash player with javascript API enabled which means you can control the player with javascript methods.

Create an account at http://www.jwplayer.com/sign-up/ and download the jWplayer. Once you login into jwplayer.com, you can download a copy from https://account.jwplayer.com/#/account. Click on Download Self-Hosted Player at the bottom of the page.


1. Now create a folder in your wamp/www directory (Normally wamp will be located at C:/wamp). I am creating a folder named wowza_web_app inside www.

2. Inside wowza_web_app create two more folders named css and js.

3. Paste jwplayer files inside js folder. You have to copy jwplayer.js, jwplayer.html5.js and jwplayer.flash.swf. Also paste jquery-1.9.1.min. Download jquery from here-http://api.androidhive.info/other/jquery-1.9.1.min.js

4. Create an html file named index.html in wowza_web_app and paste the following content. This page will have jwplayer and other UI to play and stop streaming video.

index.html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Android Video Streaming</title>
        <script src="js/jquery-1.9.1.min.js" type="text/javascript"></script>
        <script type="text/javascript" src="js/jwplayer.js"></script>       
        <script src="js/player.js" type="text/javascript"></script>
        <script>jwplayer.key = ""</script>
        <link rel="stylesheet" type="text/css" href="css/style.css" />       
    </head>
 
    <body>
        <!-- Header -->
        <div id="header">
            <div class="container">
                <h1>Android Video Streaming</h1>
            </div>
        </div>
        <!-- ./Header -->
 
        <!-- Video Preview -->
        <div class="container">            
            <div id="video_preview">                    
                <div id="player"></div><div class="clear"></div>
                <br/><br/><br/>
                <input type="text" id="stream_url" value="rtmp://192.168.43.233:1935/live/android_test"/><br/>
                <input type="button" id="btn_start" class="" value="Start" />
                <input type="button" id="btn_stop" class="" value="Stop"/>
            </div>
            <div class="clear"></div>
        </div>
        <!-- ./Video Preview -->
 
        <div class="container">
            <div class="info"><p>
                    Tutorial: <a href="#">http://www.androidhive.info/?p=4449&preview=true</a><br/>
                    <a href="http://www.wowza.com/">Wowza Media Engine</a><br/>
                    Thanks <a href="Fyhertz">@Fyhertz</a> for <a href="https://github.com/fyhertz/libstreaming">libstreaming</a><br/>
                    www.androidhive.info
                </p>
            </div>
        </div>
    </body>
</html>

 Inside css folder create a file named style.css and paste the following styles.
 
 style.css
/* 
    Document   : style
    Created on : May 31, 2014, 2:09:28 AM
    Author     : Ravi Tamada    
*/
 
body { 
    padding:0;
    margin: 0;
}
.clear{
    clear: both;
}
.container{
    width: 1100px;
    margin: 0 auto;
    padding: 0;
}
#header{
    text-align: left; 
    box-shadow: 0px 3px 3px #e3e3e3;
}
#header h1{
    font:normal 35px arial;
    color: #ed4365;
    margin: 0;
    padding: 15px 0;
}
#video_preview{
    text-align: center;
}
#player, #player_wrapper{
    margin: 0 auto !important;
    margin-bottom: 20px !important;
    margin-top: 60px !important;    
}
input#stream_url{
    background: none;
    border: 2px solid #92d07f;
    outline: none;
    padding: 5px 10px;
    font: 18px arial;
    color: #666;
    width: 600px;
    text-align: center;
}
#btn_start, #btn_stop{
    padding: 8px 30px;
    color: #fff;
    border: none;
    outline: none;
    font: normal 16px arial;
    border-radius: 6px;
    cursor: pointer;
    margin-top: 15px;
}
#btn_start{
    background: #3bbe13;    
}
#btn_stop{
    background: #e6304f;   
}
.info{
    margin-top: 80px;
    text-align: center;    
    font:normal 13px verdana;
}
.info p{
    line-height: 25px;
}
.info a{
    color: #f05539;
}

Create player.js file inside js folder and paste following javascript. This script takes care of initializing jwplayer and other button click events.

player.js
var data = [];
var jw_width = 640, jw_height = 360;
 
// Outputs some logs about jwplayer
function print(t, obj) {
    for (var a in obj) {
        if (typeof obj[a] === "object")
            print(t + '.' + a, obj[a]);
        else
            data[t + '.' + a] = obj[a];
    }
}
 
$(document).ready(function() {
 
    jwplayer('player').setup({
        wmode: 'transparent',
        width: jw_width,
        height: jw_height,
        stretching: 'exactfit'
    });
 
    $('#btn_start').click(function() {
        startPlayer($('#stream_url').val());
    });
 
    $('#btn_stop').click(function() {
        jwplayer('player').stop();
    });
 
 
 
    startPlayer($('#stream_url').val());
});
 
// Starts the flash player
function startPlayer(stream) {
 
    jwplayer('player').setup({
        height: jw_height,
        width: jw_width,
        stretching: 'exactfit',
        sources: [{
                file: stream
            }],
        rtmp: {
            bufferlength: 3
        }
    });
 
    jwplayer("player").onMeta(function(event) {
        var info = "";
        for (var key in data) {
            info += key + " = " + data[key] + "<BR>";
        }
        print("event", event);
    });
 
    jwplayer('player').play();
}

 Now if you access http://localhost:8080/wowza_web_app/index.html in browser, the page should render.

Testing the app (localhost)
⇒ Start Wowza Enginer Mangaer from Start => All Programs. Once started you should able to access http://localhost:8088/enginemanager/ in PC browser.

⇒ Make sure that the mobile and the PC are in the same wifi network.

⇒ Use Android Terminal Emulator app to check communication between mobile and PC. If you execute “ping pc_ip_address“, you should receive some data.

⇒ Try accessing http://your_pc_ip_address:8088/enginemanager/ in mobile browser. You should see wowza engine manager home page.

Once you are sure that the PC and mobile are in the same network, open both the apps (android & web) and click on Start in web page. You should able to see camera video streamed to webpage.

Exploring the libstreaming
The android app that I have created in this tutorial is very simple which lacks of lot options like start/stop video stream, changing video quality, using camera flash, using front and rare cameras. My intention was to make it as much as simple, so that every beginner can understands it. But the example given by libstreaming covers all these advanced options.

Download the Example3 and do explore yourself.https://github.com/fyhertz/libstreaming-examples


Hosting the wowza server (going live)
If you are serious about going the app to be public where anybody can stream their camera video over intenet, you need to host the Wowza engine on a server. The following links will gives you a hosting solution to host Wowza Media Engine (personally I haven’t tried though).

Google Cloud Platform Hosting
http://googlecloudplatform.blogspot.in/2013/12/you-can-now-deliver-any-screen.html

Wowza Featured Service Providers
http://www.wowza.com/customers
