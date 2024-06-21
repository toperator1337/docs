---
sidebar_position: 1
title: "Tools"
---

# Tools Documentation

`Tools` allow to to add specific, callable functionality and capability to the AI LLMs hosted within OpenWebUI. You can be super creative with this, letting your AI user perform an unlimited number of customizable actions such as reaching out to an external API, modifying a file, or connecting to an external data source. The potential is only limited by your imagination. &#x1F60A;

## Installation

e
Class Definition

Tools

The Tools class contains methods that help interact with a memory storage service.


Attributes


citation: A boolean attribute initialized to True.


Methods

__init__(self)

Constructor method to initialize the Tools class. Sets the citation attribute to True.


python
Run
Copy Code
def __init__(self):
    self.citation = True
    pass
add_memory(self, content: str, __user__: dict = {}) -> str

Asynchronously adds a memory to the user's memory storage by making an HTTP POST request.


Parameters:



content (str): The memory content that the user wants to add.

__user__ (dict, optional): A dictionary containing user information, specifically the user's ID.


Returns:



str: A string confirming that the memory has been added or an error message if the operation fails.

# Example Tool (Add to Memory)

The below is an example Tool that, when prompted for use through the AI LLM, will add a specified memory to the memory store.

```python
import os
import requests
from utils.utils import create_token
from aiohttp import ClientSession


class Tools:
    def __init__(self):
        self.citation = True
        pass

    async def add_memory(self, content: str, __user__: dict = {}) -> str:
        """
        Add a memory to users memories
        :param content: the memory content that the user wants to add.
        :return: a string that confirms the memory has been added.
        """

        url = "http://127.0.0.1:8080/api/v1/memories/add"
        print("url", url)
        print("content", content)

        try:
            async with ClientSession() as session:
                async with session.post(
                    url,
                    json={"content": content},
                    headers={
                        "Authorization": f"Bearer {create_token(data={'id': __user__['id']})}"
                    },
                ) as response:
                    print(response)
                    response_data = await response.json()

            print(f"data:{response_data}")
            return f"Memory Added: {response_data}"
        except Exception as e:
            print(e)
            return f"Error adding memory {str(e)}"

## Tool Usage

1. Ensure that the tool is selected in the + menu on the lefthand side of the dialog entry bar. 
2. Give the AI an explicit instruction `Add to memory: "User loves chocolate"`


URL Initialization:

url is set to the API endpoint http://127.0.0.1:8080/api/v1/memories/add.



Print Statements:

url and content are printed for debugging purposes.



Asynchronous HTTP Request:

A new ClientSession is created.

An asynchronous POST request is made to the url with the following payload:

json: A dictionary containing the content to be added.

headers: A dictionary containing the Authorization header with a Bearer token created using create_token.





Response Handling:

The response from the server is awaited and parsed into response_data.

The response data is printed for debugging purposes.

A success message with the response_data is returned.



Exception Handling:

Any exceptions that occur during the process are caught and printed.

An error message indicating the failure is returned.




Usage Example

python
Run
Copy Code
import asyncio
from your_module import Tools

# Initialize the Tools class
tools = Tools()

# Define memory content and user information
memory_content = "This is a new memory."
user_info = {"id": 12345}

# Use an asynchronous context to call add_memory
async def main():
    result = await tools.add_memory(memory_content, user_info)
    print(result)

# Run the async function
asyncio.run(main())
Replace your_module with the actual module name where the Tools class is defined.


Notes


Ensure that the create_token function in utils/utils.py correctly generates a valid token for authentication.

The URL http://127.0.0.1:8080/api/v1/memories/add should be replaced with the actual API endpoint URL.