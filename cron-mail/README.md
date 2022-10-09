## Design a Job that sends Mail to Users

- Requirements
  - User can select preferences: digest or non-digest
  - When user makes a change to the system, he/she needs to be notified

- HLD
  - store user preference in the db: emailoff, emaildigest, etc
  - each time users make changes store into the emails table and marked it as pending: each action will have its templates of what to send: template name + template values
  - cron job to perform the digest and non-digest
  - mark the entry in email table as processed after putting the values into templates and sent when sent is done, or error when there is an error

- Digest
  - run once a day
  - STATUS_DIGEST_PENDING, STATUS_DIGEST_PROCESSED

- Nodigest
  - run every minute
  - STATUS_PENDING, STATUS_PROCESSED
  - when processing the rows in email table - check if the recepient wants digest - if so then we will mark it as digest pending and send later on during another cron, otherwise will proceed right away