# Before you begin

Before you begin, please familiarize yourself with the [contributing](contributing.md) document.

Also, make sure the project meets our standards for inclusion.

* Services should be in development for at least a year.
* Services should be actively maintained. (Last commit within 6 months.)

# Using the addPkg.rb script to Add services to HomelabOS

## What does the script do?

addPkg.rb scripts the creation of new service files. You'll need three pieces of information:

- Package Name in Title case - This is used whenever we need a Title for the package.
- The URL for the package - Used in documentation files to link to package source.
- A one-line description of the package - Used in documention, etc.

When you have entered those three pieces of information, The script then does the following for you:

- Creates an issue on Gitlab.
- Creates a branch for, and tied to the issue.
- Creates an (empty) Merge Request, that resolves the issue.
- Fetches the new branch, and checks it out.
- Creates the Service Role Directory
  - Edits the role/PACKAGENAME/tasks/main.yml
- Creates the Documentation file
- Edits mkdocs.yml to include the new documentation file
- Edits the Readme, and Changelog files
- Edits the group_var/all file to include the new package in the Enabled Services list

## Running the script

From the root project directory run:
`./add_package.sh` and answer the 3 questions.
Once the script has run, you must edit the `roles/PACKAGENAME/templates/docker-compose.PACKAGENAME.yml.j2 file`

_Please review all other files, before pushing your changes to gitlab._

# How to Manually Add Services to HomelabOS

## Create Role Folder

Copy an existing role folder like 'inventario' from the `roles/` folder,
then adapt the values as needed.

### Use hardcoded volume paths

All mounted docker volumes should point to a folder named after the service that is using it, and located under `{{ volumes_root }}`.

## Add Service to Documentation

### Create a Documentation Page

Each service should have it's own page within the `docs/software/` folder.
Use existing docs as a template.

### Link to Documentation Page

Update the `mkdocs.yml` file with a reference to the newly created doc file.

## Add Service to Inventory File

The service needs to be added within
`group_vars/all`.

Place it in the `services:` section in alphabetical order.

## Add Service to README

The service should be added under the list of `Available Software`.

## Add Service to `config.yml.j2`

In the config template `roles/homelabos_config/templates/config.yml.j2` the
service should be added in alphabetical order under the `# Services List` section.

## Add Service to CHANGELOG.md

Add at the top of the file, rather than under the previously newest release.

# How to Debug a New Service

After a new service has been deployed, run `systemctl status SERVICE_NAME` to see
how it's doing.

If it's not running with an error like `(code=exited, status=1/FAILURE)`

Grab the value of the ExecStart line, and run it by hand. So if the ExecStart line looks like:
`ExecStart=/usr/bin/docker-compose -p zulip up`
then manually change directory `cd {{ volumes_root }}/zulip` and run the bit after the =, `/usr/bin/docker-compose -p zulip up` to see the error output directly.
