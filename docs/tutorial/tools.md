---
sidebar_position: 1
title: Tools
---

# Tools Documentation

`Tools` allow you to add specific, callable functionality and capability to the AI LLMs hosted within OpenWebUI. You can be super creative with this, letting your AI user perform an unlimited number of customizable actions such as reaching out to an external API, modifying a file, or connecting to an external data source. The potential is only limited by your imagination. 😊

## Example Tool: Add to Memory

The following is an example Tool that, when prompted for use through the AI LLM, will add a specified memory to the memory store.

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
        Add a memory to users memories.
        
        :param content: The memory content that the user wants to add.
        :param __user__: The user dictionary containing user details.
        :return: A string that confirms the memory has been added.
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
            return f"Error adding memory: {str(e)}"

### Tool Usage

    Ensure that the tool is selected in the + menu on the left-hand side of the dialog entry bar.

    Give the AI an explicit instruction: Add to memory: "User loves chocolate"


### Responses

    The response from the server is awaited and parsed into response_data.

    The response data is printed for debugging purposes.

    A success message with the response_data is returned.


### Exception Handling

    Any exceptions that occur during the process are caught and printed.

    An error message indicating the failure is returned.

