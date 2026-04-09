first use binwalk to extract file 

<img width="2211" height="581" alt="image" src="https://github.com/user-attachments/assets/1c57d826-d812-47bc-8e52-0d3ba22d3a60" />

find the file with extension is .gd, because .gd file is plaintext so we can easily read them 

<img width="1445" height="504" alt="image" src="https://github.com/user-attachments/assets/e3c37037-6f89-40db-9263-8f6107f3bb8d" />

read the FlagLabel.gd

```
extends Label

var hidden_content

func _ready():
	var key = [66, 121, 84, 51, 99, 48, 100, 51]
	var enc = [153, 222, 192, 159, 131, 148, 211, 161, 167, 165, 116, 167, 203, 149, 132, 153, 174, 218, 187, 83, 204, 163, 110, 117, 187, 237, 135, 150, 147, 148, 151, 118, 118, 231, 168, 133, 150, 163, 149, 166, 150]
	
	hidden_content = ""
	for i in range(len(enc)):
		hidden_content += char(enc[i] - key[i % len(key)])
	
	text = "nothing to see\nhere!"
```

therefore, we can find the flag 
