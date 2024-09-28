---
time created: 2024-09-28 16:15
tags:
  - linux
  - learning
references:
  - "[[Linux Helpbox]]"
source: https://www.youtube.com/watch?v=6WLaNIlDW0M&list=PL_WcXIXdDWWpuypAEKzZF2b5PijTluxRG
---
---
# An Overview

This guide provides a detailed walkthrough of various rollback methods in NixOS, emphasizing the integration of Git for robust configuration management. It covers:

- **Traditional NixOS Rollback:** Using GRUB/systemd boot menus and the `switch-to-configuration` tool.
- **Git-Based Rollback for Configuration Files:** Tracking changes with Git, using `git revert` for undoing commits, and `git reset` for more forceful reversions.
- **Rollback of Nix Channels:** Understanding channel generations, utilizing `nix-channel --rollback`, and differentiating between user-level and root-level channels.
- **Garbage Collection:** Employing `nix-collect-garbage` for storage management, understanding the implications for rollback (`--delete-older-than`), and the inherent risks of deleting past generations.
- **The Power of NixOS Flakes:** Exploring how flakes, with their `flake.nix` and `flake.lock` files, revolutionize rollback and version control.
- **Infinite Rewind with Flakes:** Understanding how the `flake.lock` file, by recording precise package versions, enables rollback to any point in your Git history, even after garbage collection.
- **Unified Rollback with Flakes:** Using `git revert` to handle both configuration changes and package version rollbacks when using flakes.
- **Managing Your NixOS Configuration with Git:** A detailed walkthrough of setting up and utilizing Git for your NixOS configuration files.

---
# Traditional NixOS Rollback Mechanisms

## Leveraging Boot Menus

When a NixOS system fails to boot or encounters issues after a configuration change or package update, the most immediate recourse is the boot menu. Both GRUB and systemd boot menus typically retain entries for several previous system generations. Selecting a prior working generation from the boot menu allows you to quickly restore a functional system state.

## The `switch-to-configuration` Tool

While booting into a previous generation provides immediate recovery, it doesn't alter the default boot entry. Subsequent reboots would default to the potentially broken configuration. The `switch-to-configuration` tool addresses this.

- **Location:** `/run/current-system/sl-bin/switch-to-configuration`
- **Usage:** `sudo switch-to-configuration [option]`
- Options resemble those of `sudo nixos-rebuild`: `switch`, `boot`, `test`, etc.

Running `sudo switch-to-configuration boot` after booting into a working older generation sets that generation as the default, ensuring future reboots default to the known good state.

---
# Git-Based Configuration Management

## Tracking Changes with Git

Version controlling your NixOS configuration files with Git is essential for reliable rollback and configuration management.

1. **Initialization:** Navigate to your configuration directory (e.g., `~/dotfiles`) and run `git init`.
2. **Configuration:** Configure Git with your name and email:
    ```
    git config --global user.name "Your Name"
    git config --global user.email "you@example.com"
    ```
3. **First Commit:** Stage all changes (`git add .`) and make your initial commit:
    ```
    git commit -m "Initial commit"
    ```

## Reverting Configuration Changes with `git revert`

When a configuration change introduces issues, `git revert` provides a safe method to undo specific commits.

1. **Identify the Problematic Commit:** Use `git log` to review commit history and locate the commit hash responsible for the problem.
2. **Revert the Commit:**
    ```
    git revert <commit-hash>
    ```
	This creates a new commit that undoes the changes introduced by the specified commit.


## Using `git reset` for More Forceful Reversions

In situations where you need to discard all changes since a particular commit, `git reset` offers a more potent solution.

**Caution:** `git reset --hard` permanently removes changes. Use with extreme care!

```
git reset --hard <commit-hash>
```

This resets your branch to the specified commit, discarding all subsequent commits.

---
# Rollback of Nix Channels

## Understanding Channel Generations

NixOS channels represent snapshots of the Nixpkgs repository, dictating the available package versions. Updating channels (`nix-channel --update`) fetches newer generations, potentially introducing breaking changes.

## Rolling Back Channels

The `nix-channel --rollback` command enables reverting to older channel generations.

1. **List Generations:**
    ```
    nix-channel --list-generations
    ```
    This displays available generations, with the most recent at the top.
2. **Rollback:**
    ```
    nix-channel --rollback <generation-number>
    ```

## User vs. Root Channels

NixOS maintains separate channel sets for regular users and the root user. Ensure you're manipulating the correct channel set (using `sudo` for root) when rolling back.

---
# Garbage Collection and Its Impact on Rollback

## Freeing Up Storage with `nix-collect-garbage`

Over time, the Nix store accumulates older package versions, consuming significant storage. The `nix-collect-garbage` command reclaims space by deleting unused generations.

## Cautious Garbage Collection

**Important:** Garbage collection removes past system states, making rollbacks to those states impossible.

- **Safer Approach:**
    ```
    nix-collect-garbage --delete-older-than 30d
    ```
    This deletes generations older than 30 days, preserving recent rollback points.
