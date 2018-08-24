## Rocket.Chat can provide OAuth2 login to Wekan

Also, if you have Rocket.Chat using LDAP/SAML/Google/etc for logging into Rocket.Chat, then same users can login to Wekan when Rocket.Chat is providing OAuth2 login to Wekan.

Source: [OAuth2 Pull Request](https://github.com/wekan/wekan/pull/1578)

## 1) Install Rocket.Chat

[Rocket.Chat Snap](https://rocket.chat/docs/installation/manual-installation/ubuntu/snaps/) has Node at port 3000 and mongodb at port 27017.
```
sudo snap install rocketchat-server
```

## 2) Install Wekan

[Wekan Snap](https://github.com/wekan/wekan-snap/wiki/Install) has Node at port 3001 and MongoDB at port 27019.
```
sudo snap install wekan
sudo snap set wekan root-url="https://wekan.example.com"
sudo snap set wekan port='3001'
sudo snap set core refresh.schedule=02:00-04:00
sudo snap set wekan with-api='true'
```
Email settings [ARE NOT REQUIRED](https://github.com/wekan/wekan/wiki/Troubleshooting-Mail), Wekan works without setting up Email.
```
sudo snap set wekan mail-url='smtps://user:pass@mailserver.example.com:453'
sudo snap set wekan mail-from='Wekan Boards <support@example.com>'
```
Edit Caddyfile:
```
sudo nano /var/snap/wekan/common/Caddyfile
```
Add Caddy config:
```
wekan.example.com {
        proxy / localhost:3001 {
          websocket
          transparent
        }
}

chat.example.com {
        proxy / localhost:3000 {
          websocket
          transparent
        }
}
```
Enable Wekan's Caddy:
```
sudo snap set caddy-enabled='true'
```

## 3) Add Rocket.Chat settings

Login to Rocket.Chat at https://chat.example.com .

Accept chat URL to be https://chat.example.com .

Click: (3 dots) Options / Administration / OAuth Apps / NEW APPLICATION

Add settings:

```
Active: [X] True
Application Name: Wekan
Redirect URI: https://wekan.example.com/_oauth/oidc
Client ID: abcde12345         <=== Rocket.Chat generates random text to here
Client Secret: 54321abcde     <=== Rocket.Chat generates random text to here
Authorization URL: https://chat.example.com/oauth/authorize
Access Token URL: https://chat.example.com/oauth/token
```
Save Changes.

## 4) Add Wekan settings

```
sudo snap set wekan oauth2-client-id='abcde12345'
sudo snap set wekan oauth2-secret='54321abcde'
sudo snap set oauth2-server-url='https://chat.example.com'
sudo snap set oauth2-auth-endpoint='/oauth/authorize'
sudo snap set oauth2-userinfo-endpoint='/oauth/userinfo'
sudo snap set oauth2-token-endpoint='/oauth/token'
```

## 5) Login to Wekan

1) Go to https://wekan.example.com

2) Click `Sign in with Oidc`

3) Click `Authorize` . This is asked only first time when logging in to Wekan with Rocket.Chat.

<img src="https://wekan.github.io/oauth2-login.png" width="60%" alt="Wekan login to Rocket.Chat" />

## 6) Set your Full Name

Currently Full Name is not preserved, so you need to change it at your username / Profile.

<img src="https://wekan.github.io/oauth2-profile-settings.png" width="60%" alt="Wekan login to Rocket.Chat" />
