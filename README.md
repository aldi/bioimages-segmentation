Bioimages Segmentation
====================

Bioimages annotation tool based on image segmentation with admin panel and upload functionalities.

 * Label image regions with the mouse.
 * Image segmentation is written in vanilla Javascript, with require.js dependency (packaged).
 * Pure client-side implementation of image segmentation.
 * Server-side implementation of frame selection, video and frames upload.

A browser must support HTML canvas to use this tool.

There is an [online demo](https://aldiduzha.com/projects/bioimages-segmentation).


Technologies used
---------
*HTML, CSS (Bulma, Bootstrap), JavaScript (Require.js), PHP (Laravel), MySQL*

Features
---------

Server-Side:
 - Admin login to upload videos and frames
 - After uploading video: Choose frame as thumbnail
 - Keyboard shortcuts for frame selection 
 - Select frame to edit
 - Add notes to individual frames     
 - Add tags to individual frames    
 - Save frames after editing     
 - Delete saved frames   
 - Block duplicate frames

Client-Side:

 - 3 tools for image segmentation (Polygon, Superpixel, Brush)
 -  Undo-Redo Actions 
 - Denoise function 
 - Zoom image 
 - Change image boundaries 
 - Change image opacity

Importing videos and frames
--------------

You can import videos and frames after logging in to the admin panel.
There are navigation items on the left side of the admin panel to help you.

Visitor Pages
--------------

There are 4 pages in the application.
 - [Videos Page](http://aldiduzha.com/projects/bioimages-segmentation).
 - [Frames Page](https://aldiduzha.com/projects/bioimages-segmentation/segmentation/?view=index)
 - [Frames Select Page](https://aldiduzha.com/projects/bioimages-segmentation/segmentation/?view=edit&id=0)
 - [Edit Page](https://aldiduzha.com/projects/bioimages-segmentation/segmentation/?view=edit&id=0)

Admin Pages
--------------
You can login [here](https://aldiduzha.com/projects/bioimages-segmentation/login)

You can upload from there videos and frames.

 - [Video Upload](https://aldiduzha.com/projects/bioimages-segmentation/admin/upload)
 - [Frame Upload](https://aldiduzha.com/projects/bioimages-segmentation/admin/uploadframe)

There are navigation items on the left side of the admin panel to browse through.
There is a settings page to change your password too.

Installation and Requirements
-----------

 - Import my **database** (Remember to change the details of the database in the environment .env file)
 - **Laravel** folder goes to **parent** folder.
 - bioimages-segmentation folder goes to your subpage (**/projects**) folder. (In my case  aldiduzha.com/projects/bioimages-segmentation)
Done! You can now use the web app.

You need these on your server:
 - PHP: 7.2.x + 
 - MySQL - 5.1.x +

There is a .env file in the **laravel** folder to change the database details if you need to.

Known issues
-----------

_Browser incompatibility_

A segmentation result can greatly differ due to the difference in JavaScript
implementation across Web browsers. The difference stems from the numerical
precision of floating-point numbers, and there is no easy way to produce the
same result across browsers.


Python tips
-----------

_Annotation PNG_

The annotation PNG file contains label map encoded in RGB value. Do the
following to encode an index map.

```python
import numpy as np
from PIL import Image

# Decode
encoded = np.array(Image.open('data/annotations/1.png'))
annotation = np.bitwise_or(np.bitwise_or(
    encoded[:, :, 0].astype(np.uint32),
    encoded[:, :, 1].astype(np.uint32) << 8),
    encoded[:, :, 2].astype(np.uint32) << 16)

print(np.unique(annotation))

# Encode
Image.fromarray(np.stack([
    np.bitwise_and(annotation, 255),
    np.bitwise_and(annotation >> 8, 255),
    np.bitwise_and(annotation >> 16, 255),
    ], axis=2).astype(np.uint8)).save('encoded.png')
```

_JSON_

Use JSON module.

```python
import json

with open('data/example.json', 'r') as f:
    dataset = json.load(f)
```

_Using dataURL_

Do the following to convert between dataURL and NumPy format.

```python
from PIL import Image
import base64
import io

# Encode
with io.BytesIO() as buffer:
    encoded.save(buffer, format='png')
    data_url = b'data:image/png;base64,' + base64.b64encode(buffer.getvalue())

# Decode
binary = base64.b64decode(data_url.replace(b'data:image/png;base64,', b''))
encoded = Image.open(io.BytesIO(binary))
```


Matlab tips
-----------

_Annotation PNG_

The annotation PNG file contains a label map encoded in RGB value. Do the
following to encode an index map.

```matlab
% Decode

X = imread('data/annotations/0.png');
annotation = X(:, :, 1);
annotation = bitor(annotation, bitshift(X(:, :, 2), 8));
annotation = bitor(annotation, bitshift(X(:, :, 3), 16));

% Encode

X = cat(3, bitand(annotation, 255), ...
           bitand(bitshift(annotation, -8), 255), ...
           bitand(bitshift(annotation, -16)), 255));
imwrite(uint8(X), 'data/annotations/0.png');
```

License
-----------
BSD 3-Clause License
