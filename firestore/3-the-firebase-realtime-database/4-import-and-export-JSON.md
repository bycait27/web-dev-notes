# import and export JSON
**JSON:** uses human-readable text to store and share data over a network

```
[
  {
    "user_id": 1,
    "tasks": [
      {
        "task": "Lorem ipsum dolor amet",
        "difficulty": "easy"
      },
      {
        "task": "laboriosam mollitia et enim quasi",
        "difficulty": "medium"
      }
    ]
  },
  {
    "user_id": 2,
     "tasks": [
      {
        "task": "qui ullam ratione quibusdam volu",
        "difficulty": "hard"
      }
    ]
  }
]
```

firebase allows us to import and export JSON files to and from our app database
- populate database with test data
- transfer only a part of a bulky database

## importing data
1. navigate to the database node where we wish to import the JSON files
2. click the “more” icon at the top of the realtime database container
3. click the “import JSON” option
4. click the “browse” button to select the file from your local device

    a. only files with the .json extension can be selected
5. click the “import” buttom to import them

*OVERWRITES existing data

## exporting data
1. navigate to the database node with the data we wish to export
2. click the “more” icon at the top of the realtime database container
3. click the “export JSON” option and save the file to a location on your device

*FOLLOWS the existing database rules