# 📫 Embed player & custom webpages

Using iframe message passing. Your page can recieve "glif ran" events from our embeddable player.

Here is the demo: [https://glif.app/embed-test.html](https://glif.app/embed-test.html)

way you can receive results from the embed player. For security reasons, you can only listen to the demo and can't send to it. If you want to run glifs, register a key and [run glifs via API](running-glifs.md). Additionally, check out [Val.town](https://val.town) + [Web Fetcher Block](../blocks/advanced-experimental/web-fetcher-block.md) for writing more advanced examples.

## Simple example

You can copy a glif embed code from the glif page in the "share" menu top right. [Here is a detailed tutorial on how to do that, with screenshots](getting-started/what-can-i-do-with-a-glif/embed-a-glif/)

Here's a working webpage that runs one of my personal favorite glifs, [Farthest Sider Cartoon Generator](https://glif.app/@Carlos/glifs/clllbhk40000rmj0f7rk3qzfp) by [@Carlos](https://glif.app/@Carlos).

```html
<div id="glif_1718829227200">
  <iframe
    id="glifIframe"
    src="https://glif.app/@Carlos/glifs/clllbhk40000rmj0f7rk3qzfp/embed?mode=last_10_runs&ar=landscape"
    style="width: 100%; height: 100%; border: 0"
    frameborder="0"
    allowfullscreen
  ></iframe>
</div>
<style>
  #glif_1718829227200 {
    position: relative;
    width: 100%;
    aspect-ratio: 16 / 9;
    overflow: hidden;
  }
</style>
```

Here is the core iframe communication part. Your page recieves events from the glif iframe using `postMessage` API

```html
<!doctype html>

<script>
window.addEventListener("message", handleMessage, false);

function handleMessage(event) {
  const allowedDomains = ["https://glif.app"];
  if (!allowedDomains.includes(event.origin)) {
    console.warn("ignoring message from untrusted origin");
    return;
  }

  const message = event.data;
  if (message.type === "glifRunStarted") {
    // handle a glif run starting (e.g. show a loading indicator)
  } else if (message.type === "glifRunCompleted") {
    // handle a glif run completing (e.g. show the final image)
  } else {
    // currently only sends those 2 messages so nothing else here yet!
  }
}
</script>
```

A more complex example is [Glif Iframe Embed Demo app](https://glif.app/embed-test.html)

Here is a simpler webpage:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Glif Embed Example</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        h1 {
            text-align: center;
        }
        #glifUrl {
            width: 70%;
            padding: 10px;
            margin-right: 10px;
        }
        button {
            padding: 10px 20px;
            background-color: #007bff;
            color: white;
            border: none;
            cursor: pointer;
        }
        #output {
            margin-top: 20px;
        }
        #outputImage {
            max-width: 100%;
            margin-top: 10px;
        }
        #outputUrl {
            width: 100%;
            height: 50px;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h1>Glif Embed Demo</h1>

    <div>
        <input type="text" id="glifUrl" placeholder="Enter Glif URL">
        <button onclick="loadGlif()">Run Glif</button>
    </div>

    <div id="output">
        <img id="outputImage" style="display: none;">
        <textarea id="outputUrl" readonly></textarea>
    </div>

    <script>
        function loadGlif() {
            const glifUrl = document.getElementById('glifUrl').value;
            const iframe = document.createElement('iframe');
            iframe.style.display = 'none';
            iframe.src = glifUrl + '/embed?mode=last_10_runs&ar=landscape';
            document.body.appendChild(iframe);

            window.addEventListener('message', handleMessage);
        }

        function handleMessage(event) {
            if (event.origin !== 'https://glif.app') return;

            const message = event.data;
            if (message.type === 'glifRunCompleted' && message.outputFull?.type === 'IMAGE') {
                const imageUrl = message.outputFull.value;
                document.getElementById('outputImage').src = imageUrl;
                document.getElementById('outputImage').style.display = 'block';
                document.getElementById('outputUrl').value = imageUrl;
            }
        }
    </script>
</body>
</html>
```

If you need help us please contact us in [#api-support](https://glif.app/discord) in our Discord server.
