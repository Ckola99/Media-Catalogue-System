# Media Catalogue System

## Overview

A **Python-based media cataloguing system** that manages movies and TV series using object-oriented programming principles. The project demonstrates inheritance, custom exceptions, input validation, and polymorphism through a simple yet robust media management interface.

---

## Features

- 🎬 **Movie Management**: Store and track movie information
- 📺 **TV Series Support**: Manage complete TV series with seasons and episodes
- ✅ **Input Validation**: Comprehensive validation for all media attributes
- 🛡️ **Custom Exceptions**: Specialized error handling for media operations
- 📚 **Catalogue System**: Organize and display media collections
- 🔍 **Type Filtering**: Separate movies from TV series
- 🎯 **Inheritance**: TVSeries inherits from Movie base class

---

## Classes

### `MediaError`

Custom exception class for media-related errors.

#### Constructor

```python
MediaError(message, obj)
```

**Parameters:**
- `message` (str): Error description
- `obj` (object): The object that caused the error

**Attributes:**
- `obj`: Stores the problematic object for debugging

**Example:**
```python
try:
    catalogue.add("Not a movie")
except MediaError as e:
    print(f'Error: {e}')
    print(f'Object: {e.obj}')
```

---

### `Movie`

Base class representing a movie.

#### Constructor

```python
Movie(title, year, director, duration)
```

**Parameters:**
- `title` (str): Movie title (cannot be empty or whitespace)
- `year` (int): Release year (must be 1895 or later)
- `director` (str): Director's name (cannot be empty or whitespace)
- `duration` (int): Movie duration in minutes (must be positive)

**Validation Rules:**
- Title must contain non-whitespace characters
- Year must be >= 1895 (first movie year in history)
- Director must contain non-whitespace characters
- Duration must be > 0

**Raises:**
- `ValueError`: If any validation rule is violated

**Example:**
```python
# Valid movie
movie = Movie('The Matrix', 1999, 'The Wachowskis', 136)

# Invalid movies
Movie('', 1999, 'Director', 120)  # ValueError: Title cannot be empty
Movie('Title', 1800, 'Director', 120)  # ValueError: Year must be 1895 or later
Movie('Title', 1999, '', 120)  # ValueError: Director cannot be empty
Movie('Title', 1999, 'Director', 0)  # ValueError: Duration must be positive
```

#### Methods

##### `__str__()`

Returns a formatted string representation of the movie.

**Format:** `{title} ({year}) - {duration} min, {director}`

**Example:**
```python
movie = Movie('Inception', 2010, 'Christopher Nolan', 148)
print(movie)
# Output: Inception (2010) - 148 min, Christopher Nolan
```

---

### `TVSeries`

Child class representing a TV series. Inherits from `Movie`.

#### Constructor

```python
TVSeries(title, year, director, duration, seasons, total_episodes)
```

**Parameters:**
- `title` (str): Series title
- `year` (int): First air year
- `director` (str): Creator/director name
- `duration` (int): Average episode duration in minutes
- `seasons` (int): Number of seasons (must be >= 1)
- `total_episodes` (int): Total number of episodes (must be >= 1)

**Additional Validation Rules:**
- Seasons must be >= 1
- Total episodes must be >= 1
- All Movie validation rules also apply

**Raises:**
- `ValueError`: If any validation rule is violated

**Example:**
```python
# Valid TV series
series = TVSeries('Breaking Bad', 2008, 'Vince Gilligan', 47, 5, 62)

# Invalid series
TVSeries('Title', 2020, 'Director', 30, 0, 10)  # ValueError: Seasons must be 1 or greater
TVSeries('Title', 2020, 'Director', 30, 5, 0)  # ValueError: Total episodes must be 1 or greater
```

#### Methods

##### `__str__()`

Returns a formatted string representation of the TV series.

**Format:** `{title} ({year}) - {seasons} seasons, {total_episodes} episodes, {duration} min avg, {director}`

**Example:**
```python
series = TVSeries('Scrubs', 2001, 'Bill Lawrence', 24, 9, 182)
print(series)
# Output: Scrubs (2001) - 9 seasons, 182 episodes, 24 min avg, Bill Lawrence
```

