[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/LisadiKaprio/rgb-to-grayscale-bmp/blob/main/RGBtoGrayscaleBMP_2.ipynb)

Following code converts a normal png into a grayscale bmp file. This tool should make it easier to create scatter brushes for SAI 2.

Click the little folder icon on the left.
Create a folder called "pngs" and put all of your png files there. Ignore the "sample_data" folder completely.
Run the code below by clicking the Play button. Images will start converting. Download should start automatically once conversion is done.
Credit to original conversion code: https://stackoverflow.com/a/46388479

```python
from PIL import Image
import numpy as np
import os

original_pngs = '/content/pngs/'
finished_bmps = '/content/finished_bmps/'

!mkdir finished_bmps

for filename in os.listdir(original_pngs):
    if filename.endswith('.png'):
        img = Image.open(original_pngs + filename)
        ary = np.array(img)

        # Split the three channels
        r,g,b = np.split(ary,3,axis=2)
        r=r.reshape(-1)
        g=r.reshape(-1)
        b=r.reshape(-1)

        # Standard RGB to grayscale 
        bitmap = list(map(lambda x: 0.299*x[0]+0.587*x[1]+0.114*x[2], 
        zip(r,g,b)))
        bitmap = np.array(bitmap).reshape([ary.shape[0], ary.shape[1]])
        bitmap = np.dot((bitmap > 128).astype(float),255)
        im = Image.fromarray(bitmap.astype(np.uint8))
        filename = filename[:-4]
        im.save(finished_bmps + filename + '.bmp')
        print(filename + ' was converted')

# DOWNLOADING
!zip -r /content/finished_bmps.zip /content/finished_bmps
from google.colab import files
files.download("/content/finished_bmps.zip")
```

The snippet below allows you to duplicate an original .ini configuration file for every png in your pngs folder. This way, you'll get the needed .ini configuration file for every brush you've created, all in a matter of seconds!

1. Click the folder icon on the left.
2. Insert original .ini file there, it should be named "template.ini". 

*You can take the default Stars.ini file from the SAI's "scatter" folder, and just rename it. Or you can take this file and preconfigure it to your liking by just opening it in notepad or even doubleclicking it here in colab, and then editing relevant values.*

3. Run the code below by clicking the Play button. New inis will start generating. Download of the whole inis.zip archive should start automatically once it's done.

```python
import os
import shutil

!mkdir inis

template_ini = '/content/template.ini'
original_pngs = '/content/pngs/'

for filename in os.listdir(original_pngs):
    if filename.endswith('.png'):
        filename = filename[:-4]
        shutil.copy(template_ini, "/content/inis/" + filename + ".ini")

# DOWNLOADING
!zip -r /content/inis.zip /content/inis
from google.colab import files
files.download("/content/inis.zip")
```
