# Golang Project Templates

Standard templates for Golang projects, designed to be used with [Grept](https://github.com/Azure/grept) policies.

## What's Included

This repository contains standardized configuration files and workflows for Golang projects:

- **`.golangci.yml`** - Standard linter configuration with goimports and gofmt formatters
- **`.github/workflows/test.yml`** - CI workflow for testing and linting
- **`.github/workflows/release.yml`** - Automated release builds (optimized for gh extensions)
- **`.github/workflows/license.yml`** - License compliance checking using go-licenses
- **`.github/dependabot.yml`** - Automated dependency updates for Go modules and GitHub Actions

## Usage with Grept

These templates are referenced by Grept policies to enforce project standards.

### Example Policy

```hcl
data http golangci_config {
  url = "https://raw.githubusercontent.com/jackchuka/golang-templates/main/.golangci.yml"
}

rule file_hash golangci_yml {
  glob = ".golangci.yml"
  hash = sha1(data.http.golangci_config.response_body)
}

fix local_file golangci_yml {
  rule_ids = [rule.file_hash.golangci_yml.id]
  paths    = [rule.file_hash.golangci_yml.glob]
  content  = data.http.golangci_config.response_body
}
```

### Check and Fix

```bash
# Check if files match templates
grept check

# Auto-fix files to match templates
grept fix
```

## Customization Points

Some templates include TODO comments for project-specific values:

- **dependabot.yml**: Update `YOUR_GITHUB_USERNAME` with your actual GitHub username
- **Other workflows**: May need adjustment for project-specific requirements

## Template Philosophy

These templates follow a "baseline + customize" approach:
- Enforce common standards and best practices
- Allow customization where project-specific needs arise
- Maintain consistency across multiple Golang projects

## Version Pinning

For production use, consider pinning to specific versions:

```hcl
url = "https://raw.githubusercontent.com/jackchuka/golang-templates/v1.0.0/.golangci.yml"
```

This ensures template changes don't unexpectedly affect your projects.

## Contributing

Feel free to suggest improvements via issues or pull requests. These templates are designed to be minimal and widely applicable.

## License

MIT
