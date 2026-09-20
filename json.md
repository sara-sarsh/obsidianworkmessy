```python
import json
```
That gives you access to functions like:

json.loads() – JSON string ➜ Python object
json.dumps() – Python object ➜ JSON string
json.load() – JSON file ➜ Python object
json.dump() – Python object ➜ JSON file

## Code examples:
### loads
```python
import json

data = '{"name": "Alice", "age": 25, "is_student": false}'

python_dict = json.loads(data)

print(python_dict)
print(type(python_dict))


ouput:
{'name': 'Alice', 'age': 25, 'is_student': False}
<class 'dict'>


```
### dumps
```python
import json

data = {
    "name": "Bob",
    "age": 30,
    "skills": ["Python", "SQL"]
}

json_string = json.dumps(data)

print(json_string)


output:
{"name": "Bob", "age": 30, "skills": ["Python", "SQL"]}

```
Use indent so it’s readable.
```python
json_string = json.dumps(data, indent=4)

print(json_string)
Output:

text
{
    "name": "Bob",
    "age": 30,
    "skills": [
        "Python",
        "SQL"
    ]
}
```

### load
```python
import json

with open("data.json", "r") as file:
    data = json.load(file)

print(data)
print(data["name"])
```
### dump
```python
import json

data = {
    "name": "Dana",
    "age": 22,
    "city": "London"
}

with open("data.json", "w") as file:
    json.dump(data, file, indent=4)
```