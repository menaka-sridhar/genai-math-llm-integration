## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function to calculate the volume of a cube and integrate it with a chat completion system using the function-calling feature of a Large Language Model (LLM).
### PROBLEM STATEMENT:
To develop a Python program that uses OpenAI Function Calling to identify a volume calculation request, pass the side length to a Python function, calculate the volume of a cube, and display the final result.
### DESIGN STEPS:

#### STEP 1:
Import the required libraries, load the OpenAI API key, and define a Python function to calculate the volume of a cube using the formula \(V = times s \times s \times s\).
#### STEP 2:
Define the function schema and provide the user's request to the LLM. The model identifies the required function and generates the necessary arguments.
#### STEP 3:
Execute the Python function using the arguments returned by the LLM and display the calculated volume.
### PROGRAM:
```
import os
import json
from dotenv import load_dotenv, find_dotenv
from openai import OpenAI

# Load API key
load_dotenv(find_dotenv())
client = OpenAI(api_key=os.environ["OPENAI_API_KEY"])

# Define function
def calculate_volume(side):
    return side * side * side

# Define function schema
tools = [
    {
        "type": "function",
        "function": {
            "name": "calculate_volume",
            "description": "Calculate the volume of a cube",
            "parameters": {
                "type": "object",
                "properties": {
                    "side": {
                        "type": "number",
                        "description": "Side length of the cube"
                    }
                },
                "required": ["side"],
                "additionalProperties": False
            },
            "strict": True
        }
    }
]

# User input
messages = [
    {
        "role": "user",
        "content": "Calculate the volume of a cube with side 5"
    }
]

# Call the LLM
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=messages,
    tools=tools,
    tool_choice="required"
)

# Get function arguments
tool_call = response.choices[0].message.tool_calls[0]
args = json.loads(tool_call.function.arguments)

# Execute function
volume = calculate_volume(args["side"])

# Display output
print(volume)
```
### OUTPUT:
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b351dd5e-6588-40e6-8613-d04978843316" />

### RESULT:
Thus, the Python function for calculating the volume of a cube was successfully integrated with the OpenAI chat completion system using LLM Function Calling, and the volume for a side length of 5 units was calculated as 125 cubic units.
