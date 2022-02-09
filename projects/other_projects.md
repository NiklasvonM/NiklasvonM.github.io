## Other Small Projects

### Parsing Files

To backup all music files that I store on my computer, I wrote a short Python script that gets all file names that are in a specific format (e.g. mp3 or .wma) from a directory and tries to parse the artist and song name from the file name.

```python
import os
from datetime import datetime, date

path = r'D:\Musik'

list_of_files = []
file_extensions = set()
allowed_extensions = {'.aac', '.mp3', '.mp4', '.wma', '.m4a'}

for root, dirs, files in os.walk(path):
    for file in files:
        filename, file_extension = os.path.splitext(file)
        file_extensions.add(file_extension)
        filename = filename.replace(";", ",")
        if file_extension in allowed_extensions:
            # Assume that the interpret is to the left and the song name is to the right of ' - '.
            if (len(filename.split(' - ')) == 2):
                interpret, song = filename.split(' - ')
            else:
                interpret = ''
                song = ''
            creation_time = datetime.fromtimestamp(os.stat(os.path.join(root, file)).st_ctime).strftime("%Y-%m-%d")
            list_of_files.append([interpret, song, filename, creation_time])

print(file_extensions)
for i in range(5):
    print(list_of_files[i])
    
file = open('Songs Stand ' + str(date.today()) + '.csv', 'w', encoding = 'utf-8')
file.write('Interpret;Song;Titel;Erstelldatum\n')
for tuple in list_of_files:
    file.write(';'.join(tuple) + "\n")
    
file.close()
```

The output .csv then looks something like this:
<img src="images/other_projects/music_csv.png?raw=true"/>