---

### `MediaCatalogue`

Container class for managing a collection of media items.

#### Constructor

```python
MediaCatalogue()
```

Initializes an empty catalogue.

**Example:**
```python
catalogue = MediaCatalogue()
```

#### Attributes

##### `items`
List containing all media items (Movie and TVSeries objects).

---

#### Methods

##### `add(media_item)`

Adds a media item to the catalogue.

**Parameters:**
- `media_item` (Movie or TVSeries): The media item to add

**Raises:**
- `MediaError`: If the item is not a Movie or TVSeries instance

**Example:**
```python
catalogue = MediaCatalogue()
movie = Movie('Inception', 2010, 'Christopher Nolan', 148)
catalogue.add(movie)  # Success

catalogue.add("Not a movie")  # Raises MediaError
```

---

##### `get_movies()`

Returns a list of all Movie objects (excludes TV series).

**Returns:** list of Movie objects

**Example:**
```python
movies = catalogue.get_movies()
print(f'Found {len(movies)} movies')
```

---

##### `get_tv_series()`

Returns a list of all TVSeries objects.

**Returns:** list of TVSeries objects

**Example:**
```python
series = catalogue.get_tv_series()
print(f'Found {len(series)} TV series')
```

---

##### `__str__()`

Returns a formatted string representation of the entire catalogue.

**Format:**
```
Media Catalogue ({count} items):

=== MOVIES ===
1. {movie1}
2. {movie2}

=== TV SERIES ===
1. {series1}
2. {series2}
```

**Example:**
```python
print(catalogue)
```

**Output:**
```
Media Catalogue (4 items):

=== MOVIES ===
1. The Matrix (1999) - 136 min, The Wachowskis
2. Inception (2010) - 148 min, Christopher Nolan

=== TV SERIES ===
1. Scrubs (2001) - 9 seasons, 182 episodes, 24 min avg, Bill Lawrence
2. Breaking Bad (2008) - 5 seasons, 62 episodes, 47 min avg, Vince Gilligan
```

---

## Usage

### Basic Example

```python
from media_catalogue import Movie, TVSeries, MediaCatalogue, MediaError

# Create a catalogue
catalogue = MediaCatalogue()

# Add movies
movie1 = Movie('The Matrix', 1999, 'The Wachowskis', 136)
movie2 = Movie('Inception', 2010, 'Christopher Nolan', 148)
catalogue.add(movie1)
catalogue.add(movie2)

# Add TV series
series = TVSeries('Breaking Bad', 2008, 'Vince Gilligan', 47, 5, 62)
catalogue.add(series)

# Display catalogue
print(catalogue)

# Get specific types
movies = catalogue.get_movies()
tv_shows = catalogue.get_tv_series()

print(f'\nTotal movies: {len(movies)}')
print(f'Total TV series: {len(tv_shows)}')
```

---

### Error Handling Example

```python
catalogue = MediaCatalogue()

try:
    # This will raise ValueError
    invalid_movie = Movie('', 1999, 'Director', 120)
except ValueError as e:
    print(f'Validation Error: {e}')

try:
    # This will raise MediaError
    catalogue.add("Not a movie object")
except MediaError as e:
    print(f'Media Error: {e}')
    print(f'Problematic object: {e.obj}')

try:
    # This will raise ValueError
    invalid_series = TVSeries('Title', 2020, 'Director', 30, 0, 10)
except ValueError as e:
    print(f'Validation Error: {e}')
```

---

### Complete Workflow

