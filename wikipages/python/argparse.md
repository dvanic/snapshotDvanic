---
layout: page
---

```python
import argparse
import sys

def process_command_line_and_env_settings():
   ## Set up parser
   parser = argparse.ArgumentParser(description='Convert jason of paperpile notes to nice markdown text')
   ## Add arguments
   parser.add_argument('inputfile', type=str, help='The jason file with the notes')

   # Display the help message if you haven't specified any arguments
   if len(sys.argv)==1:
         parser.print_help()
         sys.exit(1)
   # Parse the arguements
   args = parser.parse_args()
   return args
   
```
