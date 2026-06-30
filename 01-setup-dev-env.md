# Set up your development environment

First, set up your development environment.

You should now have access to a [spreadsheet](http://hklink.to/workshop) with credentials and information.
The organizers should have assigned you to a team and shared your team number.

Follow the steps below to get ready for the next stage.

Before you start, make sure you have your GitHub account, access to the workshop spreadsheet, and your assigned team number.

## Get the GitHub organization name

In the [spreadsheet](http://hklink.to/workshop), you should find your GitHub organization.
If everything went smoothly, the organizers should have invited your GitHub account to the org.
You can accept pending invitations at [github.com/settings/organizations](https://github.com/settings/organizations).
If in doubt, ask the organizers.

This is your organization for this workshop. It is not shared with anyone else, so everything already in it is yours to use.
Before moving on, confirm that you can open the organization in GitHub and see its repositories.

## Get the URL of the ephemeral dev environment

In the [spreadsheet](http://hklink.to/workshop), you will find the URL of your dev environment.
This is a web-based IDE built on VS Code, so if you are familiar with it, you will feel at home.

The organizers will share the password needed to log in once everyone is ready to start.
In the meantime, you can look around your organization and discover the content already available.

Once you log in, open `/home/coder/work` first. That is the directory where you should create or clone all workshop repositories.
Your first action in the dev environment should be to clone this workshop repository there as `_cicada-sense-workshop`.

## What you should know

⚠️⚠️⚠️ **IMPORTANT** ⚠️⚠️⚠️ Only `/home/coder/work` is persisted. Create, clone, and keep all workshop repositories there.

- The instance is a Debian-based container, and the default shell is zsh
- Normally, all tools needed are already installed
- If something is missing, you can install it with a command like `sudo apt update && sudo apt install cowsay`
- This environment uses [mise](https://mise.jdx.dev/) for developer experience, so if you know it, feel free to use it
- Do not change tool versions, for reproducibility

## Set up your environment (finally)

Open a terminal in the dev environment and run the following commands from `/home/coder/work`, in this order:

```bash
mise install # just to be sure
mise run ssh:setup # no passkey needed
mise run gnupg:setup # no passkey needed
git clone https://github.com/hoverkraft-tech/cicada-sense-workshop.git _cicada-sense-workshop
```

⚠️⚠️⚠️ **IMPORTANT** ⚠️⚠️⚠️ The SSH command will ask you at the end to add the generated SSH key to your account.
When that happens, add the key in GitHub, then come back to the terminal and continue.
Do not worry: we will not keep the keys after the workshop, and you can delete them right after the workshop ends.
This is the only way to commit to the organization repository without many compromises.

You now have a ready-to-use development environment with a dedicated SSH key and a GPG key, fully configured.

Before moving to the next step, confirm these points:

1. you can access your workshop GitHub organization
2. you are logged in to the dev environment
3. `mise run ssh:setup` completed and the SSH key was added to GitHub
4. `mise run gnupg:setup` completed successfully
5. `_cicada-sense-workshop` exists under `/home/coder/work`

It is time to start your new dedicated platform!
