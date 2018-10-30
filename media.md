[//]: # (tags: media youtube youtube-dl mp3 ffmpeg convert)

### download youtube playlist
`youtube-dl -f bestaudio "https://www.youtube.com/watch?v=zW_rCTRfkzc&index=1&list=PLZdFArl7QXgvVHh1hLFXfXQno2SCr6ely" -ciw --restrict-filenames -o “%(playlist_index)s-%(title)s.%(ext)s"`

### convert webm files from youtube-dl to mp3 ffmpeg:
`ffmpeg -i Beethoven-5th.webm -acodec libmp3lame -aq 4 Beethoven-5th.mp3`

### mass convert in folder and rename:
`for i in *.webm; do ffmpeg -i $i -acodec libmp3lame -aq 4 `basename $i .webm`.mp3; done`
