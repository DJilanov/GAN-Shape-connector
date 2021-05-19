# GAN - Shape connector
## _Information about our iframe connector created for Jack_

The problems we are solving is the integration between the old Java crud and the new API which is used at Shape login dialog

## Theory behind the code
By using two completelly different systems we need to somehow transfer information between them. The easiest way to do it, is with parameters in the header of the pages you load from ours.

## How to implement it
When you fire a basic login request to our login service\
https://johsports.alpha.gameaccount.com/api/login/jackOH/credentials

you recieve some login information looking like this

`
{
   "reference":"a4597c02-c44a-4096-9a5b-93abb0ba65b5",
   "status":"SUCCESS",
   "error":null,
   "challenge":null,
   "data":{
      "player":{
         "playerGuid":"14C4E579-AF09-43A6-B779-6ACF54082414",
         "externalPlayerRef":null,
         "firstName":"Fernando",
         "lastName":"Abolafio",
         "currency":"USD",
         "alias":"fernando",
         "email":"fernando@shape.dk",
         "playerId":4380477
      },
      "token":"031e27f8-7ef4-4c5e-a292-c40a47fc71db",
      "sessionId":3694358,
      "sessionStartTime":1620727029334,
      "cookies":[
         {
            "name":"JSESSIONID",
            "value":"935446D103855744AF08BCDC6C96842E.jboss",
            "domain":null,
            "path":"/",
            "secure":true,
            "httpOnly":true,
            "maxAge":0
         }
      ],
      "lastLogin":1620719822770,
      "properties":{
         "kambiToken":"71a82c4f-bcbd-48f5-a931-34ccc95a8575"
      }
   }
}
`

Once you have it, you must take couple of the parameters inside\
`const playerGuid = res.data.player.playerGuid;`\
`const token = res.data.token;`\
`const sessionId = res.data.sessionId;`\
`const lastLoginDate = (new Date(res.data.lastLogin)).toISOString();`\

and put it into the `<iframe>` we are loading like\
`'https://johsports.alpha.gameaccount.com/profile.shtml?playerGuid=' + playerGuid + '&token=' + token + '&sessionId=' + sessionId + '&lastLoginDate=' + parts[0] + 'T' + parts[1].substring(0,8)`

By doing this, we transfer all the needed info between your system and our system and our system.
