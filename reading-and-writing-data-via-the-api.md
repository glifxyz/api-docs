# 📙 Reading & writing data

<Warning>
These endpoints are highly beta and subject to change
</Warning>

For fetching info about a specific glif, user, or a specific glifRun.

Paginate using `?page=` when applicable. You can also increase the # of results per page using e.g. `?limit=50`, currently defaulting to 20 and capped at 100

## Individual glif

Fetch a specific glif: GET <https://glif.app/api/glifs?id=clgh1vxtu0011mo081dplq3xs>

Includes full glif-graph JSON in the `data` field

## Lists of glifs

Fetch all public glifs: GET <https://glif.app/api/glifs>

Fetch featured glifs on homepage: GET <https://glif.app/api/glifs?featured=1>

Fetch user's glifs by username: GET <https://glif.app/api/glifs?username=jamiedubs>

Fetch user's glifs by user ID: GET <https://glif.app/api/glifs?userId=clcu26uxx0000lc08mqnjf5mq>

To get the full glif-graph JSON in the `data` field, append `?includes=spells.data`

## Runs for a given glif

Fetch a specific run: GET <https://glif.app/api/runs?id=i3kqrilvp6rmsszwnw44ycyl>

Fetch all public runs for a specific glif: GET <https://glif.app/api/runs?glifId=clgh1vxtu0011mo081dplq3xs> (`?spellId=` also works)

Fetch all public runs by a specific user:

- by username: GET <https://glif.app/api/runs?username=jamiedubs>
- by userId: GET <https://glif.app/api/runs?userId=clcu26uxx0000lc08mqnjf5mq>

## Create a glif

Currently in private testing; [contact us](https://glif.app/contact) if you're messing with this!

Here's a `curl` example which creates an empty, unpublished (draft) glif:

```sh
curl -X POST "https://glif.app/api/glifs" \
     -H "Authorization: Bearer $GLIF_API_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{
       "name": "API test glif",
       "description": "just testing :)",
       "graph": {"nodes": []}
     }'
```

Here's a curl example that includes one text input and an LLM call (GPTBlock) copied from this [Perplexity search](https://glif.app/@jamiedubs/glifs/clxkuj2p30000y8tecjvkajog) glif.

Please note that formatting complex JSON inside a shell command like this is a huge pain in the butt and is not recommended for serious development :)

```sh
curl -X POST "https://glif.app/api/glifs" \
  -H "Authorization: Bearer $GLIF_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "API test glif 2",
    "description": "just testing again, I love APIs",
    "graph": {
      "nodes": [
        {
          "name": "query",
          "type": "TextInputBlock",
          "params": {
            "label": "query",
            "value": "what is trending in pop culture this week?"
          }
        },
        {
          "name": "result",
          "type": "GPTBlock",
          "params": {
            "model": "Perplexity: Llama3 Sonar 8B Online",
            "prompt": "{query}",
            "jsonMode": false,
            "maxTokens": 1000,
            "temperature": 0.2,
            "systemPrompt": "You are a helpful assistant."
          }
        }
      ]
    }
  }'
```

Here's an example response on success for that call:

```json
{"id":"clzs7eoan0003n1xgod8grhaj","name":"API test glif 2","imageUrl":null,"description":"just testing again, I love APIs","createdAt":"2024-08-13T09:12:02.688Z","updatedAt":"2024-08-13T09:12:02.688Z","publishedAt":null,"output":null,"outputType":null,"forkedFromId":null,"featuredAt":null,"userId":"clcu26uxx0000lc08mqnjf5mq","completedSpellRunCount":0,"averageDuration":null,"user":{"id":"clcu26uxx0000lc08mqnjf5mq","name":"jamiedubs","image":"https://res.cloudinary.com/dzkwltgyd/image/upload/v1709593997/user-image-production/ckkbd7jmxly9ms0csaxv.jpg","username":"jamiedubs"},"_count":{"likes":0,"comments":0},"spheres":[],"data":{"nodes":[{"name":"query","type":"TextInputBlock","params":{"label":"query","value":"what is trending in pop culture this week?"}},{"name":"result","type":"GPTBlock","params":{"model":"Perplexity: Llama3 Sonar 8B Online","prompt":"{query}","jsonMode":false,"maxTokens":1000,"temperature":0.2,"systemPrompt":"You are a helpful assistant."}}]}}
```

Note the glif id, `clzs7eoan0003n1xgod8grhaj`. Also note that this glif is also _unpublished_ and so it is not accessible to any user except you! You can publish it via the website, or by specifying a "publishedAt" date in your POST request.

You can then run it this glif:

```sh
curl -X POST "https://simple-api.glif.app" \
  -H "Authorization: Bearer $GLIF_API_TOKEN" \
  -d '{"id": "clzs7eoan0003n1xgod8grhaj", "query": "who is the coolest software developer of all time?"}'
```

To view raw glif data, you can fork or build something on glif.app and use the "Debug" button bottom-left.

Alternately, it's present in all _published_ glifs via the API, e.g., in order of increasing complexity:

- Simple text generator: <https://glif.app/api/glifs?id=clzs7eoan0003n1xgod8grhaj>
- Audio input with CanvasBlock formatting: <https://glif.app/api/glifs?id=clyimuesi0009445j4laxm8o1>
- ComfyUI-based image transformer: <https://glif.app/api/glifs?id=clz0e47iu0002vr1lyyuwdl0x>

## User info

Fetch info about the logged-in user (and thus test that your API token is working and you are logged-in): GET <https://glif.app/api/me>

Fetch info about a user, via either ID or username:

- GET <https://glif.app/api/user?username=jamiedubs>
- GET <https://glif.app/api/user?id=clcu26uxx0000lc08mqnjf5mq>

## Spheres (beta)

To get glifs from a specific sphere (AKA folder or collection): GET <https://glif.app/api/spheres?slug=memes>

You can also use `?id=` param

Like the 'list of glifs' above, you can fetch the full glif-graph JSON in these list queries by appending `?includes=spells.data`

Public spheres are listed on <https://glif.app/spheres>

## Help/Support/Questions

Please [join our Discord](https://glif.app/discord) and post in the `#api-support` channel if you have any questions or need help with the API. We'd love to hear from you!

Alternately you can [contact us via email](https://glif.app/contact), but those will get slower replies.