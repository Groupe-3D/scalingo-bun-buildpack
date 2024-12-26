
# Scalingo Bun Buildpack

A custom buildpack designed to enable and streamline the use of **Bun** as the package manager and runtime for applications deployed on **Scalingo**. This buildpack is particularly optimized for **monorepos** and supports Bun-specific workflows, including workspace management and caching.

## Features

- Installs **Bun** automatically.
- Executes `bun install` to manage dependencies.
- Supports monorepos with custom scripts.
- Builds and runs your application using Bun commands.
- Optimized for Scalingo's environment.

## Installation

1. Add the buildpack to your Scalingo application:
   ```bash
   scalingo --app <your-app-name> env-set BUILDPACK_URL=https://github.com/<your-username>/scalingo-bun-buildpack.git
   ```

2. Deploy your application as usual:
   ```bash
   git push scalingo main
   ```

## Buildpack Workflow

1. **Detection**: The buildpack checks if the `package.json` file contains `"packageManager": "bun"`.
2. **Installation**:
   - Installs Bun via the official Bun installer.
   - Adds Bun to the system `PATH`.
   - Installs dependencies with `bun install --cache`.
3. **Build**:
   - Executes `bun run build` if defined in the `package.json` file.
   - Runs workspace-specific build scripts if applicable.
4. **Release**:
   - Configures the startup command for the application (e.g., `bun run start`).

## Requirements

- **Node.js** version `20` (as specified in your `package.json`).
- A `package.json` file with `"packageManager": "bun"`.
- Properly configured workspace structure (if applicable).

## Customization

You can modify the following scripts in your `package.json` to align with the buildpack's default behavior:

- **`build`**: Define custom build steps for your application.
- **`start`**: Specify the entry point for your app (default: `bun run start`).

## Example `package.json`

```json
{
  "packageManager": "bun@1.1.38",
  "workspaces": {
    "packages": [
      "apps/*",
      "packages/*"
    ]
  },
  "scripts": {
    "build": "bun run --cwd packages/common build && bun run --cwd apps/api build",
    "start": "bun run --cwd apps/api start"
  },
  "engines": {
    "node": "20"
  }
}
```

## Contributing

Feel free to fork this repository and submit pull requests for improvements or additional features.

## License

This project is licensed under the [MIT License](LICENSE).
