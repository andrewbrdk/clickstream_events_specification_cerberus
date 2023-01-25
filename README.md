### Clickstream events [specification and validation](https://github.com/noooway/clickstream_events_specification_cerberus/blob/main/clickstream_events_spec_and_validation.ipynb) with [Cerberus](https://docs.python-cerberus.org/en/stable/index.html)

```python
from datetime import datetime
from cerberus import Validator

payment_btn_click_scheme = {
    'dt': {'type': 'datetime', 'coerce': lambda x: datetime.strptime(x,'%Y-%m-%d %H:%M:%S')},
    'device_id': {'type': 'string'},
    'user_id': {'type': 'integer'},
    'name': {'type': 'string', 'allowed': ['payment_btn_click']},
    'order_id': {'type': 'integer'}
}
v = Validator(payment_btn_click_scheme, require_all=True)

payment_btn_click_events = [
    {
        #ok
        'dt': '2023-01-10 12:23:01',
        'device_id': 'abc123-efg456',
        'user_id': 1231231,
        'name': 'payment_btn_click',
        'order_id': 101010
    },
    {
        #missing order_id
        'dt': '2023-01-10 12:23:01',
        'device_id': 'abc123-efg456',
        'user_id': 1231231,
        'name': 'payment_btn_click'
    },
    {
        #wrong user_id type
        'dt': '2023-01-10 12:23:01',
        'device_id': 'abc123-efg456',
        'user_id': '1231231',
        'name': 'payment_btn_click',
        'order_id': 101010
    }
]

for i,e in enumerate(payment_btn_click_events):
    if v.validate(e):
        print(f'{i}: ok')
    else:
        print(f'{i}: {v.errors}')

# 0: ok
# 1: {'order_id': ['required field']}
# 2: {'user_id': ['must be of integer type']}
```
