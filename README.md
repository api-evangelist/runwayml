# Runway (runwayml)

Runway (RunwayML) is a generative AI media platform whose developer API turns text, images, and video into new video, images, speech, and character performances. The REST API at `https://api.dev.runwayml.com/v1` exposes Runway's Gen-4 and Gen-4 Turbo video models, Gen-4 Image text-to-image, the Aleph video-to-video editing model, and Act character performance.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/runwayml/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/runwayml/refs/heads/main/apis.yml)

## Access Model

The Runway API is a paid, self-serve developer API - it is a separate product from the Runway web app and consumer subscription plans, and API credits are billed separately.

- **Base URL:** `https://api.dev.runwayml.com/v1`
- **Authentication:** `Authorization: Bearer <RUNWAYML_API_SECRET>`. API secrets are created inside a developer **organization** in the dev portal at [https://dev.runwayml.com/](https://dev.runwayml.com/).
- **Versioning:** every request must send a dated `X-Runway-Version` header (for example `2024-11-06`). New behavior ships behind new version dates.
- **Billing:** usage is metered in **credits** purchased in the developer portal at roughly **$0.01 per credit**. There is no free/anonymous tier for the API - you buy credits against your organization.
- **Async pattern:** generation is **asynchronous**. You `POST` to a generation endpoint, receive a task `id`, then **poll** `GET /v1/tasks/{id}` until the task reaches `SUCCEEDED` (or `FAILED`). The official Node and Python SDKs wrap this with `waitForTaskOutput()` / `wait_for_task_output()`. Poll no more than once every five seconds per task. There is **no public WebSocket** surface - see `review.yml`.

## Tags

- Video Generation
- AI Video
- Generative AI
- Text-to-Video
- Image-to-Video
- Text-to-Image
- Video-to-Video

## Timestamps

- **Created:** 2026-07-11
- **Modified:** 2026-07-11

## APIs

### Runway Image-to-Video API

Generate video from a starting (and optional ending) image and a text prompt using Runway's Gen-4 Turbo and Gen-4.5 models (and the deprecated Gen-3 Alpha Turbo). `POST /v1/image_to_video` creates an asynchronous task; the returned task id is polled on the Tasks API until the rendered video URL is ready.

- **Human URL:** [https://docs.dev.runwayml.com/api-details/sdks/](https://docs.dev.runwayml.com/api-details/sdks/)
- **Base URL:** `https://api.dev.runwayml.com/v1`

### Runway Text-to-Image API

Generate still images from a text prompt (and optional reference images) using the Gen-4 Image and Gen-4 Image Turbo models via `POST /v1/text_to_image`. Results are commonly used as the seed frames for image-to-video generation.

- **Human URL:** [https://docs.dev.runwayml.com/guides/using-the-api/](https://docs.dev.runwayml.com/guides/using-the-api/)
- **Base URL:** `https://api.dev.runwayml.com/v1`

### Runway Video-to-Video API

Transform an existing video with a text prompt and optional reference keyframes using Runway's Aleph video-to-video editing model via `POST /v1/video_to_video` - restyle, alter, or re-render footage.

- **Human URL:** [https://docs.dev.runwayml.com/guides/using-the-api/](https://docs.dev.runwayml.com/guides/using-the-api/)
- **Base URL:** `https://api.dev.runwayml.com/v1`

### Runway Character Performance API

Drive a character (image or video) with a reference performance video using the Act-Two model via `POST /v1/character_performance`, transferring facial expression and body motion onto the target character.

- **Human URL:** [https://docs.dev.runwayml.com/characters/integration/](https://docs.dev.runwayml.com/characters/integration/)
- **Base URL:** `https://api.dev.runwayml.com/v1`

### Runway Text-to-Speech API

Synthesize speech audio from text via `POST /v1/text_to_speech`, used to add voice-over to generated video. Billed per character.

- **Human URL:** [https://docs.dev.runwayml.com/guides/using-the-api/](https://docs.dev.runwayml.com/guides/using-the-api/)
- **Base URL:** `https://api.dev.runwayml.com/v1`

### Runway Video Upscale API

Upscale a generated or uploaded video to a higher resolution via `POST /v1/video_upscale`, billed per output frame.

- **Human URL:** [https://docs.dev.runwayml.com/guides/using-the-api/](https://docs.dev.runwayml.com/guides/using-the-api/)
- **Base URL:** `https://api.dev.runwayml.com/v1`

### Runway Tasks API

Retrieve the status and output of a generation task (`PENDING`, `RUNNING`, `SUCCEEDED`, `FAILED`) with `GET /v1/tasks/{id}`, or cancel/delete a running task with `DELETE /v1/tasks/{id}`. Every generation endpoint returns a task id that is polled here.

- **Human URL:** [https://docs.dev.runwayml.com/guides/using-the-api/](https://docs.dev.runwayml.com/guides/using-the-api/)
- **Base URL:** `https://api.dev.runwayml.com/v1`

### Runway Organization API

Return the usage tier and remaining credit balance for the developer organization tied to your API key with `GET /v1/organization`, so integrations can check credits before submitting billable generation tasks.

- **Human URL:** [https://docs.dev.runwayml.com/usage/tiers/](https://docs.dev.runwayml.com/usage/tiers/)
- **Base URL:** `https://api.dev.runwayml.com/v1`

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/runwayml)
- [Website](https://runwayml.com/)
- [Documentation](https://docs.dev.runwayml.com/)
- [Plans](plans/runwayml-plans-pricing.yml)
- [Rate Limits](rate-limits/runwayml-rate-limits.yml)
- [Fin Ops](finops/runwayml-finops.yml)
- [Sign Up](https://dev.runwayml.com/)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
