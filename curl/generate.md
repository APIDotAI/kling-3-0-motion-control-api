# Kling 3.0 Motion Control cURL Quickstart

## What this example shows

This example shows how to call Kling 3.0 Motion Control through APIDot with a server-side API key.

## Requirements

- An APIDot account.
- An APIDot API key stored server-side.
- `curl` installed locally.
- A backend or terminal environment that does not expose API keys to browser code.

## Environment variables

Use placeholders only. Do not commit real credentials.

```env
APIDOT_API_KEY=YOUR_API_KEY_HERE
```

## How to run

Add `callback_url` only when you have a real webhook receiver.

```bash
export APIDOT_API_KEY="YOUR_API_KEY_HERE"

curl --fail-with-body --request POST \
  --url https://api.apidot.ai/api/generate/submit \
  --header "Authorization: Bearer $APIDOT_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{
  "model": "kling-3.0-motion-control",
  "input": {
    "prompt": "optional scene prompt",
    "image_urls": [
      "https://your-domain.com/character.png"
    ],
    "video_urls": [
      "https://your-domain.com/reference-motion.mp4"
    ],
    "character_orientation": "image",
    "resolution": "720p"
  }
}'
```

Store the returned `data.task_id`, then poll status:

```bash
curl --fail-with-body --request GET \
  --url https://api.apidot.ai/api/generate/status/task-unified-example \
  --header "Authorization: Bearer $APIDOT_API_KEY"
```

## Request variants

### Match image orientation

```json
{
  "model": "kling-3.0-motion-control",
  "callback_url": "https://your-domain.com/callback",
  "input": {
    "prompt": "Transfer the dance motion to the uploaded character, keep the character identity stable, clean studio lighting",
    "image_urls": [
      "https://your-domain.com/character.png"
    ],
    "video_urls": [
      "https://your-domain.com/reference-motion.mp4"
    ],
    "character_orientation": "image",
    "resolution": "720p"
  }
}
```

### Match video orientation

```json
{
  "model": "kling-3.0-motion-control",
  "callback_url": "https://your-domain.com/callback",
  "input": {
    "prompt": "Follow the reference video's camera framing and action rhythm for an energetic product mascot clip",
    "image_urls": [
      "https://your-domain.com/mascot.png"
    ],
    "video_urls": [
      "https://your-domain.com/action-reference.mov"
    ],
    "character_orientation": "video",
    "resolution": "1080p"
  }
}
```

## Expected response

```json
{
  "code": 200,
  "data": {
    "task_id": "task-unified-example",
    "status": "finished",
    "output": {
      "files": [
        {
          "file_url": "https://example.com/generated-video.mp4",
          "file_type": "video"
        }
      ]
    },
    "error_message": null
  }
}
```

## Production notes

- Keep APIDot API keys on the server side only.
- Store selected model, request payload, user ID, and response metadata together for support and retries.
- Store `data.task_id` before polling or waiting for a webhook.
- Poll at a moderate interval and avoid hot loops.
- Treat `finished` and `failed` as terminal states.

## Common mistakes

- Committing a real API key or `.env` file.
- Calling APIDot directly from browser code.
- Reusing request fields from a different model without checking this model's docs.

## Related links

- Website: https://apidot.ai
- Docs: https://apidot.ai/docs
- Kling 3.0 Motion Control docs: https://apidot.ai/docs/kling-3-0-motion-control
- Kling 3.0 Motion Control model page: https://apidot.ai/models/kling-3-0-motion-control
- GitHub: https://github.com/APIDotAI
- Examples: https://github.com/APIDotAI/apidot-examples
