# Spicetify-Nix

# Warning: currently nonfunctional, WIP
The project I forked this from invoked spicetify-cli in order to change
configuration values. I'm going to have to switch to an ini generator
instead, because spicetify auto-generates the config ini if it's not there
(and seems to always use the default configuration values).

Modifies Spotify using [spicetify-cli](https://github.com/khanhas/spicetify-cli).

[spicetify-themes](https://github.com/morpheusthewhite/spicetify-themes) are included and available, including Dribblish.

To use, add this flake to your home-manager configuration flake inputs, like so:
```nix
spicetify-nix = {
  url = "github:the-argus/spicetify-nix";
  inputs.nixpkgs.follows = "nixpkgs";
};
```

An example of a file which configures spicetify when imported into
a users home-manager configuration:
```nix
{ pkgs, spicetify-nix, ... }:
{
  # import the flake's module
  imports = [ spicetify-nix.homeManagerModule ];

  # configure spicetify :)
  programs.spicetify =
    let
      hidePodcasts = pkgs.fetchgit {
        url = "https://github.com/theRealPadster/spicetify-hide-podcasts";
        rev = "cfda4ce0c3397b0ec38a971af4ff06daba71964d";
        sha256 = "146bz9v94dk699bshbc21yq4y5yc38lq2kkv7w3sjk4x510i0v3q";
      };
    in
    {
      enable = true;
      theme = "Dribbblish";
      colorScheme = "rosepine";
      enabledCustomApps = [ ];
      enabledExtensions = [
        "fullAppDisplay.js"
        "shuffle+.js"
        "hidePodcasts.js"
      ];
      #
      # thirdPartyCustomApps = {
      #   localFiles = "${localFiles}";
      # };
      #
      thirdPartyExtensions = {
        hidePodcasts = "${hidePodcasts}/hidePodcasts.js";
      };
    };
}
```

To add third-party themes, extensions or custom apps use `thirdPartyThemes`, `thirdParyExtensions` or `thirdParyCustomApps`. These expect a set, where the key is the name of the new theme/extension and the value the path. Don't forget to enable it seperatly.

For all available options, check module.nix or package.nix and the spicetify repo. Everything is optional and will revert to the defaults from spicetify.

## macOS
This package has no macOS support, because Spotify in nixpkgs has no macOS support.
