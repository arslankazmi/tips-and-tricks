# Datetime python library tips

important imports:

```python
from datetime import datetime, timedelta, timezone

```

You can get total seconds between two timestamp objects:

```python
min_t = datetime.now()
max_t = datetime.now() + timedelta(secs=1000)
seconds = (max_t - min_t).total_seconds()
```


You can multiply timedeltas:

```python
min_t = datetime.now()
max_t = datetime.now() + timedelta(secs=1000) * 10
```
