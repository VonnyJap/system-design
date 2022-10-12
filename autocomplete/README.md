## Autocomplete Search

- requirements as follows:
  - Our system can expect 4 billion queries per day.
  - We need 7 autocomplete suggestions for each character we enter in the search box
  - It must be a real-time service. If a search query is trending, we must add it to our database, so that we can include it in our autocomplete suggestions.

- System APIS
  - get_suggestions(prefix)
  - add_to_db(query)

- [Links](https://systemdesignprep.com/autocomplete)