# VM0 Image Build Action

A GitHub Action to build custom [VM0](https://www.vm0.ai) images from a Dockerfile.

## Usage

```yaml
- name: Build VM0 Image
  uses: vm0-ai/image-build-action@v1
  with:
    name: my-custom-image
    vm0-token: ${{ secrets.VM0_TOKEN }}
```

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `working-directory` | No | `.` | Working directory for running the build command |
| `dockerfile` | No | `Dockerfile` | Path to Dockerfile (relative to working-directory) |
| `name` | Yes | - | Name for the image |
| `delete-existing` | No | `false` | Delete existing image before building |
| `vm0-token` | Yes | - | VM0 authentication token |
| `vm0-api-url` | No | `https://www.vm0.ai` | VM0 API URL |
| `cli-version` | No | `latest` | @vm0/cli version to install |

## Outputs

| Output | Description |
|--------|-------------|
| `image-id` | The image ID |
| `name` | The image name |

## Examples

### Basic Usage

```yaml
name: Build Image
on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Build VM0 Image
        uses: vm0-ai/image-build-action@v1
        with:
          name: my-custom-image
          vm0-token: ${{ secrets.VM0_TOKEN }}
```

### Custom Dockerfile Path

```yaml
- name: Build VM0 Image
  uses: vm0-ai/image-build-action@v1
  with:
    dockerfile: docker/Dockerfile.prod
    name: my-custom-image
    vm0-token: ${{ secrets.VM0_TOKEN }}
```

### Custom Working Directory

```yaml
- name: Build VM0 Image
  uses: vm0-ai/image-build-action@v1
  with:
    working-directory: packages/my-service
    name: my-custom-image
    vm0-token: ${{ secrets.VM0_TOKEN }}
```

### Delete Existing Image Before Build

```yaml
- name: Build VM0 Image
  uses: vm0-ai/image-build-action@v1
  with:
    name: my-custom-image
    delete-existing: true
    vm0-token: ${{ secrets.VM0_TOKEN }}
```

### Using Outputs

```yaml
- name: Build VM0 Image
  id: build
  uses: vm0-ai/image-build-action@v1
  with:
    name: my-custom-image
    vm0-token: ${{ secrets.VM0_TOKEN }}

- name: Print Results
  run: |
    echo "Image Name: ${{ steps.build.outputs.name }}"
    echo "Image ID: ${{ steps.build.outputs.image-id }}"
```

## Authentication

To use this action, you need a VM0 authentication token:

1. Install the VM0 CLI: `npm install -g @vm0/cli`
2. Login: `vm0 auth login`
3. Export token: `vm0 auth setup-token`
4. Add to GitHub Secrets as `VM0_TOKEN`

```bash
# Export token and add to GitHub secrets
vm0 auth setup-token | gh secret set VM0_TOKEN
```

## License

MIT
