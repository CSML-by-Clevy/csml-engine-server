# CSML Engine Server

Run [CSML Conversational Engine](https://csml.dev) on premise!

## Installation

### Docker

The easiest way to start a CSML Engine Server is by using our pre-built docker container. To do that, make sure that you have a mongodb database ready and that you have correctly setup a `.env` file accordingly (see `.env.example` for reference).

```
docker run -p 3000:3000 --env-file .env clevy/csml-engine
```

### Docker-compose

If you also need to start a MongoDB server, you can also use our docker-compose.yml configuration by running:

```
docker-compose up -d
```

Tweak the docker-compose.yml file to your liking. You can use a different MongoDB instance by setting the right environment variables (cf `.env.example`).

## Usage

Verify that CSML Engine is running by calling http://localhost:3000.

### Handle incoming events
```shell
curl -X "POST" "http://localhost:3000/run" \
     -H 'content-type: application/json' \
     -d $'{
  "bot": {
    "default_flow": "Default",
    "id": "mybot",
    "flows": [
      {
        "id": "e0a13373-2037-4590-8018-ab14e74b27a1",
        "content": "start:\\n\\tsay \\"Hello\\"\\n\\tgoto end",
        "commands": [
          "/default"
        ],
        "name": "Default",
        "description": "Default custom flow"
      }
    ],
    "updated_at": "2020-05-28T17:57:20.725Z",
    "name": "MySuperBot"
  },
  "event": {
    "request_id": "d7077041-c81a-4872-820c-dc23e015aa84",
    "client": {
      "user_id": "myuser",
      "channel_id": "mychan",
      "bot_id": "mybot"
    },
    "metadata": {
      "some": "info",
      "about": "the current user"
    }
  }
}'
```
> Note: if `body.event.callback_url` is set, any message issued by the bot will be sent in real-time to this endpoint as a `POST` HTTP request.

### Get open conversation
```shell
curl -X "POST" "http://localhost:3000/conversations/open" \
     -H 'content-type: application/json' \
     -d $'{
  "user_id": "myuser",
  "channel_id": "mychannel",
  "bot_id": "mybot"
}'
```

### Close all open conversations
```shell
curl -X "POST" "http://localhost:3000/conversations/close" \
     -H 'content-type: application/json' \
     -d $'{
  "user_id": "myuser",
  "channel_id": "mychannel",
  "bot_id": "mybot"
}'
```

### Validate Bot
```shell
curl -X "POST" "http://localhost:3000/bots/validate" \
     -H 'content-type: application/json' \
     -d $'{
  "id": "mybot",
  "default_flow": "Default",
  "flows": [
    {
      "commands": [
        "/default"
      ],
      "content": "start:\\n\\tsay \\"Hello\\"\\n\\tgoto end",
      "id": "some-random-id",
      "name": "Default"
    }
  ],
  "name": "MySuperBot"
}'
```

### Get Flow Steps
```shell
curl -X "POST" "http://localhost:3000/get_flow_steps" \
     -H 'content-type: application/json' \
     -d $'{
  "id": "myflow",
  "content": "start:\\n\\tsay \\"Hello\\"\\n\\tgoto end",
  "commands": [
    "/default"
  ],
  "name": "Default"
}'
```
