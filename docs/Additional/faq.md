# FAQ

I've installed Bazarr using Windows Installer and I'm stuck on version 0.9.1. What should I do


Why doesn't Bazarr find any subtitles

### How do I reset my password?

[comment]: <> (What password is referenced here?)

Edit your `config.ini` and change your auth type to None and restart Bazarr.

![!How do I reset my password](images/config-ini.png)

Your config.ini can be found in your [in the same location where your logs or database are](#where-can-i-find-the-logs-or-database)

### Synchronization failed

### Which external subtitles does Bazarr recognize?

Bazarr recognizes the following subtitle types:

- .srt
- .sub
- .smi
- .txt
- .ssa
- .ass
- .mpl
- .vtt

### Why doesn't Bazarr support subtitle provider `XYZ`?

Other users might have already added a feature request [to the backlog](https://bazarr.featureupvote.com/). Feel free to upvote an existing feature request or create a feature request yourself.

[Here you can see which providers are in the motion of being supported.]()

Additionally you can [create a pull request for Bazarr]() and submit your implementation.

### I would like to see the following Feature in Bazarr