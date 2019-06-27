---
layout: default
title: Authentication
nav_order: 2
---

# Authentication
{: .no_toc }

## Obtaining the API Keys

Our REST API uses Basic Authentication. In order to start developing with our REST API, you will need a username and password.

Log into the [application](https://application.acceptemail.com/) and select Settings under the Account menu-tab (your account needs Admin or Settings-rights to access this).

At the REST API Keys Settings, enter a username and let the app generate the keys
<div class='embed-container'><iframe src='https://player.vimeo.com/video/255000672' frameborder='0' webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe></div>


### Rotating the API Keys
Both keys will work for authentication, and you can renew them one by one. This enables you to rotate the keys without downtime if you want to change them. You can use the secondary key as you renew the primary.

<div class='embed-container'><iframe src='https://player.vimeo.com/video/254999563' frameborder='0' webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe></div>

To test the keys, you can use them to log in on the top right corner of the Swagger documentation: [https://api.acceptemail.com/swagger/ui/index#!/Bill/Bill_Post](https://api.acceptemail.com/swagger/ui/index#!/Bill/Bill_Post)