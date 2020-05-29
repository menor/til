# Postgres

## Debug postgres
I had the problem of postgres crashing with some weird error. To debug I did the following:
- Started postgres service through homebrew `brew services start postgresql`
- Ran `brew services list` to see to whic file the errors where logged
- Tailed the file (`tail -n 100 /usr/local/var/log/postgres.log`) and saw that the error was about the data being created by a previous version of postgres
(see stack overflow response [here](https://stackoverflow.com/a/47673746/2565132))

## Update postgres data
- To update data from previous postgres installations you can run `brew postgresql-upgrade-database` and the data will be migrated to the new version