```python
def main():
    catalogue = MediaCatalogue()
    
    try:
        # Add movies
        movie1 = Movie('The Matrix', 1999, 'The Wachowskis', 136)
        catalogue.add(movie1)
        
        movie2 = Movie('Inception', 2010, 'Christopher Nolan', 148)
        catalogue.add(movie2)
        
        movie3 = Movie('Interstellar', 2014, 'Christopher Nolan', 169)
        catalogue.add(movie3)
        
        # Add TV series
        series1 = TVSeries('Scrubs', 2001, 'Bill Lawrence', 24, 9, 182)
        catalogue.add(series1)
        
        series2 = TVSeries('Breaking Bad', 2008, 'Vince Gilligan', 47, 5, 62)
        catalogue.add(series2)
        
        series3 = TVSeries('The Office', 2005, 'Greg Daniels', 22, 9, 201)
        catalogue.add(series3)
        
        # Display full catalogue
        print(catalogue)
        
        # Filter by type
        print('\n' + '='*50)
        print('MOVIES ONLY:')
        for movie in catalogue.get_movies():
            print(f'  • {movie}')
        
        print('\n' + '='*50)
        print('TV SERIES ONLY:')
        for series in catalogue.get_tv_series():
            print(f'  • {series}')
            
    except ValueError as e:
        print(f'Validation Error: {e}')
    except MediaError as e:
        print(f'Media Error: {e}')
        print(f'Unable to add {e.obj}: {type(e.obj)}')

if __name__ == '__main__':
    main()
```

---

## OOP Concepts Demonstrated

### 1. **Inheritance**

`TVSeries` inherits from `Movie`, reusing common attributes and validation:

```python
class TVSeries(Movie):
    def __init__(self, title, year, director, duration, seasons, total_episodes):
        super().__init__(title, year, director, duration)  # Inherit validation
        # Add TV-specific attributes
```

### 2. **Polymorphism**

Both `Movie` and `TVSeries` can be stored in the same catalogue:

```python
catalogue.add(movie)   # Movie instance
catalogue.add(series)  # TVSeries instance
```

### 3. **Encapsulation**

Data validation is encapsulated within class constructors:

```python
if not title.strip():
    raise ValueError('Title cannot be empty')
```

### 4. **Custom Exceptions**

`MediaError` provides specific error handling with context:

```python
class MediaError(Exception):
    def __init__(self, message, obj):
        super().__init__(message)
        self.obj = obj  # Store problematic object
```

### 5. **Type Checking**

The catalogue uses `isinstance()` and `type()` for type validation:

```python
# Accepts Movie and subclasses
if not isinstance(media_item, Movie):
    raise MediaError('Only Movie or TVSeries instances can be added', media_item)

# Gets only Movie instances (not TVSeries)
def get_movies(self):
    return [item for item in self.items if type(item) is Movie]
```

---

## Validation Rules Summary

| Field | Rule | Exception |
|-------|------|-----------|
| **Title** | Non-empty, non-whitespace string | `ValueError` |
| **Year** | Integer >= 1895 | `ValueError` |
| **Director** | Non-empty, non-whitespace string | `ValueError` |
| **Duration** | Positive integer | `ValueError` |
| **Seasons** | Integer >= 1 | `ValueError` |
| **Episodes** | Integer >= 1 | `ValueError` |
| **Add to Catalogue** | Must be Movie or TVSeries instance | `MediaError` |

---

## Class Hierarchy

```
Exception
    └── MediaError

Movie
    ├── __init__(title, year, director, duration)
    └── __str__()
    
    └── TVSeries (inherits from Movie)
        ├── __init__(title, year, director, duration, seasons, total_episodes)
        └── __str__() [overridden]

MediaCatalogue
    ├── __init__()
    ├── add(media_item)
    ├── get_movies()
    ├── get_tv_series()
    └── __str__()
```

---

## Testing

### Test Case 1: Valid Movie Creation

```python
def test_valid_movie():
    movie = Movie('Inception', 2010, 'Christopher Nolan', 148)
    assert movie.title == 'Inception'
    assert movie.year == 2010
    assert movie.director == 'Christopher Nolan'
    assert movie.duration == 148
```

### Test Case 2: Invalid Movie Creation

```python
def test_invalid_movie():
    # Empty title
    try:
        Movie('', 1999, 'Director', 120)
        assert False, 'Should raise ValueError'
    except ValueError as e:
        assert str(e) == 'Title cannot be empty'
    
    # Invalid year
    try:
        Movie('Title', 1800, 'Director', 120)
        assert False, 'Should raise ValueError'
    except ValueError as e:
        assert str(e) == 'Year must be 1895 or later'
```

