# ⚡ Running glifs via the API

The Glif API is **unstable** and **in limited beta**, but we’re excited to see what y'all build.

**To get started, create your API tokens on <https://glif.app/settings/api-tokens>**

The API is currently free with the same rate limits as normal user accounts; see [FAQ](../getting-started/faqs/) details. This is all subject to change without notice.

Additionally, API users may not run glifs that use any of the following models: StableDiffusion 3, DALL-E 3, Flux Pro, and Flux Dev; or Flux Dev via comfyBlock. Please make sure to follow these rules as we'll otherwise have to revoke your API access.

If you're building with our API, please [join our Discord](https://glif.app/discord) or [get in touch](https://glif.app/contact) since we'd love to chat and learn more about what you're building. For selected apps we can extend a higher rate limit during this free testing period.

{% hint style="warning" %}
We ask that you include “powered by glif.app” and a link to [glif.app](https://glif.app/) within the UI of your integration. Please use a Glif logo badge in the zip file below.
{% endhint %}

{% file src="../.gitbook/assets/glif_badges.zip" %}

## Running glifs using the Simple API

{% hint style="info" %}
**The Simple API requires an API token to make requests**

[Register for your free token here](https://glif.app/settings/api-tokens)
{% endhint %}

{% tabs %}
{% tab title="curl" %}

```javascript
curl -X POST -H "Authorization: Bearer abzfasdf2349820349" -d '{"id": "clgh1vxtu0011mo081dplq3xs", "inputs": ["cute friendly oval shaped bot friend"]}' https://simple-api.glif.app
```

{% endtab %}

{% tab title="Python" %}

```python
import requests

response = requests.post(
    "https://simple-api.glif.app",
    json={"id": "clgh1vxtu0011mo081dplq3xs", "inputs": ["cute friendly oval shaped bot friend"]},
    headers={"Authorization": "Bearer abzfasdf2349820349"},
)
print(response.content)
```

{% endtab %}

{% tab title="Javascript" %}

```javascript
const apiToken = 'abzfasdf2349820349';
const data = {
  id: "clgh1vxtu0011mo081dplq3xs",
  inputs: ["cute friendly oval shaped bot friend"]
};
const response = await fetch('https://simple-api.glif.app',
  {
    method: 'POST',
    headers: {
        'Authorization': `Bearer ${apiToken}`,
        'Content-Type': 'application/json'
    },
    body: JSON.stringify(data)
});
const result = await response.json();
console.log(result);
```

{% endtab %}
{% endtabs %}

Here’s the expected output of that command:

```json
{
  "id": "clgh1vxtu0011mo081dplq3xs",
  "inputs": { "node_6": "cute friendly oval shaped bot friend" },
  "output":"<https://res.cloudinary.com/dzkwltgyd/image/upload/v1686242317/glif-run-outputs/fhbvbp9bwf0pkmm4woj2.png>",
  "outputFull": { "...": "" }
}
```

You're most likely interested in the `output` field, which is the final output of the glif.

`outputFull` is a JSON object with more info about the final output, including it's type.

If there’s an error it’ll be in an `error` field, but the response will have a "200 OK" status code. For legacy reasons the the Simple API always returns 200 OK. This might change in the future.

### Parameters

#### id

You can put the glif id in the url:

```sh
https://simple-api.glif.app/clgh1vxtu0011mo081dplq3xs
```

Or put it in the body:

```json
{
  "id": "clgh1vxtu0011mo081dplq3xs",
  "inputs": ["..."]
}
```

#### inputs

{% hint style="warning" %}
for now only string values are allowed
{% endhint %}

You can either use a positional array:

```json
{
  "inputs": ["a happy horse", "living on a farm", "in France"]
}
```

Or name your parameters. The names have to correspond to your glif-block names. If you don't know the block names, check out [#looking-up-the-internal-names-of-a-glifs-inputs](getting-started.md#looking-up-the-internal-names-of-a-glifs-inputs "mention").

```json
{
  "inputs": {
    "param1": "a happy horse",
    "param2": "living on a farm",
    "param3": "in France"
  }
}
```

### Looking up the internal names of a glif's inputs

To specify named parameters, you need to use the internal block name. This has no spaces and usually is something like "text1", "multipick2", but each glif creator can customize it.

On the glif.app website you can look at the block names in the "View source" page of a glif, [like this one](https://glif.app/@tanaki/glifs/clkbasluf0000mi08h541a3j4/source):

<figure><img src="../.gitbook/assets/CleanShot 2024-04-03 at 10.15.17.png" alt=""><figcaption><p>little diagram on the "View Source" page which shows the names of each block</p></figcaption></figure>

Or you can look at the `nodes` field of a glif's data via our API:

[https://glif.app/api/glifs?id=clkbasluf0000mi08h541a3j4](https://glif.app/api/glifs?id=clkbasluf0000mi08h541a3j4)\
\
This glif has blocks with the names:

```json
username
ego
radmilk
text1
```

<details>

<summary>Full example Glif API response JSON</summary>

```json
[
  {
    "id": "clkbasluf0000mi08h541a3j4",
    "name": "chat.welcome",
    "imageUrl": null,
    "description": "",
    "createdAt": "2023-07-20T15:19:11.511Z",
    "updatedAt": "2023-12-01T11:59:23.279Z",
    "output": "Welcome to our Discord Chat, @jamiedubs, where creativity's at!\nWith Tanaki as your guide, prepare for a delightful ride.\nWe'll connect ideas across the land, with art and magic at our command.\nSo join us now, let's collaborate, and create wonders that resonate.",
    "outputType": "TEXT",
    "forkedFromId": null,
    "featuredAt": null,
    "userId": "cli4waaz20002l5082wrs99bx",
    "completedSpellRunCount": 3,
    "user": {
      "id": "cli4waaz20002l5082wrs99bx",
      "name": "tanaki",
      "image": "https://cdn.discordapp.com/avatars/1111720492408262686/a355dbcf07ce472fc7bc382cf0807802.png",
      "username": "tanaki"
    },
    "spheres": [],
    "data": {
      "nodes": [
        {
          "name": "username",
          "type": "TextInputBlock",
          "params": {
            "label": "@username to greet",
            "value": "harrystyles"
          }
        },
        {
          "name": "ego",
          "type": "GlifBlock",
          "params": {
            "id": "clj359oq70000le08nog0a0sb",
            "inputValues": []
          }
        },
        {
          "name": "radmilk",
          "type": "GlifBlock",
          "params": {
            "id": "clpmk3e3x000xqrpfuxrjq6gl",
            "inputValues": []
          }
        },
        {
          "name": "text1",
          "type": "GPTBlock",
          "params": {
            "api": "chat",
            "model": "gpt-4",
            "prompt": "{ego}\nplease keep the above in mind.\n\nI need you to welcome a new user to a discord chat.\n\nAdd a joke in the style of some example jokes. Share the joke as if it were a fact and you are mitch hedberg. Make it weird and under 2 sentences. Don't mention it is a joke. Just tell the joke. Play with their name. Add linebreaks and markdown to format your message where needed.\n\nExample jokes:\n{radmilk}\n\n\nTheir username is:\n{username}\n\nNow write the welcome message in no more than 2 short sentences:\n\n",
            "maxTokens": 500,
            "temperature": 0.9
          }
        }
      ]
    },
    "_count": {
      "likes": 0,
    }
  }
]
```

</details>

Lastly, you can use the commandline to fetch field names dynamically using `curl` + `jq`:

```bash
curl -s https://glif.app/api/glifs?id=clmdnx8fa0000lf0fbovo8lv1 | jq -r '.[] .data .nodes[] .name'
```

here's the output:

```bash
input1
image1
```

meaning this glif requires two inputs, `input1` and `image1`. We plan to improve this in the near future.

### ⚠️ Strict mode and default glif input value substitution

If you do not provide enough inputs, the default values in the glif values will be automatically substituted.

So if you specify arg1, that'll be used, but if it requires arg1 and arg2, it'll use your arg1 but use the defualt value for arg2.

If you'd prefer to have API calls with insufficient inputs to fail, append `?strict=1` to your URLs, e.g. `https://simple-api.glif.app?strict=1`

### ⚠️ CORS (cross-origin requests)

CORS is allowed by the Simple API, but not by our other API endpoints. You'll need to make your own serverside wrappers for these cases.
