# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code of the various project contained in this folder.

## What is Angel-IA
 The Angel-IA service is an assistant through an avatar for aged persons, potentially with disabilities, to propose them activities (gives weather, warn for appointment, story telling, ...) or directs discussions or answers to whatsapp/phone calls. 
 It is mainly developped in java with well structured small classes. 
 It is hosted in github in various repositories :
 - angel-dl4j-detection-models (private) : "https://github.com/rbaudu/angel-dl4j-detection-models".
 - angel-server-capture (public) : "https://github.com/rbaudu/angel-server-capture".
 - angel-virtual-assistant (public) : "https://github.com/rbaudu/angel-virtual-assistant".
 - angel-update-service (private) : "https://github.com/rbaudu/angel-update-service".
 - angel-install-on-pi (public) : "https://github.com/rbaudu/angel-install-on-pi".
 - synovant-website (private) : "https://github.com/rbaudu/synovant-website". 

'**angel-dl4j-detection-models**' is deployed on development/prototype/training server to produce Deep Learning models by analysing video, photos, audio of aged persons to determine their presence and situations. It is based on Java and ND4J.
'**angel-server-capture**' is a Java Spring boot  application deployed on a Tablet/PC/Pi of the aged person home and gets continuously the video of the person to detect her current situation using the AI models produces by '**angel-dl4j-detection-models**'.
'**angel-virtual-assistant**'  is a Java Spring boot  application also deployed on the Tablet/PC/Pi of the aged person home and gets continuously the current situation of the person given by '**angel-server-capture**' to propose her activities.

 According to this current situation, the angel virtual assistant will propose or not an activity; if it proposes an activity, it will alternate between many kind of activities and according to the preferences of the person (no story telling if the person does not appreciate for instance). 
 The person may also tells a word close to 'Angèle' and the avatar will listen to direct commands or open questions. The virtual assistant starts in normal mode via './angel-launcher.sh start' and in test mode (no angel-server-capture but simulated activities) via './angel-launcher.sh start -p test'. 
 At startup, The page 'http://localhost:8080/angel' loads an avatar : the loaded page is 'src/main/resources/templates/avatar.html' and uses the javascript under 'src/main/resources/static/js', css under 'src/main/resources/static/css' and avatar configuration file under 'src/main/resources/config/avatar.properties. 
 The main java application 'com.angel.core.AngelApplication' (started as a Spring boot application by 'com.angel.core.SpringBootAngelApplication') has a main configuration file 'config/application.properties' (overwritten by 'config/application-test.properties' in test mode).
The proposed activities are provided to the virtual assistant by the web service '**anegl-update-service**' : this web service provides dynamic contents (news, meteo, ...) 3 times a day, new static contents (story, recipes, riddles, ...) daily, software update (when available), ia model updates (when available).

'**angel-install-on-pi**' is a project to provide the necessary tools and information to be able to prepare a RaspBerry Pi5 with an operationnal Angel-IA.

'**synovant-website**' is the static synovant website deployed on synovant.com at OVH which adescription of Synovant missions but also all what needed for Angel-IA description and pilot programm/inscription.


## Documentation
Each subproject has a README.md and a docs folder containing specific documentation parts of the project.

A rule is that documentation for all these projects must not contain mentions about what has evolved but only the current status of the application (so, NO 'now the activities' neither 'configured has been improved' -> just current facts). Do not put things like 'nouvelle approche' or 'amélioration' or 'plus intelligent' but just facts of what exists and so no comparisons between what has been and what is; this burdens uselessly the documentation without bringing useful information at this stage.
Moreover, no value judgement like 'this approach is more PRAGMATIC' or marketing stuff like 'this is a WONDERFUL architecture'. Only the full and plain facts and that's it!

