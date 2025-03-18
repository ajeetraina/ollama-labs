# Contributing to Ollama Labs

Thank you for your interest in contributing to Ollama Labs! This document outlines the process for contributing content, code, and improvements to the site.

## Types of Contributions

We welcome the following types of contributions:

1. **Content**: New labs, tutorials, documentation improvements
2. **Code**: Bug fixes, feature enhancements, UI improvements
3. **Examples**: Code examples, use cases, model demonstrations
4. **Reviews**: Technical reviews of existing content

## Getting Started

### Prerequisites

- Basic familiarity with Git and GitHub
- [Hugo](https://gohugo.io/installation/) installed locally
- Experience with Ollama for content contributions

### Local Development

1. **Fork the repository**

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR-USERNAME/ollama-labs.git
   cd ollama-labs
   ```

3. **Install the theme**
   ```bash
   git clone https://github.com/alex-shpak/hugo-book themes/hugo-book
   ```

4. **Start the local development server**
   ```bash
   hugo server -D
   ```

5. **View your local version**
   - Open http://localhost:1313 in your browser

## Content Guidelines

### Adding a New Lab

1. Create a new Markdown file in `content/labs/`
2. Follow the structure of existing labs:
   - Front matter with title and weight
   - Clear objectives
   - Prerequisites section
   - Step-by-step instructions
   - Code examples
   - Conclusion and next steps

### Content Format

- Use Markdown for all content
- Include code blocks with proper syntax highlighting
- Use relative links for internal references
- Include images in the `static/images/` directory

### Style Guide

- Write clear, concise, and accessible content
- Use second person ("you") and active voice
- Break down complex concepts into manageable steps
- Include practical examples and code snippets
- Use consistent terminology

## Submitting Changes

1. **Create a branch for your changes**
   ```bash
   git checkout -b my-contribution
   ```

2. **Make your changes**
   - Follow the content guidelines above
   - Test locally with Hugo

3. **Commit your changes**
   ```bash
   git add .
   git commit -m "Add descriptive commit message"
   ```

4. **Push to your fork**
   ```bash
   git push origin my-contribution
   ```

5. **Create a pull request**
   - Go to the original [ollama-labs repository](https://github.com/ajeetraina/ollama-labs)
   - Click "New pull request"
   - Select "compare across forks"
   - Select your fork and branch
   - Click "Create pull request"
   - Fill in the PR template with a description of your changes

## Review Process

- Maintainers will review your PR
- You may receive feedback or requests for changes
- Once approved, your changes will be merged into the main branch

## Community

- Join the [Collabnix Community](https://collabnix.com) for discussions
- Follow [@collabnix](https://twitter.com/collabnix) on Twitter

## Code of Conduct

Please adhere to our [Code of Conduct](CODE_OF_CONDUCT.md) in all interactions related to the project.

## License

By contributing to Ollama Labs, you agree that your contributions will be licensed under the project's [MIT License](LICENSE).
