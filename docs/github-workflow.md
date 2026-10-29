# Goodebot GitHub workflow

## Start with your own repository

Use this repository as a template, or fork it into your GitHub account. Use a readable `goodebot-profile-...` repository name so Goodebot's cloud-profile scan can find it. You need permission to write to the repository you sync.

To start from scratch instead, choose **New Profile → Cloud** in Goodebot, sign in, and enter your robot's name. Complete Create or Import for that profile. A local profile does not acquire a cloud repository just because you sign in.

## Clone into Goodebot

From the profile chooser, choose **Clone Cloud Profiles**, scan the signed-in account, and select the profiles to add. Launch one profile at a time. Check its name, hardware, controls, and autonomous routines before changing anything.

A GitHub template or fork contains this example profile identity until Goodebot loads it. Avoid importing several unchanged copies into one profile list: their shared embedded profile ID can identify them as the same profile. For a clean new identity, create a new Goodebot profile, import this repository's `Goode/workspace`, and let that new profile sync to its own repository. Do not edit UUIDs by hand.

## What gets saved

| File | Role |
| --- | --- |
| `Goode/workspace/src/main.goode.py` | Authoritative editable robot behavior |
| `Goode/workspace/goodecode.toml` | Project entrypoint and export target |
| `Goode/profile.json` | Hardware, controls, autonomous routines, programs, and profile settings |
| `Goode/README.md` | Generated “Big One” summary and recoverable profile JSON |
| `Goode/programs/manifest.json` | Program names, Brain slots, and exported source paths |
| `Goode/programs/*/main.py` | Exported program source; regenerate from the editable project |

The application and Cortex binaries are distributed separately. They are not included here.

## Edit, build, sync

1. Change hardware, controller bindings, or autonomous blocks in Goodebot; save the edit.
2. For source editing, open the project in VS Code with Goodecode. Save the file and refresh Software in Goodebot.
3. Check the generated summary reflects the actual group names, button bindings, ports, and routine selection.
4. Build with Cortex. This validates the project and seals the deployment targets. A successful Cortex Apply queues the profile's GitHub update.
5. Confirm sync succeeds. Open the repository and inspect the commit. Offline or failed sync is not a successful cloud update.
6. Deploy the selected sealed target to the correct Brain slot. GitHub sync by itself does not deploy or run the robot.

Pull or sync before editing on a second computer. Do not edit the same profile on two computers simultaneously. If Git reports conflicting histories, preserve both versions and resolve the conflict; do not force-push to make the warning disappear.

## The Big One and recovery

`Goode/README.md` is generated from the same profile configuration used by the app. Its collapsed **Recoverable Goodebot profile data** section contains a JSON snapshot. This is a recovery aid, not a separate control program.

Keep a complete `Goode/` folder and Git history. Goodebot can read the structured profile data and recover supported README snapshots. A README copied without the editable workspace is not a complete robot project and cannot recreate missing custom source.

The root tutorial README is intentionally separate from the generated summary. This lets a master repository retain its own documentation while Goodebot refreshes the data inside `Goode/`.

## Reset and restore

**Reset Profile** clears that profile's setup while keeping its declared GitHub repository. Each reset is recorded as a commit; an offline reset stays queued until sync succeeds. **Delete Profile** is a different action and can remove the repository—do not use it to clear a robot configuration.

Use **Profile Settings → Version History** to inspect supported published versions. Restore is a pending local state: review it and build again before deployment. GitHub also retains prior committed source for manual recovery.

## Keep a master repository

```text
my-robot/
  README.md
  Goode/
  EZ-Template/
  CAD/
  Notes/
```

Goodebot manages its `Goode/` data. Keep unrelated projects outside that folder. Do not put access tokens, app-specific passwords, signing keys, or compiled Goodebot application files in the repository.

## Troubleshooting

- **Repository missing from Clone:** verify the signed-in account, access permissions, repository naming convention, and presence of supported Goodebot profile data.
- **Old hardware or controls:** confirm the active profile, saved source, latest successful sync, and latest remote commit. Do not copy another robot's profile JSON into this one.
- **Generic action names:** configure named groups and their bindings in the app; generated documentation should use those configured names.
- **No autonomous movement:** this example selects Main. Your own profile must have a nonempty routine selected for the program you deployed. Verify field enable, motor wiring, ports, battery, and the runtime's status.
- **Build not deployed:** GitHub stores the project; Cortex builds it; Deploy transfers it. These are separate steps.

The included code passed Goodebot's static authoring validation and import/export checks. That does not certify physical motion, calibration, field behavior, or competition legality on a particular robot.
