---
time created: 2024-09-28 16:14
tags:
  - linux
  - academics
references:
  - "[[Linux Helpbox]]"
source: https://www.youtube.com/watch?v=6WLaNIlDW0M&list=PL_WcXIXdDWWpuypAEKzZF2b5PijTluxRG
---

---

# An Overview

This documentation provides a comprehensive overview of NixOS configuration, focusing on custom options, conditional logic with if-else statements, and the power of special arguments (special args) in NixOS flakes.

---

# Understanding NixOS Options

In NixOS, the entire system configuration is defined in a file named `configuration.nix`. This file uses the Nix expression language, a purely functional language that might feel different from imperative languages like Python or JavaScript.

## Defining Custom Options

While NixOS provides a vast collection of pre-defined options for configuring various aspects of the system, you might encounter situations where you need to introduce your own custom options. This is achieved by leveraging the `options` attribute set within your `configuration.nix` file.

- **The `options` Attribute Set:** This special attribute set is where you define your custom options. Unlike directly setting options to values as you would in other programming languages, defining an option in NixOS involves specifying its **type** and **default value**.
- **Using `lib.makeOption`:** The `lib.makeOption` function from the Nix library (`lib`) is instrumental in defining custom options. It takes two arguments:
  - **Type (`type`):** Specifies the data type of the option. For instance, `lib.types.str` indicates a string type.
  - **Default Value (`default`):** Sets the default value for the option. If the option is not set elsewhere in your configuration, this default value will be used.

**Example:**

```
options = {
  myArbitraryOption = lib.makeOption {
    type = lib.types.str;
    default = "stuff";
  };
};
```

In this example, we define a custom option named `myArbitraryOption` as a string with the default value "stuff".

## Common NixOS Option Types

NixOS provides a wide array of types for options. Here are a few common ones:

- **`lib.types.anything`:** Accepts any value.
- **`lib.types.str`:** Represents a string.
- **`lib.types.strNonEmpty`:** Represents a string that cannot be empty.
- **`lib.types.bool`:** Represents a Boolean value (true or false).
- **`lib.types.path`:** Represents a path to a file or directory.
- **`lib.types.listOf`:** Represents a list of a specific type.

You can explore the full range of available types within the Nix Packages source code. The file containing type definitions is linked in the description of source.

## Utilizing Custom Options

Once you define a custom option, you can use it throughout your configuration like any other NixOS option. A key strength of NixOS options lies in their modularity: you can set and override them at various levels of your configuration. This allows for fine-grained control and customization.

### The `config` Attribute Set

It's crucial to understand that the entire NixOS configuration, including option declarations and their values, is implicitly enclosed within a top-level attribute set named `config`. When you write `boot.loader.grub.enable = true;`, you're essentially writing `config.boot.loader.grub.enable = true;`.

To ensure clarity and avoid potential conflicts, especially when defining custom options, it's recommended to structure your `configuration.nix` with explicit `options` and `config` attribute sets:

```
{
  imports = [
    # Your module imports
  ];

  options = {
    # Your custom option definitions
  };

  config = {
    # Your configuration settings using both default and custom options
  };
}
```

### Conditional Logic with If-Else Statements

Nix, being declarative, handles if-else statements differently compared to imperative languages. Instead of executing different blocks of code based on a condition, Nix if-else expressions evaluate to different values.

**Imperative (e.g., Bash):**

```
if [ condition ]; then
  # Do something
else
  # Do something else
fi
```

**Declarative (Nix):**

```
let
  message = if condition then "Condition is true" else "Condition is false";
in
message
```

In the Nix example, the `message` variable is assigned the value "Condition is true" if `condition` is true; otherwise, it's assigned "Condition is false." The entire `if` expression evaluates to a single value that is then assigned to the variable.

---

# Conditional Package Installation

A practical use of if-else statements in NixOS is the conditional installation of packages. You can leverage conditional logic within your `environment.systemPackages` or `home.packages` (if using Home Manager) to include packages based on other configuration settings.

**Example:**

```
environment.systemPackages = with packages; [
  vim
  wget
] ++ (
  if config.services.xserver.windowManager.xmonad.enable == true then
    [ packages.rofi ]
  else if config.programs.hyprland.enable == true then
    [ packages.fuzzle ]
  else
    [ ]
);
```

In this example, we conditionally add packages to the `environment.systemPackages` list:

- If `xmonad` is enabled (`config.services.xserver.windowManager.xmonad.enable` is true), the package `rofi` is added.
- If `hyprland` is enabled (`config.programs.hyprland.enable` is true), the package `fuzzle` is added.
- If neither is enabled, an empty list (`[]`) is added, meaning no extra package is installed.

### Important Considerations for If-Else Statements

