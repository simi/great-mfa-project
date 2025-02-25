# Token Usage Guide

This is a guide to how to use multi-factor authentication (MFA) tokens,
aka MFA keys.
We focus here on Titan tokens and Yubikey tokens, as those are the kinds of
tokens we are distributing to critical open source software (OSS)
projects, but most of this information applies to other tokens as well.

This guide was created by the Great MFA Distribution project,
part of the OpenSSF.
For an introduction to the project, see the [README](../README.md).

## How to use your MFA token (key)

This documentation provides instructions on how to use an MFA token in
common OSS situations.

* [How to protect your GitHub login](#securing-your-github-login)
* [How to protect your GitHub connection](#securing-your-github-connections)
* [How to protect your GitLab login](#securing-your-gitlab-login)
* How to protect your GitLab connections (TBD)
* [How to secure your npm connections](#securing-your-npm-connections)
* [How to protect your PyPI login](#securing-your-pypi-login)
* How to post a release to Python PyPI (TBD)
* How to post a release to Javascript npm (TBD)
* [How to protect your RubyGems login](#securing-your-rubygems-login)
* [How to secure your SSH connections](#securing-your-ssh-connections)
* [Token unavailable](#token-unavailable)
* [Additional information](#additional-information)

### Token setup

Both the Titan token and Yubikey support the FIDO standard. FIDO keys
generally do not require any special setup on modern systems. Some of
the steps listed below may require newer versions of utilities or
libraries, versions will be specified when appropriate.

Most notably, to secure SSH communications with your MFA token you
need a version of SSH that supports key types such as `ecdsa-sk` or
`ed25519-sk` (the former being supported by older tokens). You can
verify whether you have an adequate version of ssh by doing a simple
`ssh-keygen --help` and checking whether such a type is listed along
with the `-t` option. If not, you need to update your ssh installation
and if none is available for your system install OpenSSH 8.2 or above.

On MacOS if you use [brew](https://brew.sh/) a simple `brew install
openssh` will do that for you.

On Linux if you use Ubuntu you can do a `sudo apt update` and `sudo
apt install openssh-client`.

<!--
On Windows if you use [Chocolotey](https://chocolatey.org/) a simple
`choco install openssh --pre` from the PowerShell will do that for
you. See [Win32 OpenSSH Universal
Installer](https://community.chocolatey.org/packages/openssh) for more
details.

********** This is commented out for now because although the option
exists with this install it doesn't seem to actually work, I get:

C:\Users\lehors>ssh-keygen -t ecdsa-sk
Generating public/private ecdsa-sk key pair.
You may need to touch your authenticator to authorize key generation.
Key enrollment failed: unknown or unsupported key type

***********

-->

You can test your token by visiting the [yubico demo site](https://demo.yubico.com/webauthn-technical/registration). It is expected this test will work on any modern operationg system and updated web browser. Even though the test site is hosted by yubico, any FIDO key can be tested.

Assuming your test worked, please continue with the following instructions.

### Securing your GitHub login

Follow GitHub's instructions to [Protect your GitHub login with a security key](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-security-key).

### Securing your GitHub connections

You need to generate a new SSH key that uses your MFA token following
GitHub's instructions to [Generate a new SSH key for a hardware
security
key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key-for-a-hardware-security-key)
and to [Add a new SSH key to your GitHub
account](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

If you already had an SSH key set up to access your GitHub account you
need to remove it from your GitHub account to make sure you are using
your new key.

Once this is done issuing a `git push` command should ask for a
confirmation with a message such as: `Confirm user presence for key
ECDSA-SK SHA256:xxx`

For video instructions see [Set up your SSH security key in less than two minutes](https://www.youtube.com/watch?v=EbsmqUJy5ag).

GitHub has instructions for enabling a security key for logging into the website
* [Configuring two-factor authentication using a security
key](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-security-key)

At this time, tokens that support FIDO cannot be directly used to commits on GitHub. Please add an upvote to [this discussion](https://github.com/github/feedback/discussions/7744) to enable this feature. The FIDO token can be used to store your SSH key which can be used to push and pull repositories from GitHub. Instructions for using your FIDO token with SSH are included below.

### GitLab

GitLab has instructions for configuring a security key for logging into the website.
* [WebAuthn
device](https://docs.gitlab.com/ee/user/profile/account/two_factor_authentication.html#webauthn-device)

At this time tokens that support FIDO cannot be directly used to commits on GitLab. Please add an upvote to [this discussion](https://gitlab.com/gitlab-org/gitlab/-/issues/343879) to enable this feature. The FIDO token can be used to store your SSH key which can be used to push and pull repositories from GitLab. Instructions for using your FIDO token with SSH are included below.

### Securing your GitLab login

Follow GitLab's instructions to protect your login using a [U2F device](https://docs.gitlab.com/ee/user/profile/account/two_factor_authentication.html#u2f-device).

### Securing your NPM connections

NPM does not support security keys at this time. To use MFA you must use an authenticator app.
* [Configuring two-factor
authentication](https://docs.npmjs.com/configuring-two-factor-authentication)

A package can be configured to require MFA when publishing
* [Requiring 2FA for package publishing and settings modification](https://docs.npmjs.com/requiring-2fa-for-package-publishing-and-settings-modification)

When the 2FA option is configured, the updates can only happen interactively.
* [See the --otp option](https://docs.npmjs.com/cli/v8/commands/npm-publish)

<a name="PyPI_login"/></a>
### Securing your PyPI login

To protect your login to PyPi, follow PyPI's documentation on [How does two factor authentication with a security device (e.g. USB key) work?
How do I set it up on PyPI?](https://pypi.org/help/#utfkey)

Using a security key with PyPI is only needed to login to the website. Packages can still be pushed using a username and passowrd or an API token. The PyPI API token documentation can be found [here](https://pypi.org/help/#apitoken).

### Securing your RubyGems login

RubyGems does not support security keys. To use MFA you must use an
authenticator app.
* [SETTING UP MULTIFACTOR
AUTHENTICATION](https://guides.rubygems.org/setting-up-multifactor-authentication/)

### Securing your SSH connections

You can protect your SSH connections with your MFA token. This means
SSH authentication can only happen with the MFA token plugged into the
machine. See the very nice article on [How to use FIDO2 USB
authenticators with
SSH](https://www.stavros.io/posts/u2f-fido2-with-ssh/) for generic
instructions.

This SSH key can then be used to push and pull from Git repositories
as well as logging into remote systems. Git version 2.34 and above
supports signing commits with an SSH key, however GitHub and GitLab do
not support verifying SSH signatures at this time.

### Token unavailable

Most of the code repository platforms offer recovery methods to gain access
to the account when you are dealing with a token that has
become unavailable (e.g., it has become lost, broken, misplaced, or stolen).
For example the most common default fail-safe mechanism is the 
secrets one time use codes that are automatically generated when you 
enrolled your hardware token. Using this mechanism it’s
*extremely* important to 
store these secret one time use codes in a safe matter. You can achieve this by 
printing them out and store them in a physical vault.
You could also use a digital secure password storage, but then you need to
make sure attackers can't access that.
We suggest printing them out twice, storing one near you and another
in a secure location far away from you (so that a fire in a building
won't eliminate access).

If you don’t like this approach there are often special per-platform options.
For example, with [GitHub you can use your phone number and SMS](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication-recovery-methods#downloading-your-two-factor-authentication-recovery-codes)
to create a fail-safe mechanism to regain access to your account.
[GitLab offers other fail-safe options](https://about.gitlab.com/blog/2018/08/09/keeping-your-account-safe/) - you can use a backup email address or use your SSH keys that are connected to the account to can generate new secret one time use codes.

### Additional information

The [Yubikey Guide](http://github.com/drduh/YubiKey-Guide) is a relatively
exhaustive guide for Yubikeys.
