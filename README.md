<p align="center"><code>npm i -g @openai/codex</code><br />or <code>brew install --cask codex</code></p>
<p align="center"><strong>Codex CLI</strong> is a coding agent from OpenAI that runs locally on your computer.
<p align="center">
  <img src="https://github.com/openai/codex/blob/main/.github/codex-cli-splash.png" alt="Codex CLI splash" width="80%" />
</p>
</br>
If you want Codex in your code editor (VS Code, Cursor, Windsurf), <a href="https://developers.openai.com/codex/ide">install in your IDE.</a>
</br>If you want the desktop app experience, run <code>codex app</code> or visit <a href="https://chatgpt.com/codex?app-landing-page=true">the Codex App page</a>.
</br>If you are looking for the <em>cloud-based agent</em> from OpenAI, <strong>Codex Web</strong>, go to <a href="https://chatgpt.com/codex">chatgpt.com/codex</a>.</p>

---

## Quickstart

### Installing and running Codex CLI

Install globally with your preferred package manager:

```shell
# Install using npm
npm install -g @openai/codex
```

```shell
# Install using Homebrew
brew install --cask codex
```

Then simply run `codex` to get started.

<details>
<summary>You can also go to the <a href="https://github.com/openai/codex/releases/latest">latest GitHub Release</a> and download the appropriate binary for your platform.</summary>

Each GitHub Release contains many executables, but in practice, you likely want one of these:

- macOS
  - Apple Silicon/arm64: `codex-aarch64-apple-darwin.tar.gz`
  - x86_64 (older Mac hardware): `codex-x86_64-apple-darwin.tar.gz`
- Linux
  - x86_64: `codex-x86_64-unknown-linux-musl.tar.gz`
  - arm64: `codex-aarch64-unknown-linux-musl.tar.gz`

Each archive contains a single entry with the platform baked into the name (e.g., `codex-x86_64-unknown-linux-musl`), so you likely want to rename it to `codex` after extracting it.

</details>

### Using Codex with your ChatGPT plan

Run `codex` and select **Sign in with ChatGPT**. We recommend signing into your ChatGPT account to use Codex as part of your Plus, Pro, Team, Edu, or Enterprise plan. [Learn more about what's included in your ChatGPT plan](https://help.openai.com/en/articles/11369540-codex-in-chatgpt).

You can also use Codex with an API key, but this requires [additional setup](https://developers.openai.com/codex/auth#sign-in-with-an-api-key).

## Docs

- [**Codex Documentation**](https://developers.openai.com/codex)
- [**Contributing**](./docs/contributing.md)
- [**Installing & building**](./docs/install.md)
- [**Open source fund**](./docs/open-source-fund.md)

## Maintaining A Public Fork

If you keep a public fork of Codex, use the upstream/origin split so it stays easy to sync with OpenAI's repository:

```bash
# One-time setup
git remote rename origin upstream
git remote add origin https://github.com/YOUR_GITHUB_USERNAME/codex.git

# Sync with upstream later
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

This keeps `upstream` pointed at `openai/codex` and `origin` pointed at your own repository.

## Keeping Private Gateway Config Out Of Git

If you use a private OpenAI-compatible gateway, keep the base URL and API key in local configuration instead of committing them to source control.

Example local setup in `~/.codex/config.toml`:

```toml
model = "gpt-5.4"
model_provider = "openai"
openai_base_url = "http://gateway.example.com/v1"
```

Then provide the API key locally, for example:

```bash
export OPENAI_API_KEY="your-local-secret"
```

This repository intentionally avoids embedding gateway URLs or API keys in tracked files so the fork can remain public safely.

## Running A Local Build

If you build your own Codex binary locally, you do not need to reinstall from npm or Homebrew. You only need to make sure the shell resolves `codex` to the binary you want to run.

Check which binary is currently active:

```bash
which codex
readlink -f "$(which codex)"
```

If you want your locally built binary to be the default `codex`, copy it into a directory that is already on your `PATH`, such as `/usr/local/bin`:

```bash
sudo install -m 755 /path/to/codex /usr/local/bin/codex
```

If you prefer not to replace the global binary, you can also run it directly:

```bash
/path/to/codex --version
/path/to/codex
```

Example local config with two permission profiles:

```toml
profile = "auto"

[profiles.auto]
approval_policy = "never"
sandbox_mode = "danger-full-access"

[profiles.safe]
approval_policy = "on-request"
sandbox_mode = "workspace-write"
```

With that setup:

- `codex` uses the default `auto` profile
- `codex -p safe` switches back to confirmation prompts and sandboxed writes
- `codex -a never -s danger-full-access` overrides permissions for a single run

`danger-full-access` disables Codex's sandbox, but it does not bypass the operating system's privilege model. For true root operations, run Codex under `sudo`, or ensure your environment already allows passwordless or cached `sudo`.

This repository is licensed under the [Apache-2.0 License](LICENSE).