### Test Case 3: Valid TV Series

```python
def test_valid_tv_series():
    series = TVSeries('Breaking Bad', 2008, 'Vince Gilligan', 47, 5, 62)
    assert series.seasons == 5
    assert series.total_episodes == 62
```

### Test Case 4: Catalogue Operations

```python
def test_catalogue():
    catalogue = MediaCatalogue()
    movie = Movie('Inception', 2010, 'Christopher Nolan', 148)
    series = TVSeries('Breaking Bad', 2008, 'Vince Gilligan', 47, 5, 62)
    
    catalogue.add(movie)
    catalogue.add(series)
    
    assert len(catalogue.items) == 2
    assert len(catalogue.get_movies()) == 1
    assert len(catalogue.get_tv_series()) == 1
```

### Test Case 5: MediaError

```python
def test_media_error():
    catalogue = MediaCatalogue()
    
    try:
        catalogue.add("Not a movie")
        assert False, 'Should raise MediaError'
    except MediaError as e:
        assert e.obj == "Not a movie"
```

---

## Common Use Cases

### Personal Movie Library

```python
# Build your personal collection
my_library = MediaCatalogue()

# Add favorite movies
my_library.add(Movie('The Shawshank Redemption', 1994, 'Frank Darabont', 142))
my_library.add(Movie('The Godfather', 1972, 'Francis Ford Coppola', 175))
my_library.add(Movie('Pulp Fiction', 1994, 'Quentin Tarantino', 154))

print(my_library)
```

### TV Series Tracker

```python
# Track series you're watching
watchlist = MediaCatalogue()

watchlist.add(TVSeries('Game of Thrones', 2011, 'D.B. Weiss', 57, 8, 73))
watchlist.add(TVSeries('Stranger Things', 2016, 'The Duffer Brothers', 51, 4, 34))
watchlist.add(TVSeries('The Crown', 2016, 'Peter Morgan', 58, 6, 60))

print(f'Currently tracking {len(watchlist.get_tv_series())} series')
```

### Mixed Media Collection

```python
# Combined collection
entertainment = MediaCatalogue()

# Movies
entertainment.add(Movie('Avatar', 2009, 'James Cameron', 162))
entertainment.add(Movie('Titanic', 1997, 'James Cameron', 195))

# TV Series
entertainment.add(TVSeries('Friends', 1994, 'David Crane', 22, 10, 236))
entertainment.add(TVSeries('The Sopranos', 1999, 'David Chase', 55, 6, 86))

print(entertainment)
```

---

## Design Decisions

### Why Inherit TVSeries from Movie?

TV series share many attributes with movies:
- Both have titles, years, directors, and durations
- Validation rules for common fields are identical
- Reduces code duplication through inheritance

### Why Custom MediaError?

Standard exceptions don't store the problematic object:
- `MediaError` stores the object for debugging
- Provides context about what failed
- Enables better error messages

### Why Separate get_movies() and get_tv_series()?

Different filtering approaches:
- `get_movies()`: Uses `type()` to exclude subclasses
- `get_tv_series()`: Uses `isinstance()` to include TVSeries
- Allows precise control over what's returned

### Why Validate in Constructor?

Early validation prevents invalid objects:
- Catches errors immediately
- Ensures all instances are valid
- Follows "fail fast" principle

---

## Error Messages Reference

| Error | Message | Cause |
|-------|---------|-------|
| Empty title | "Title cannot be empty" | Title is empty or only whitespace |
| Invalid year | "Year must be 1895 or later" | Year is before 1895 |
| Empty director | "Director cannot be empty" | Director is empty or only whitespace |
| Invalid duration | "Duration must be positive" | Duration is 0 or negative |
| Invalid seasons | "Seasons must be 1 or greater" | Seasons is 0 or negative |
| Invalid episodes | "Total episodes must be 1 or greater" | Total episodes is 0 or negative |
| Wrong type | "Only Movie or TVSeries instances can be added" | Trying to add non-media object |

---

## Installation & Running

### Prerequisites
- Python 3.6 or higher
- No external dependencies

### Running the Program

1. **Save the code** to `media_catalogue.py`

