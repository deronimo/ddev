# Uninstalling DDEV

A DDEV-Local installation consists of:

* The binary itself (self-contained)
* The .ddev folder in a project
* The ~/.ddev folder where various global items are stored.
* The docker images and containers created.
* Any entries in /etc/hosts

Please make backups of your databases before deleting projects or uninstalling. You can do this with `ddev snapshot` or `ddev export-db`.

You can use `ddev clean` to uninstall the vast majority of things DDEV has touched. For example, `ddev clean <project>` or `ddev clean --all`.

To uninstall just a project: `ddev delete <project>`. This removes any hostnames in `/etc/hosts` and removes your database. If you don't want it to make a database backup/snapshot on the way down: `ddev delete --omit-snapshot <project>`

To remove all /etc/hosts entries owned by ddev: `ddev hostname --remove-inactive`

To remove the global .ddev directory: `rm -r ~/.ddev`

If you installed docker just for ddev and want to uninstall it with all containers and images, just uninstall it for your version of Docker.

Otherwise:

* Remove Docker images from before the current ddev release with `ddev delete images`.
* To remove all ddev docker containers that might still exist: `docker rm $(docker ps -a | awk '/ddev/ { print $1 }')`
* To remove all ddev docker images that might exist: `docker rmi $(docker images | awk '/ddev/ {print $3}')`
* To remove all Docker images of any type (does no harm, they'll just be re-downloaded): `docker rmi -f $(docker images -q)`
* To remove any docker volumes: `docker volume rm $(docker volume ls | awk '/ddev|-mariadb/ { print $2 }')`

To remove the ddev binary:

* On macOS or Linux with Homebrew, `brew uninstall ddev`
* For linux or other simple installs, just remove the binary, for example `sudo rm /usr/local/bin/ddev`
* On Windows (if you used the ddev Windows installer) use the uninstall on the start menu or in the "Add or Remove Programs" section of Windows settings.
