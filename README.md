# The Big One — Goodebot on GitHub

**Create. Import. Deploy. Keep your robot project together.**

This is a public **example robot profile** for Goodebot 5.1 and Goodecode 1.0.2. It demonstrates how a robot configuration, editable code, autonomous routine, and program slots can live in one GitHub repository.

Goodebot is proprietary software. This repository contains example robot code and documentation; it does not contain the Goodebot app or Cortex implementation.

[Get Goodebot](https://goodebot.net/download) · [Read the GitHub guide](docs/github-workflow.md) · [Open the example code](Goode/workspace/src/main.goode.py) · [View the generated profile summary](Goode/README.md)

## What is “The Big One”?

“The Big One” is the complete, readable overview of a Goodebot robot project. In the current layout, that information is presented through README files rather than a separate legacy script:

- **This README** explains how the repository works and how to use it.
- **[Goode/README.md](Goode/README.md)** is the app-generated robot summary: hardware, configured controls, programs, autonomous steps, and a collapsed recovery-data section.
- **[Goode/profile.json](Goode/profile.json)** stores the structured profile configuration.
- **[Goode/workspace/](Goode/workspace/)** contains the editable Goodecode project. This is where the robot behavior comes from.

The recovery block helps Goodebot reconstruct a profile. Do not hand-edit it to change a motor or routine: edit the project in Goodebot or Goodecode, then sync it. There is no “Big Brother” script.

## Try it

1. Install Goodebot from [goodebot.net](https://goodebot.net/download) and Goodecode 1.0.2 in VS Code.
2. Create your own repository from this template, or fork it. Give your copy a readable name such as `goodebot-profile-my-robot`.
3. In Goodebot, sign in to the GitHub account that owns your copy. Use **Clone Cloud Profiles**, scan your account, and select your repository. This example uses the `goodebot-profile-` naming convention so it can be discovered.
4. Open the profile. Verify every motor port, reversal, IMU port, pneumatic output, and controller binding against your robot.
5. Inspect the **Main** autonomous routine. Use the Digital Twin to preview it before running hardware.
6. Build with Cortex, select the program slots, and deploy to your robot Brain. Practice and Competition have separate slots.
7. Open your repository from **Software → Open repository on GitHub** to review the synced configuration and commit history.

If you only want the example source, download this repository as a ZIP, extract it, and import `Goode/workspace` into Goodebot. That imports source; it does not automatically connect the downloaded folder to your GitHub account.

## Example robot

These ports are a demonstration, not detected hardware. Change them before deployment.

| Device | Ports | Configuration |
| --- | --- | --- |
| Left Drive | 1, 2 | Green 200 RPM motors |
| Right Drive | 3, 4 | Green 200 RPM motors; reversed in code |
| Intake | 5 | Green motor, coast braking |
| Drive IMU | 8 | Inertial sensor |
| Clamp | Three-wire A | Pneumatic digital output |

Drive geometry: 3.25-inch wheels, 1:1 external ratio, 12.5-inch track width. Motor count does not multiply travel distance. Adjust these measurements to your real robot; the sample is not a calibrated motion model.

| Input | Action |
| --- | --- |
| Left/right Y sticks | Tank drive |
| L1 held | Intake spins |
| A pressed | Clamp toggles |

The **Main** routine drives forward 24 inches at 50%, runs Intake for 0.3 seconds at 30%, then turns Clamp on. The example includes an IMU declaration on port 8. Verify the physical sensor and calibrate your robot before relying on turns or distances.

## Practice and Competition

| Program | Brain slot | Purpose |
| --- | --- | --- |
| Example Robot Practice | 1 | Interactive testing and autonomous selection |
| Example Robot Competition | 2 | Autonomous and driver control enabled by field control or a timed competition run |

The Field Controller controls the robot's Competition program; the Field Controller Brain itself is not where you install this robot project.

## Repository layout

```text
README.md                         The Big One: overview and getting started
docs/github-workflow.md           Sync, clone, recovery, and collaboration guide
Goode/
  README.md                       Generated hardware/control/auton summary
  profile.json                    Portable profile data
  profiles/<profile-id>/          Profile identity metadata
  workspace/
    goodecode.toml                 Project entrypoint
    src/main.goode.py              Editable robot program
  programs/
    manifest.json                 Practice/Competition slots and entrypoints
    practice/main.py              Exported Practice source
    competition/main.py           Exported Competition source
```

All Goodebot data stays under `Goode/`. You can keep an `EZ-Template/`, CAD, notes, or other projects alongside it. The UUID inside profile metadata identifies the profile; it is not part of the visible repository name.

## Keeping GitHub in sync

Save changes in Goodebot or your Goodecode source. Cloud profiles sync saved configuration; successful Cortex Apply also records the sealed build state. Check the app's sync status and GitHub's latest commit before assuming another computer has your changes. Offline changes need a later successful sync.

The generated `Goode/programs/` files are exports. Edit `Goode/workspace/src/main.goode.py` or the visual configuration, not multiple exported copies independently. Cortex rebuilds local deployment artifacts; compiled app files, build caches, credentials, and private robot profiles do not belong in this example repository.

For reset history, recovery, using your own copy, and troubleshooting, see the [GitHub workflow guide](docs/github-workflow.md).