2. **Run the example**:
```bash
python media_catalogue.py
```

3. **Expected output**:
```
Media Catalogue (4 items):

=== MOVIES ===
1. The Matrix (1999) - 136 min, The Wachowskis
2. Inception (2010) - 148 min, Christopher Nolan

=== TV SERIES ===
1. Scrubs (2001) - 9 seasons, 182 episodes, 24 min avg, Bill Lawrence
2. Breaking Bad (2008) - 5 seasons, 62 episodes, 47 min avg, Vince Gilligan
```

---

## Project Structure

```
.
├── README.md
└── media_catalogue.py
    ├── MediaError (Exception)
    ├── Movie (Base class)
    ├── TVSeries (Inherits from Movie)
    └── MediaCatalogue (Container)
```

---

## Future Enhancements

Potential improvements:

- [ ] Add genre/category fields
- [ ] Implement rating system (1-5 stars)
- [ ] Add search functionality (by title, director, year)
- [ ] Sort catalogue by various criteria
- [ ] Export catalogue to JSON/CSV
- [ ] Add watch status (watched/unwatched)
- [ ] Implement movie/series reviews
- [ ] Add cast information
- [ ] Create web interface
- [ ] Database persistence
- [ ] Import from IMDB/TMDB API
- [ ] Add streaming platform info

---

## Best Practices Demonstrated

- ✅ **Inheritance**: Reuse code through class hierarchy
- ✅ **Custom Exceptions**: Domain-specific error handling
- ✅ **Input Validation**: Comprehensive data validation
- ✅ **Type Checking**: Proper use of isinstance() and type()
- ✅ **String Representation**: Clear __str__() implementations
- ✅ **Single Responsibility**: Each class has one clear purpose
- ✅ **Defensive Programming**: Validate all inputs
- ✅ **Clean Code**: Readable, well-documented code

---

## Learning Outcomes

This project teaches:

- 📚 Object-oriented programming principles
- 🔗 Class inheritance and super() usage
- 🛡️ Custom exception creation and handling
- ✅ Input validation techniques
- 🎯 Polymorphism and type checking
- 📝 String formatting and representation
- 🏗️ Software design patterns
- 🧪 Error handling strategies

---

## Contributing

Contributions welcome! Ideas:

1. Add unit tests with pytest
2. Implement search/filter methods
3. Add data persistence layer
4. Create rating system
5. Build CLI interface
6. Add genre classification
7. Implement data export
8. Create API integration

---

## License

This project is provided for educational purposes. No license restrictions are applied.

---

## Author

Created as a Python programming exercise demonstrating object-oriented design, inheritance, and exception handling.

---

## Acknowledgments

Built to demonstrate:
- Class hierarchy design
- Custom exception handling
- Input validation strategies
- Object-oriented principles
- Type checking in Python

---

## FAQ

### Q: Why is 1895 the minimum year?

A: 1895 is considered the birth year of cinema, when the Lumière brothers held the first public film screening.

### Q: Can I modify items after adding them?

A: Yes, you can modify the objects directly since Python passes by reference.

### Q: What's the difference between get_movies() and get_tv_series()?

A: `get_movies()` returns only Movie instances (not TVSeries), while `get_tv_series()` returns TVSeries instances.

### Q: Can I add the same movie twice?

A: Yes, the current implementation allows duplicates. You'd need to add duplicate checking if desired.

### Q: Why store the object in MediaError?

A: It helps with debugging by showing exactly what object caused the error.

---

## Support

For questions or issues:
- Review the code examples
- Check the validation rules
- Examine the test cases
- Open an issue on the repository

---

## Version History

### Version 1.0.0
- Initial release
- Movie class with validation
- TVSeries class with inheritance
- MediaCatalogue container
- Custom MediaError exception
- Type filtering methods

---

## Real-World Applications

This system can be adapted for:

- 🎬 Personal movie collections
- 📺 TV show tracking
- 🏢 Media library management
- 🎓 Film study databases
- 📊 Entertainment analytics
- 🎯 Watchlist applications
- 💼 Content management systems
- 📱 Entertainment apps
