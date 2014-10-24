# T_1000
**Points:** 120
**Category:** Recon
> Who made me ?

> Hint: Do you know WhoIs T_1000 ?

## Write up
BOT_T_1000 is a bot that the contestants come across in the challenge ECTF_Hacked? on the channel #nitk-maliciousbots.
On querying WhoIs on the bot as suggested in the hint it reveals a twitter handle:
> [@31337_h4X0R](https://twitter.com/31337_h4X0R)

On going through the tweets, we can find a tweet with a link to an image hosted on our server (also the description on the profile says he's a graphic designer. Maybe he's proud of his work?). On downloading the same and checking the author field (using exiftool) the flag is obtained.

> flag{I_am_N0t_Ge0Hot}

## External Write ups
