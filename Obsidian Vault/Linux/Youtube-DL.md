Use this cool little app to download YouTube videos locally and mash them together in a remix using something like [[Davinci Resolve]]


Here is the documentation
https://github.com/ytdl-org/youtube-dl/blob/master/README.md#readme

Here is a snippet of working code I used in my Ubuntu Distrobox container [[Distrobox]]


`youtube-dl -f bestvideo+bestaudio/best https://youtu.be/fdPu-wvl3KE`

Want to make my boy a straight Cocomelon video without having to switch to different YouTube links or videos


### Convert the DL from default WEBM to MP3
[Stack Overflow](https://stackoverflow.com/a/44929515 "Short permalink to this answer")
[[FFMpeg 1]]
youtube-dl already contains this functionality - just specify that you want mp3:

```
youtube-dl -x --audio-format mp3 https://www.youtube.com/watch?v=BaW_jenozKc
```

Replace `https://www.youtube.com/watch?v=BaW_jenozKc` with your actual URL.


