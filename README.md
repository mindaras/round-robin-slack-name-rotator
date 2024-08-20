# Round robin responsible person rotator

A job that selects a responsible person every two weeks from the configured list in round robin fashion and posts a message to the Slack channel announcing the name.

### Outcome

Every two weeks, on Monday 09:00, posts a Slack message to the selected channel, announcing the responsible person:
"Responsible person for this sprint: `<name>`"

### Additional features

Availability to trigger manually, selecting a new name index to jump to, in case there's a need to skip someone.

### Configuration

The list of names to choose from can be configured in `responsible-people.json`

### How it works

1. Loads the cached timestamp and name index files
2. Creates a data directory if the cache is empty
3. If it's triggered by a scheduler, checks if it's the second Monday (> 10 days have passed), if it is - updates the cached timestamp file, if it's not - fails the job. This is necessary since the GitHub cron syntax does not support a schedule of running every two weeks, so we're limited to triggering it every week. If it's triggered manually, this step is skipped
4. Selects the name based on the name index saved in the cached file or the provided input if triggered manually
5. Updates the name index in the cached file
6. Posts the message to Slack
7. Invalidates the cache
8. Caches the updated timestamp and name index files
