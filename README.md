# Runway (runwayml)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
