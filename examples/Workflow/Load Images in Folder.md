---
modified: 2025-05-20T04:17:33-04:00
---
```canvasblocksettings
{
	"type": "workflow",
	"ioConnections":
	{
		"ImageParentFolder": {
			"direction": "input",
			"type": "file"
		},
			
	}
}
```

```pycanvasblock
install_dependency("pillow", "PIL")
from PIL import Image
import base64, io
install_dependency("pathlib")
from pathlib import Path

def build_img_html_tag(img):
    # Convert the image to bytes
    img_byte_array = io.BytesIO()
    img.save(img_byte_array, format=img.format)
    img_byte_array = img_byte_array.getvalue()

    # Convert the bytes to base64
    base64_str = base64.b64encode(img_byte_array).decode('utf-8')

    # Create HTML image tag
    html_img_tag = f'<img src="data:image/png;base64,{base64_str}" alt="Image">'
    return html_img_tag


directory_path = in_data["ImageParentFolder"]
# print(f'directory_path: {directory_path}')
if isinstance(directory_path, dict):
	directory_path = directory_path.get('text', None) # extract the text string
print(f'directory_path: {directory_path}')

if not isinstance(directory_path, Path):
	directory_path = Path(directory_path) #.resolve()

print(f'directory_path: {directory_path}')
assert directory_path.exists(), f'directory_path: "{directory_path}" does not exist!'


# Convert to Path object if it's a string
directory = Path(directory_path)

# Get all PNG files in the directory
png_files = list(directory.glob("*.png"))

# Load each image as a PIL Image object
images = {}

node_ids = []

## Create a group to contain all the images
# group_id = create_group(f"Images from {directory.name}", script_data["x"], script_data["y"]+script_data["height"]+50)

limit_max_num_nodes_added = 6

num_nodes_added: int = 0
curr_horizontal_offset_x: int = 0

for file_path in png_files:
    if (limit_max_num_nodes_added is None) or (num_nodes_added >= limit_max_num_nodes_added):
        print(f'skipping due to too many nodes!')
    else:
        # try to add new node		
        try:
            img = Image.open(file_path)
            # Use the filename (without extension) as the key
            filename = file_path.stem
            images[filename] = img
            # Calculate height to maintain aspect ratio
            img_width = img.size[0]
            img_height = img.size[1]
            
            # ratio = width / float(img.size[0])
            # height = int(ratio * img.size[1])
            
            html_img_tag = build_img_html_tag(img)
            # Create the node and get its ID
            node_id = create_text_node(html_img_tag, (script_data["x"] + curr_horizontal_offset_x), script_data["y"]+script_data["height"]+120, width=img_width, height=img_height)
			# Add the node ID to our list
            node_ids.append(node_id)
            curr_horizontal_offset_x += img_width
            num_nodes_added += 1
        
        except Exception as e:
            print(f"Error loading {file_path}: {e}")


# Add all nodes to the group
add_nodes_to_group(group_id, node_ids)

# Optionally, notify the user
notice(f"Added {num_nodes_added} images to group")

```
