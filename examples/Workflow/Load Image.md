---
modified: 2025-05-20T03:42:13-04:00
---
```canvasblocksettings
{
	"type": "workflow",
	"ioConnections":
	{
		"ImagePath": {
			"direction": "input",
			"type": "file"
		},
		
		"Image": {
			"direction": "output",
			"type": "image"
		}
	}
}
```

```pycanvasblock
install_dependency("pillow", "PIL")
from PIL import Image
import base64, io
install_dependency("pathlib")
from pathlib import Path

img_path = in_data["ImagePath"]
# print(f'img_path: {img_path}')
if isinstance(img_path, dict):
	img_path = img_path.get('text', None) # extract the text string
print(f'img_path: {img_path}')

if not isinstance(img_path, Path):
	img_path = Path(img_path) #.resolve()

print(f'img_path: {img_path}')
assert img_path.exists(), f'img_path: "{img_path}" does not exist!'
img = Image.open(img_path)
out_data["Image"] = img


# Convert the image to bytes
#img_byte_array = io.BytesIO()
#img.save(img_byte_array, format=img.format)
#img_byte_array = img_byte_array.getvalue()

# Convert the bytes to base64
#base64_str = base64.b64encode(img_byte_array).decode('utf-8')

# Create HTML image tag
#html_img_tag = f'<img src="data:image/png;base64,{base64_str}" alt="Image">'
#create_text_node(html_img_tag, script_data["x"], #script_data["y"]+script_data["height"]+120, 400, 400)

```
