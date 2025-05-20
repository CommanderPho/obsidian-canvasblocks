```canvasblocksettings
{
	"type": "workflow",
	"ioConnections":
	{
		"Text": {
			"direction": "input",
			"type": "text"
		}
	}
}
```

```pycanvasblock
# install_dependency("numpy", "np")
import numpy as np

good_session_names = ['2006-6-08_14-26-15',
 '2006-6-09_1-22-43',
 '2006-6-12_15-55-31',
 '2006-6-07_16-40-19',
 '2006-6-08_21-16-25',
 '2006-6-09_22-24-40',
 '2006-6-12_16-53-46',
 '2006-4-09_17-29-30',
 '2006-4-10_12-25-50',
 '2006-4-09_16-40-54',
 '2006-4-10_12-58-3',
 '11-02_17-46-44',
 '11-02_19-28-0',
 '11-03_12-3-25',
 'fet11-01_12-58-54']
 
single_block_height = 120
#for i in np.arange(7):
for i, a_sess_name in enumerate(good_session_names):
	create_text_node(f"{a_sess_name}", script_data["x"]+script_data["width"], script_data["y"]+script_data["height"]+(float(i)*float(single_block_height)))

```
