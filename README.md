# jq Docker Image

A lightweight Docker image for [jq](https://jqlang.github.io/jq/), the command-line JSON processor, built on Alpine Linux.

## Features

- **Lightweight**: Based on Alpine Linux for minimal image size
- **Shell Access**: Includes `sh` shell for interactive use and scripting
- **Latest jq**: Always includes the latest version of jq available in Alpine repositories
- **Ready to Use**: Pre-configured entrypoint for immediate jq usage

## Quick Start

### Using the published image

```bash
# Pull the image from GitHub Container Registry
docker pull ghcr.io/danielramosacosta/jq:main

# Basic usage - process JSON from stdin
echo '{"name": "John", "age": 30}' | docker run -i ghcr.io/danielramosacosta/jq:main '.name'

# Process a JSON file
docker run -v $(pwd):/data ghcr.io/danielramosacosta/jq:main '.[] | .name' /data/input.json
```

### Building locally

```bash
# Clone the repository
git clone https://github.com/DanielRamosAcosta/jq.git
cd jq

# Build the image
docker build -t jq .

# Run it
echo '{"hello": "world"}' | docker run -i jq
```

## Usage Examples

### Basic JSON processing

```bash
# Pretty print JSON
echo '{"name":"John","age":30}' | docker run -i ghcr.io/danielramosacosta/jq:main '.'

# Extract specific field
echo '{"name":"John","age":30}' | docker run -i ghcr.io/danielramosacosta/jq:main '.name'

# Filter arrays
echo '[{"name":"John","age":30},{"name":"Jane","age":25}]' | docker run -i ghcr.io/danielramosacosta/jq:main '.[] | select(.age > 27)'
```

### Working with files

```bash
# Mount current directory and process a file
docker run -v $(pwd):/workspace -w /workspace ghcr.io/danielramosacosta/jq:main '.users[].name' data.json

# Save output to file
docker run -v $(pwd):/workspace -w /workspace ghcr.io/danielramosacosta/jq:main '.users[].name' data.json > names.txt
```

### Interactive shell access

```bash
# Access the container with shell
docker run -it --entrypoint sh ghcr.io/danielramosacosta/jq:main

# Inside the container you can use jq and other Alpine utilities
/# echo '{"test": true}' | jq '.test'
/# ls -la
/# cat /etc/alpine-release
```

### Advanced usage with shell scripting

```bash
# Use shell to combine jq with other commands
docker run -v $(pwd):/data --entrypoint sh ghcr.io/danielramosacosta/jq:main -c '
  for file in /data/*.json; do
    echo "Processing $file"
    jq ".timestamp = $(date +%s)" "$file" > "${file%.json}_processed.json"
  done
'
```

## Available Tags

- `main` - Latest build from the main branch
- `main-<sha>` - Specific commit builds
- `<sha>` - Full SHA builds

## Image Details

- **Base Image**: Alpine Linux (latest)
- **jq Version**: Latest available in Alpine repositories
- **Shell**: `sh` (ash)
- **Entrypoint**: `/usr/bin/jq`
- **Default Command**: `.` (identity filter)

## Building and Development

The image is automatically built and published using GitHub Actions on every push to the main branch.

### Local Development

```bash
# Build the image
docker build -t jq:local .

# Test it works
echo '{"test": "value"}' | docker run -i jq:local '.test'

# Test shell access
docker run -it --entrypoint sh jq:local
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Related Projects

- [jq Official Documentation](https://jqlang.github.io/jq/)
- [jq Manual](https://jqlang.github.io/jq/manual/)
- [Alpine Linux](https://alpinelinux.org/)
