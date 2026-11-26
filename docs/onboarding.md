# Onboarding

What you need to start working on the project. Keep this file short and update it whenever something here changes.

## Glossary

| Term | Meaning |
| --- | --- |
| **issue** | a GitHub issue: one piece of work or one decision, assigned to one person |
| **PR** | a GitHub pull request: asks to merge your branch into `main` |
| **branch** | your own line of work in git, created from `main` |
| **status** | where an issue stands: *Backlog* (later) · *Todo* (now, not started) · *In progress* · *In review* (PR open) · closed = done |
| **label** | issue tag: `task` (work to do) · `decision` (choice the team must make) · `bug` |
| **milestone** | a project phase on GitHub, groups the issues of that phase |
| **lead** | everonel, who creates issues and keeps the roadmap |
| **N1–N11** | mandatory features in the specification |
| **E1–E5** | optional extensions in the specification |
| **C1–C12** | final evaluation checklist in the specification |

## 1. What we are building

An emergency messaging network: ESP32 boards with LoRa radios relay each other's messages, no internet needed. Read the [specification](specification.md). The final evaluation checks the **checklist C1–C12** at the end of it, so everything we do should serve that list.

## 2. Setup

Everywhere:

```bash
git clone git@github.com:emptydeepblue/42-open-project.git
cd 42-open-project
git config user.name "<intra login>"         # only for this repo
git config user.email "<intra email>"
```

Your commits are signed with your intra login and email, so it is clear who did what. Add your intra email to your GitHub account (*Settings → Emails*), otherwise GitHub can't link your commits to your profile.

**Campus machines.** PlatformIO is being installed by the 42 IT staff: **don't install it yourself**. Once it is there, run `pio test -e native` (must say PASSED). Flashing boards from campus machines is pending the same request (serial port access).

**Personal machines** (Linux, macOS, WSL). On Windows, keep the repo **inside WSL** (not under `/mnt/c`) and open it with VS Code + the WSL extension.

```bash
sudo apt install pipx && pipx ensurepath     # Ubuntu/WSL; macOS: brew install pipx
pipx install platformio                      # open a new terminal if `pio` is not found
pio test -e native                           # must say PASSED
```

That's all you need: protocol, simulator and tests run on the PC. The hardware stays at school, and flashing instructions will be added here once the ESP32 environment is set up.

## 3. How we work

1. **Take your issue** from the GitHub *Issues* tab and set its *Status* to *In progress*
2. **Create a branch** from `main`: `<type>/<short-name>`, e.g. `feat/mesh-dedup` (type from the box below)
3. **Commit** using the [commit format](#commit-format): small commits are how you split the work into steps
4. **Open a pull request** with `Closes #<issue>` in the description and set the *Status* to *In review*. Another member must approve it before merging.
5. **After the merge, delete the branch**: GitHub deletes it on the remote, you delete your local copy with `git switch main && git pull && git branch -d <branch>`

> [!IMPORTANT]
> Only these values in branch names and commits (need a new one? ask the lead):<br>
> **type**: `feat` · `fix` · `docs` · `test` · `refactor` · `build` · `chore`<br>
> **scope**: `mesh` · `radio` · `sim` · `fw` · `sec` · `ui` · `spec` · `repo` · `ci`

**Issues are created only by the lead**, following the project roadmap. Don't create issues or sub-issues yourself: if you think something is missing (a bug, a task nobody planned), say it on Slack or in a comment on a related issue.

Questions and decisions go in the issue comments or on Slack, not in private chats, so everybody can see them.

### Commit format

```
<type>(<scope>): <summary>

Refs #<issue>
```

- **type** and **scope**: only the values in the box in [How we work](#3-how-we-work)
- **summary**: English, imperative ("add", not "added"), lowercase, no final period
- **`Refs #N`** goes on its own line after a blank line: N is the issue this commit works on (usually your branch's issue)

Example, for work on issue #5:

```bash
git commit -m "feat(mesh): drop packets already seen by message ID" -m "Refs #5"
```

(The second `-m` adds the blank line and the `Refs` line for you.)

You don't close issues from commits: write `Closes #5` in the **pull request description** and the issue closes when the PR is merged.

## 4. Rules

- **Never push directly to `main`**: every change reaches `main` through a pull request
- **Every pull request needs approval from at least one other member** before it is merged. You never merge your own unreviewed PR.
- **Never commit the 42 subject PDFs**: the repo is public. They are ignored by `.gitignore`, so don't force them in.
- Add files by name (`git add path/to/file`), not with `git add .`
- `pio test -e native` must pass before you open a PR

## 5. Repository layout

```
docs/             specification and documentation (this file)
lib/mesh/         mesh protocol: routing, relay, queue, deduplication
lib/radio/        portable Radio interface only (no hardware headers)
lib/radio_esp32/  ESP-NOW and LoRa drivers, ESP32 only (not built by pio test -e native)
sim/              PC simulator
src/              ESP32 firmware entry point
test/             automated tests (run with pio test -e native)
```

## 6. Who does what

| Person | Area |
| --- | --- |
| everonel | Lead, mesh protocol |
| lemarino | Firmware and hardware: radio drivers, duty cycle, board bring-up, assembly guide |
| gpirozzi | Firmware and interface: screen, buttons, GPS, minimal web page |
| lotrapan | Security |
| marboccu | Testing and integration: simulator, tests, CI |

lemarino and gpirozzi work as a pair and review each other's pull requests.

Stuck? Ask the others for help.
