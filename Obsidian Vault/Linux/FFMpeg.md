https://ffmpeg.org/

This library is an amazing piece of software that requires learning to fully utilize but it mostly used by me for transcoding and compatibility of videos and audio files.

On Fedora and [[Fedora Silverblue]], FFMpeg is not installed by default due to licensing and proprietary code constraints.

It is easy enough to install it from the [[RPMFusion]] repo though.


### Batch Convert WEBM to MP3
[CITE - Stack Overflow](https://stackoverflow.com/posts/66798358/timeline)
[[Youtube-DL]]
You can convert through this script:

```
for FILE in *.webm; do
    echo -e "Processing video '\e[32m$FILE\e[0m'";
    ffmpeg -i "${FILE}" -vn -ab 128k -ar 44100 -y "${FILE%.webm}.mp3";
done;
```

Save it into a .sh file and execute it. It will automatically convert all the .webm into .mp3