- **Risky Approach (`--delete-old`):** Deletes all unused generations, severely limiting rollback options. Use with extreme caution!

---
# NixOS Flakes: A Paradigm Shift in Rollback

## The `flake.lock` File: Precise Version Control

Traditional NixOS rollbacks rely on preserving system generations. Flakes, through the `flake.lock` file, introduce a superior approach. This file records the exact Git revisions of all dependencies used in your system, enabling precise recreation of past configurations.

### Benefits of `flake.lock`

- **Complete Reproducibility:** Build identical system configurations even after garbage collection.
- **Historical Rollback:** Revert to any point in your Git history, restoring both configuration and specific package versions.
- **Space Efficiency:** No need to retain numerous system generations for rollback purposes.

## Unified Rollback with `git revert`

With flakes, rolling back configuration changes and package versions becomes a unified process using `git revert`. Simply revert the commit that introduced the changes, and your `flake.lock` file will ensure the system is restored to its precise prior state.

---
# Managing Your NixOS Configuration with Git (Detailed)

## Setting Up a Git Repository

1. **Choose a Location:** Designate a directory to house your configuration files (e.g., `~/dotfiles`).
2. **Initialize the Repository:**
    ```
    git init
    ```
3. **Configure Git (Mandatory):**
    ```
    git config --global user.name "Your Name"
    git config --global user.email "you@example.com"
    ```
4. **Set Default Branch (Recommended):** To avoid potential conflicts with remote repositories:
    ```
    git config --global init.defaultBranch main
    ```

## Handling Configurations Owned by Root

If your configuration files reside under `/etc/nixos` (owned by root), you'll need to adjust permissions for seamless Git integration as a regular user.

1. **Create the `.git` Directory as Root:**
    ```
    sudo mkdir /etc/nixos/.git
    ```

2. **Change Ownership to Your User:**
    ```
    sudo chown -R $USER:$USER /etc/nixos/.git
    ```
3. **Initialize the Repository:**
    ```
    git init
    ```
4. **Add Safe Directory:**
    ```
    git config --global --add safe.directory /etc/nixos
    ```
	This informs Git that the directory is managed by you, preventing warnings about ownership.


## Staging, Committing, and Pushing Changes

1. **Stage Changes:** Use `git add <file>` to stage specific files or `git add .` for all changes.
2. **Commit Changes:**
    ```
    git commit -m "Descriptive commit message"
    ```
3. **Set Up a Remote Repository:** Choose a hosting service (GitHub, GitLab, etc.) and create an empty repository.
4. **Connect Your Local Repository to the Remote:**
    ```
    git remote add origin <remote-repository-url>
    ```
5. **Push Changes:**
    ```
    git push origin main
    ```
	Replace `main` with your default branch name if different.


## Restoring Your Configuration on a New System

1. **Clone the Repository:**
    ```
    git clone <remote-repository-url>
    ```
2. **Rebuild Your System:** Follow the standard NixOS procedures to apply your configuration.

## Common Gotchas and Solutions

- **Unstaged Changes:** NixOS detects unstaged changes in your Git repository. Ensure you've staged (`git add`) all new or modified files before rebuilding your system to avoid errors.

---
# Advanced Git Integration with NixOS Flakes

## Running Scripts and Software Directly from Git

Leveraging flakes, you can execute scripts or software directly from your Git repository without explicit installation.

## Understanding the Mechanism

- **`packages` Output in `flake.nix`:** Declare packages or scripts as applications, specifying runtime dependencies using `runtimeInputs`.
- **`apps` Output in `flake.nix`:** Define how these applications should be invoked.
- **`nix run` Command:** Execute applications defined in your flake.

## Setting Up Direct Execution

1. **Define `packages` and `apps` Outputs:** Follow the structure outlined in source, adjusting names, scripts, and dependencies as needed.
2. **Ensure Git is Available:** On a fresh NixOS system, Git might not be installed by default. You can use `nix-shell -p git` to enter a shell with Git.
3. **Run from Git:**
    ```
    nix run github:username/repository#appName
    ```
    - Replace `github:username/repository` with your Git repository information.
    - Replace `appName` with the name defined in your `apps` output.

**Example:**

```
nix run github:LibrePhoenix/nix-config#install
```

**Note:** This example assumes you have a repository named `nix-config` on GitHub under the username `LibrePhoenix` and an application named `install` defined in your flake outputs.

## Benefits

- **Single-Command Installation:** Streamline the setup process on new machines.
- **Dependency Management:** Automatically handle runtime dependencies.
- **Simplified Distribution:** Share and deploy configurations easily.

---
# Conclusion

Mastering rollback mechanisms and configuration management is crucial for a smooth NixOS experience. While traditional methods offer immediate recovery and basic version control, embracing Git and the power of NixOS flakes unlocks a new level of reproducibility, flexibility, and control over your system configuration.

---