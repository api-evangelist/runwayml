# Runway ML GraphQL Schema

Runway is a generative AI research company building creative tools for video, image, and audio production. Its Gen-4 family of models (and the legacy Gen-3 Alpha family) powers text-to-video, image-to-video, video-to-video, and text-to-image generation, alongside text-to-speech and avatar capabilities. The Runway API follows an async task pattern: clients submit a generation job and poll for completion using a task ID.

This conceptual GraphQL schema maps the Runway REST API surface (base URL: `https://api.dev.runwayml.com/v1`) into a typed GraphQL model. It is derived from the public documentation at https://docs.dev.runwayml.com/ and covers the full lifecycle of media generation, asset management, team administration, billing, and platform operations.

## Schema Source

- REST API docs: https://docs.dev.runwayml.com/
- GitHub organization: https://github.com/runwayml
- API base URL: `https://api.dev.runwayml.com/v1`
- Authentication: Bearer token (`Authorization: Bearer <RUNWAYML_API_SECRET>`)

## Type Summary

| Category | Types |
|---|---|
| Models | `Model`, `ModelDetails`, `ModelCapabilities`, `Gen3Alpha`, `Gen3AlphaTurbo` |
| Text-to-Video | `TextToVideo`, `T2VRequest`, `VideoPrompt`, `VideoSeed`, `Duration`, `Resolution`, `AspectRatio`, `FrameRate` |
| Image-to-Video | `ImageToVideo`, `I2VRequest`, `InputImage`, `MotionScore` |
| Video-to-Video | `VideoToVideo`, `V2VRequest`, `VideoInput`, `VideoStyle`, `VideoStrength` |
| Upscale / Inpaint | `UpscaleRequest`, `InpaintRequest`, `MaskInput`, `BrushTool`, `BrushStroke` |
| Video Output | `VideoOutput`, `VideoURL`, `VideoPreview` |
| Assets | `RunwayAsset`, `Asset`, `AssetType`, `ImageAsset`, `VideoAsset`, `AudioAsset` |
| Projects & Teams | `Project`, `ProjectDetails`, `Team`, `TeamMember` |
| Billing | `CreditBalance`, `UsageRecord`, `Subscription`, `RunwayPlan` |
| Generation Tracking | `Generation`, `GenerationStatus`, `GenerationResult`, `PollingStatus`, `TaskID` |
| Auth | `APIKey`, `Token` |
| Platform | `Webhook`, `RateLimit` |
| Pagination | `AssetConnection`, `AssetEdge`, `GenerationConnection`, `GenerationEdge`, `PageInfo` |

## Key Design Decisions

**Async task pattern.** Every generation mutation (`createTextToVideo`, `createImageToVideo`, `createVideoToVideo`, `upscaleVideo`, `inpaintAsset`) returns a type that includes a `taskId: TaskID!` and `status: GenerationStatus!`. Callers poll via `Query.generation(taskId)` or subscribe via `Subscription.generationStatusUpdated(taskId)`.

**Asset union.** The `AssetDetails` union (`ImageAsset | VideoAsset | AudioAsset`) matches Runway's multi-modal output surface, enabling a single `asset(id)` query to return strongly-typed output regardless of media type.

**Separate Gen-3 Alpha types.** `Gen3Alpha` and `Gen3AlphaTurbo` are surfaced as distinct query targets (`gen3AlphaModels`) to preserve backward compatibility with the legacy model family while the Gen-4 family is addressed through the generic `Model` type.

**Credit and subscription modelling.** `CreditBalance`, `UsageRecord`, `Subscription`, and `RunwayPlan` reflect Runway's credit-based billing system, where each generation consumes credits tied to model, duration, and resolution.

**Webhook and rate-limit introspection.** `Webhook` and `RateLimit` types give API consumers the ability to inspect platform limits and manage event delivery without leaving the GraphQL layer.

## Example Queries

### Submit a text-to-video job

```graphql
mutation CreateVideo {
  createTextToVideo(input: {
    model: "gen4_turbo"
    promptText: "A golden retriever running through a field of sunflowers, cinematic lighting"
    duration: FIVE_SECONDS
    resolution: HD_720P
    aspectRatio: RATIO_16_9
    frameRate: FPS_24
  }) {
    taskId { value }
    status
    createdAt
  }
}
```

### Poll generation status

```graphql
query PollGeneration($taskId: ID!) {
  generation(taskId: $taskId) {
    status
    pollingStatus
    result {
      outputs {
        url { href expiresAt }
        durationSeconds
        width
        height
      }
    }
    creditsUsed
    completedAt
    failureReason
  }
}
```

### Submit an image-to-video job

```graphql
mutation AnimateImage {
  createImageToVideo(input: {
    model: "gen4_turbo"
    promptImage: "https://example.com/my-image.jpg"
    promptText: "Camera slowly pulls back to reveal a vast snowy mountain range"
    duration: TEN_SECONDS
    ratio: RATIO_16_9
    motionScore: 7
  }) {
    taskId { value }
    status
    inputImage { url width height }
  }
}
```

### Check credit balance

```graphql
query Credits {
  creditBalance {
    available
    used
    total
    resetAt
  }
  subscription {
    plan
    status
    currentPeriodEnd
  }
}
```

### List assets

```graphql
query ListAssets {
  assets(type: VIDEO, first: 20) {
    edges {
      node {
        id
        name
        type
        url
        createdAt
      }
    }
    pageInfo { hasNextPage endCursor }
    totalCount
  }
}
```

### Subscribe to live generation updates

```graphql
subscription WatchGeneration {
  generationStatusUpdated(taskId: "task_abc123") {
    status
    pollingStatus
    result {
      outputs { url { href } }
    }
  }
}
```

## File

- Schema: [runway-ml-schema.graphql](runway-ml-schema.graphql)
