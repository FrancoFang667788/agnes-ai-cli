# Agnes AI CLI

Command-line tools for [Agnes AI](https://agnes-ai.com) multimodal generation — text-to-image, image-to-image, image recognition, and video generation.

## Features

- **Text-to-Image** — Generate images from text descriptions
- **Image-to-Image** — Transform images with style transfer, editing, etc.
- **Image Recognition** — Analyze and describe image content using vision models
- **Video Generation** — Text-to-video, image-to-video, and multi-image keyframe animation

## Setup

1. Get your API key from [Agnes AI](https://agnes-ai.com)
2. Set your API key:

```bash
# Option 1: Environment variable
export AGNES_API_KEY="your-api-key"

# Option 2: Key file
echo "your-api-key" > ~/.agnes-ai-key
```

No additional dependencies required — uses Python 3 standard library only.

## Usage

### Text-to-Image

```bash
python scripts/agnes-image.py --prompt "A futuristic city at sunset" --size 1024x1024
python scripts/agnes-image.py --prompt "A cat in space" --output cat.png
```

### Image-to-Image

```bash
python scripts/agnes-image.py --prompt "turn into watercolor style" --image photo.png
python scripts/agnes-image.py --prompt "add a hat" --image https://example.com/photo.png
```

### Image Recognition

```bash
python scripts/agnes-image.py recognize --image photo.png
python scripts/agnes-image.py recognize --image photo.png --prompt "What breed is this dog?"
```

### Video Generation

```bash
# Text-to-video
python scripts/agnes-video.py --prompt "A cat walking on a beach" --wait

# Image-to-video
python scripts/agnes-video.py --prompt "The woman turns around" --image photo.png --wait

# Multi-image keyframes
python scripts/agnes-video.py --prompt "Smooth transition" --images img1.png img2.png --mode keyframes --wait

# Check status
python scripts/agnes-video.py status <task_id>
```

## Available Models

| Model | Capability |
|-------|-----------|
| `agnes-image-2.0-flash` | Image generation (default) |
| `agnes-image-2.1-flash` | Image generation (latest) |
| `agnes-2.0-flash` | Image recognition / vision |
| `agnes-video-v2.0` | Video generation |

## Claude Code Skill

This project also works as a [Claude Code](https://claude.com/claude-code) skill. See `SKILL.md` for integration details.

## License

MIT