- **Both `if` and `else` are Required:** Unlike some other languages, Nix requires both an `if` and an `else` branch in your conditional statements. This ensures that the expression always evaluates to a value.
- **Parentheses for Clarity:** Using parentheses to enclose your conditional logic, especially when it becomes complex, can significantly enhance readability.

---

# Special Args in NixOS Flakes

Nix flakes introduce a powerful feature called "special args" that simplifies configuration management, particularly for users with complex setups or those managing configurations across multiple machines.

## What are Special Args?

Special args provide a way to pass variables from your `flake.nix` file (the heart of a Nix flake) down to various parts of your NixOS configuration. This is particularly useful for settings that need to be accessed by both system-level (`configuration.nix`) and user-level (e.g., Home Manager) modules.

## Setting Up Special Args

1. **Declare Variables:** In your `flake.nix` file, within the `outputs` definition, declare the variables you want to use as special args. It's generally recommended to use a `let` binding for organization:

   ```
   outputs = { self, nixpkgs, ... }@args: let
     # System Settings
     system = "x86_64-linux";
     hostname = "my-nixos-system";
     username = "myusername";

     # User Settings
     editor = "vim";
     browser = "firefox";
   in {
     # ... rest of your flake outputs
   };
   ```

2. **Pass Variables to NixOS and Home Manager:** When defining the `nixosConfiguration` and `homeManagerConfiguration` outputs in your `flake.nix`, pass the declared variables using the `specialArgs` (for `nixosConfiguration`) or `extraSpecialArgs` (for `homeManagerConfiguration`) arguments:

   ```
   nixosConfiguration = nixpkgs.lib.nixosSystem {
     system = system;
     modules = [
       # ... your modules
     ];
     specialArgs = { inherit username hostname; };
   };

   homeManagerConfiguration = {pkgs, ...}: {
       imports = [
           # ... your home-manager modules
       ];
       extraSpecialArgs = { inherit username editor browser; };
   };
   ```

3. **Access Variables in Modules:** In your configuration modules (`configuration.nix`, `home.nix`, and any other imported modules), include the special args as arguments to the module functions:

   ```
   # configuration.nix
   { config, pkgs, username, hostname }:

   {
     # Access special args
     users.users.${username} = { ... };
     networking.hostName = hostname;
   }
   ```

   ```
   # home.nix
   { config, pkgs, lib, username, editor, browser }:

   {
     home.username = username;
     home.packages = with pkgs; [ editor browser ];
   }
   ```

## Benefits of Special Args

- **Centralized Management:** Manage critical configuration settings in a single location (`flake.nix`).
- **Improved Modularity:** Easily reuse and adapt your configuration across different machines or environments by changing the values of special args.
- **Enhanced Readability:** Make your configuration files cleaner and easier to understand.

## Using Attribute Sets for Special Args

To further streamline your configuration, especially when dealing with numerous special args, consider grouping them into attribute sets:

```
# flake.nix
outputs = { self, nixpkgs, ... }@args: let
  # ... other variable declarations

  systemSettings = {
     system = "x86_64-linux";
     hostname = "my-nixos-system";
     # ... other system settings
   };

  userSettings = {
     username = "myusername";
     editor = "vim";
     browser = "firefox";
     # ... other user settings
   };
in {
  # ... your flake outputs

  nixosConfiguration = nixpkgs.lib.nixosSystem {
    # ... other configuration
    specialArgs = { inherit (systemSettings) hostname; };
  };

  homeManagerConfiguration = {pkgs, ...}: {
      # ... other configuration
      extraSpecialArgs = { inherit (userSettings) username; };
  };
};
```

This approach reduces the number of arguments you need to pass to individual modules, improving readability and maintainability.

## Recursive Attribute Sets (The `rec` Keyword)

In some scenarios, you might want to define the value of a special arg based on other values within the same attribute set. This is where recursive attribute sets come in. By using the `rec` keyword when defining your attribute set, you enable the calculation of values within the set based on other values within the same set.

**Example:**

```
userSettings = rec {
  username = "myusername";
  editor = "vim";
  spawnEditor =
    if userSettings.editor == "vim" then
      "${pkgs.st}/bin/st -e ${userSettings.editor}"
    else
      userSettings.editor;
};
```

In this example, `spawnEditor`, a special arg within the `userSettings` attribute set, is dynamically determined based on the value of another special arg, `editor`, within the same set. The `rec` keyword is crucial here, as it enables this recursive calculation. Without it, referring to `userSettings.editor` within the definition of `spawnEditor` would lead to an error.

---

# Conclusion

This detailed documentation has covered various aspects of NixOS configuration, ranging from defining custom options and using conditional logic to harnessing the power of special args in Nix flakes. Remember, NixOS configuration is highly flexible and customizable. Explore different approaches, experiment with various options, and tailor your system to perfectly match your requirements!
