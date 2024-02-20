aiocometd-ng
=========

aicometd-ng is a Python 3.10+ compatible fork of https://github.com/robertmrk/aiocometd

Usage
-----

.. code-block:: python

    import asyncio

    from aiocometd import Client

    async def chat():
        nickname = "John"

        # connect to the server
        async with Client("http://example.com/cometd") as client:

                # subscribe to channels to receive chat messages and
                # notifications about new members
                await client.subscribe("/chat/demo")
                await client.subscribe("/members/demo")

                # send initial message
                await client.publish("/chat/demo", {
                    "user": nickname,
                    "membership": "join",
                    "chat": nickname + " has joined"
                })
                # add the user to the chat room's members
                await client.publish("/service/members", {
                    "user": nickname,
                    "room": "/chat/demo"
                })

                # listen for incoming messages
                async for message in client:
                    if message["channel"] == "/chat/demo":
                        data = message["data"]
                        print(f"{data['user']}: {data['chat']}")

    if __name__ == "__main__":
        asyncio.run(chat())

Documentation
-------------

https://aiocometd.readthedocs.io/

Install
-------

.. code-block:: bash

    pip install aiocometd-ng

Requirements
------------

- Python 3.10+
- aiohttp_

.. _aiohttp: https://github.com/aio-libs/aiohttp/
.. _aiocometd: https://github.com/robertmrk/aiocometd
.. _CometD: https://cometd.org/
.. _Comet: https://en.wikipedia.org/wiki/Comet_(programming)
.. _asyncio: https://docs.python.org/3/library/asyncio.html
.. _Bayeux: https://docs.cometd.org/current/reference/#_bayeux
.. _ext: https://docs.cometd.org/current/reference/#_bayeux_ext
.. _cli_example: https://github.com/robertmrk/aiocometd/blob/develop/examples/chat.py
.. _aiocometd-chat-demo: https://github.com/robertmrk/aiocometd-chat-demo
