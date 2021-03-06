# Emailed Before

A simple Python library for keeping track when emails about some topics were sent.

## Installation

You can install the library from PyPI.


```shell script
pip install emailed-before
```

## Usage

First construct a `SentEmails` instance:

```python
from emailedbefore import SentEmails

sent_emails = SentEmails(sqlite_file)
```

`sqlite_file` is the path to a Sqlite3 file. It will be created if it doesn't exist yet.

You can then register the fact that an email has been sent.

```python
email = "someone@example.com"
topic = "Your library books are due"
sent_emails.register(email, topic)
```

To find out when emails about a topic were sent you can use the `sent_at` method, which returns a list of datetimes. The list is ordered.

```python
from datetime import datetime, timedelta

# Remind the user about those books again after a week.
datetimes = sent_emails.sent_at(email, topic)
if len(datetimes) and datetime.now() - datetimes[-1] > timedelta(days=7):
    ... # email the user
```

For convenience, there is also a method `last_sent_at` which returns the datetime when the last email was sent (or `None` if no email has been sent). So the previous code example can be rewritten as:

```python
from datetime import datetime, timedelta

# Remind the user about those books again after a week.
last_sent = sent_emails.last_sent_at(email, topic)
if last_sent and datetime.now() - last_sent > timedelta(days=7):
    ... # email the user
```
