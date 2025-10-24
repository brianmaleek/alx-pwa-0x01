# MoviesDatabase API Documentation

## API Overview:

The MoviesDatabase API provides comprehensive and regularly updated data for over **9 million titles**, including movies, series, and episodes. 

- **Recent titles**: Updated weekly
- **Ratings & episodes**: Updated daily
- **Developer**: SAdrian
- **Support**: [Buy Me A Coffee](https://www.buymeacoffee.com/SAdrian13)
- **DatabaseLink**: [MoviesDatabase API Documentation](https://rapidapi.com/SAdrian/api/moviesdatabase)
---

## API Version:

Version: 1.0 (as per RapidAPI documentation)

## Table of Contents:

- [Available Endpoints](#available-endpoints)
- [Request and Response Format](#request-and-response-format)
- [Query Parameters](#query-parameters)
- [Response Models](#response-models)
- [Info Parameter Options](#info-parameter-options)
- [Predefined Lists](#predefined-lists)
- [Authentication](#authentication)
- [Usage Examples](#usage-examples)
- [Error Handling](#error-handling)
- [Usage Limits & Best Practices](#usage-limits-and-best-practices)

---

## Available Endpoints:

### Titles Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/titles` | Returns an array of titles based on filter and sorting parameters |
| GET | `/titles/{id}` | Returns a specific title by its IMDb ID |
| GET | `/titles/{id}/ratings` | Returns rating information and vote count for a specific title |
| GET | `/x/titles-by-ids` | Returns an array of titles based on a list of IMDb IDs |
| GET | `/titles/x/upcoming` | Returns an array of upcoming titles based on filters |

### Series & Episodes Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/titles/series/{id}` | Returns all episodes for a series in ascending order |
| GET | `/titles/seasons/{id}` | Returns the number of seasons for a series |
| GET | `/titles/series/{id}/{season}` | Returns episodes for a specific season |
| GET | `/titles/episode/{id}` | Returns details for a specific episode |

### Search Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/titles/search/keyword/{keyword}` | Search titles by keyword |
| GET | `/titles/search/title/{title}` | Search titles by title (exact or partial match) |
| GET | `/titles/search/akas/{aka}` | Search titles by alternate titles (exact match, case-sensitive) |

### Actors Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/actors` | Returns an array of actors based on filters |
| GET | `/actors/{id}` | Returns details for a specific actor by IMDb ID |

### Utility Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/title/utils/titleType` | Returns available title types |
| GET | `/title/utils/genres` | Returns available genres |
| GET | `/title/utils/lists` | Returns available predefined lists |

---

## Request and Response Format:

### Request Structure

All endpoints accept **optional** query parameters for filtering, sorting, and pagination.

> **Note**: Every endpoint returns an object with a `results` key. Endpoints with pagination include additional keys: `page`, `next`, `entries`.

---

## Query Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `info` | string | `mini_info` | Specifies which information fields to return |
| `limit` | number | `10` | Number of results per page (max 50) |
| `page` | number | `1` | Page number for pagination |
| `titleType` | string | - | Filter by title type |
| `year` | number | - | Filter by specific release year |
| `startYear` | number | - | Start of year range filter |
| `endYear` | number | - | End of year range filter |
| `genre` | string | - | Filter by genre (case-sensitive, capitalized) |
| `sort` | string | - | Sort results: `year.incr` or `year.decr` |
| `list` | string | `titles` | Predefined collection list |
| `exact` | boolean | `false` | Exact match for title searches |
| `idsList` | array | - | Array of IMDb IDs (for `/x/titles-by-ids` endpoint) |

### Response Structure

```json
{
  "results": [...],
  "page": 1,
  "next": "/titles?page=2",
  "entries": 12
}
```

---

## Response Models

### Title Model (mini_info)

```json
{
  "id": "tt0000001",
  "titleText": { "text": "Movie Title" },
  "primaryImage": { "url": "..." },
  "titleType": { "text": "movie" },
  "releaseDate": { "year": 2024 }
}
```

### Rating Model

| Field | Type | Description |
|-------|------|-------------|
| `tconst` | string | IMDb title ID |
| `averageRating` | number | Average rating score |
| `numVotes` | number | Total number of votes |

```json
{
  "tconst": "tt0000003",
  "averageRating": 6.5,
  "numVotes": 1631
}
```

### Light Episode Model

| Field | Type | Description |
|-------|------|-------------|
| `tconst` | string | IMDb episode ID |
| `parentTconst` | string | IMDb series ID |
| `seasonNumber` | number | Season number |
| `episodeNumber` | number | Episode number |

```json
{
  "tconst": "tt0020666",
  "parentTconst": "tt15180956",
  "seasonNumber": 1,
  "episodeNumber": 2
}
```

### Actor Model

| Field | Type | Description |
|-------|------|-------------|
| `nconst` | string | IMDb actor ID |
| `primaryName` | string | Actor's name |
| `birthYear` | number | Birth year |
| `deathYear` | number | Death year (if applicable) |
| `primaryProfession` | string | Comma-separated professions |
| `knownForTitles` | string | Comma-separated IMDb title IDs |

```json
{
  "nconst": "nm0000001",
  "primaryName": "Fred Astaire",
  "birthYear": 1899,
  "deathYear": 1987,
  "primaryProfession": "soundtrack,actor,miscellaneous",
  "knownForTitles": "tt0050419,tt0053137,tt0072308,tt0031983"
}
```

---

## Info Parameter Options

The `info` parameter accepts predefined options or **any key** from the title object:

| Option | Returns |
|--------|---------|
| `base_info` | titleText, id, primaryImage, titleType, releaseDate, genres, runtime, plot, meterRanking, ratingsSummary, episodes, series |
| `mini_info` | titleText, id, primaryImage, titleType, releaseDate |
| `image` | id, primaryImage |
| `creators_directors_writers` | id, creators, directors, writers |
| `revenue_budget` | id, productionBudget, lifetimeGross, openingWeekendGross, worldwideGross |
| `extendedCast` | id, cast |
| `rating` | id, ratingsSummary |
| `awards` | id, wins, nominations, prestigiousAwardSummary |

---

## Predefined Lists

| List Name | Description |
|-----------|-------------|
| `most_pop_movies` | Top 10,000 most popular movies |
| `most_pop_series` | Top 10,000 most popular series |
| `top_boxoffice_200` | Top 200 all-time box office movies |
| `top_boxoffice_last_weekend_10` | Top 10 box office from last weekend |
| `top_rated_250` | Top 250 movies by rating |
| `top_rated_english_250` | Top 250 English movies by rating |
| `top_rated_lowest_100` | Top 100 lowest rated movies |
| `top_rated_series_250` | Top 250 series by rating |
| `titles` | Entire collection (default, 9+ million titles) |

> **Note**: Titles in lists (except `titles`) include a `position` field indicating their rank.

---

## Authentication:

The API uses **RapidAPI** authentication with API key-based access.

### Required Headers

```http
x-rapidapi-host: moviesdatabase.p.rapidapi.com
x-rapidapi-key: YOUR_API_KEY
```

### Obtaining an API Key

1. Sign up at [RapidAPI](https://rapidapi.com)
2. Subscribe to the MoviesDatabase API
3. Copy your API key from the dashboard
4. Store the key securely in environment variables

⚠️ **Never commit API keys to version control**

---

## Usage Examples

### Example 1: Fetch Top Rated Movies

```javascript
const fetchTopRatedMovies = async () => {
  const url = 'https://moviesdatabase.p.rapidapi.com/titles';
  const options = {
    method: 'GET',
    headers: {
      'x-rapidapi-host': 'moviesdatabase.p.rapidapi.com',
      'x-rapidapi-key': process.env.RAPIDAPI_KEY
    }
  };

  const params = new URLSearchParams({
    list: 'top_rated_250',
    limit: '10',
    info: 'base_info'
  });

  try {
    const response = await fetch(`${url}?${params}`, options);
    const data = await response.json();
    console.log(data.results);
  } catch (error) {
    console.error('Error fetching top rated movies:', error);
  }
};
```

### Example 2: Search Movies by Title

```javascript
const searchMovieByTitle = async (title) => {
  const url = `https://moviesdatabase.p.rapidapi.com/titles/search/title/${encodeURIComponent(title)}`;
  const options = {
    method: 'GET',
    headers: {
      'x-rapidapi-host': 'moviesdatabase.p.rapidapi.com',
      'x-rapidapi-key': process.env.RAPIDAPI_KEY
    }
  };

  const params = new URLSearchParams({
    exact: 'false',
    titleType: 'movie',
    limit: '5'
  });

  try {
    const response = await fetch(`${url}?${params}`, options);
    const data = await response.json();
    return data.results;
  } catch (error) {
    console.error('Error searching movie:', error);
  }
};

// Usage
searchMovieByTitle('Inception');
```

### Example 3: Get Movie Details by ID

```javascript
const getMovieDetails = async (movieId) => {
  const url = `https://moviesdatabase.p.rapidapi.com/titles/${movieId}`;
  const options = {
    method: 'GET',
    headers: {
      'x-rapidapi-host': 'moviesdatabase.p.rapidapi.com',
      'x-rapidapi-key': process.env.RAPIDAPI_KEY
    }
  };

  const params = new URLSearchParams({
    info: 'base_info'
  });

  try {
    const response = await fetch(`${url}?${params}`, options);
    const data = await response.json();
    return data.results;
  } catch (error) {
    console.error('Error fetching movie details:', error);
  }
};

// Usage
getMovieDetails('tt1375666'); // Inception
```

### Example 4: Get Movie Ratings

```javascript
const getMovieRatings = async (movieId) => {
  const url = `https://moviesdatabase.p.rapidapi.com/titles/${movieId}/ratings`;
  const options = {
    method: 'GET',
    headers: {
      'x-rapidapi-host': 'moviesdatabase.p.rapidapi.com',
      'x-rapidapi-key': process.env.RAPIDAPI_KEY
    }
  };

  try {
    const response = await fetch(url, options);
    const data = await response.json();
    console.log(`Rating: ${data.results.averageRating}/10`);
    console.log(`Votes: ${data.results.numVotes}`);
    return data.results;
  } catch (error) {
    console.error('Error fetching ratings:', error);
  }
};
```

### Example 5: Filter Movies by Genre and Year

```javascript
const filterMovies = async () => {
  const url = 'https://moviesdatabase.p.rapidapi.com/titles';
  const options = {
    method: 'GET',
    headers: {
      'x-rapidapi-host': 'moviesdatabase.p.rapidapi.com',
      'x-rapidapi-key': process.env.RAPIDAPI_KEY
    }
  };

  const params = new URLSearchParams({
    genre: 'Action',
    year: '2023',
    titleType: 'movie',
    limit: '20',
    sort: 'year.decr'
  });

  try {
    const response = await fetch(`${url}?${params}`, options);
    const data = await response.json();
    return data.results;
  } catch (error) {
    console.error('Error filtering movies:', error);
  }
};
```

### Example 6: Get Series Episodes

```javascript
const getSeriesEpisodes = async (seriesId) => {
  const url = `https://moviesdatabase.p.rapidapi.com/titles/series/${seriesId}`;
  const options = {
    method: 'GET',
    headers: {
      'x-rapidapi-host': 'moviesdatabase.p.rapidapi.com',
      'x-rapidapi-key': process.env.RAPIDAPI_KEY
    }
  };

  try {
    const response = await fetch(url, options);
    const data = await response.json();
    console.log(`Total episodes: ${data.results.length}`);
    return data.results;
  } catch (error) {
    console.error('Error fetching episodes:', error);
  }
};
```

### Example 7: Get Multiple Titles by IDs

```javascript
const getTitlesByIds = async (ids) => {
  const url = 'https://moviesdatabase.p.rapidapi.com/x/titles-by-ids';
  const options = {
    method: 'GET',
    headers: {
      'x-rapidapi-host': 'moviesdatabase.p.rapidapi.com',
      'x-rapidapi-key': process.env.RAPIDAPI_KEY
    }
  };

  const params = new URLSearchParams({
    idsList: JSON.stringify(ids),
    info: 'mini_info'
  });

  try {
    const response = await fetch(`${url}?${params}`, options);
    const data = await response.json();
    return data.results;
  } catch (error) {
    console.error('Error fetching titles:', error);
  }
};

// Usage
getTitlesByIds(['tt1375666', 'tt0468569', 'tt0816692']);
```

### Example 8: React Component Example

```jsx
import React, { useState, useEffect } from 'react';

const MovieList = () => {
  const [movies, setMovies] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchMovies = async () => {
      const url = 'https://moviesdatabase.p.rapidapi.com/titles';
      const options = {
        method: 'GET',
        headers: {
          'x-rapidapi-host': 'moviesdatabase.p.rapidapi.com',
          'x-rapidapi-key': process.env.REACT_APP_RAPIDAPI_KEY
        }
      };

      const params = new URLSearchParams({
        list: 'top_rated_250',
        limit: '10',
        info: 'base_info'
      });

      try {
        const response = await fetch(`${url}?${params}`, options);
        const data = await response.json();
        setMovies(data.results);
      } catch (error) {
        console.error('Error:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchMovies();
  }, []);

  if (loading) return <div>Loading...</div>;

  return (
    <div>
      <h1>Top Rated Movies</h1>
      {movies.map(movie => (
        <div key={movie.id}>
          <h2>{movie.titleText.text}</h2>
          <img src={movie.primaryImage?.url} alt={movie.titleText.text} />
          <p>Year: {movie.releaseDate?.year}</p>
        </div>
      ))}
    </div>
  );
};

export default MovieList;
```

## Error Handling:

### HTTP Status Codes

| Status Code | Description |
|-------------|-------------|
| `200` | OK - Request successful |
| `400` | Bad Request - Invalid parameters provided |
| `401` | Unauthorized - Invalid or missing API key |
| `404` | Not Found - Resource not found |
| `429` | Too Many Requests - Rate limit exceeded |
| `500` | Internal Server Error - Server-side error |

### Error Response Format

```json
{
  "message": "Error description",
  "statusCode": 400
}
```

### Example Error Handling

```typescript
try {
  const response = await fetch(apiUrl, { headers });
  
  if (!response.ok) {
    throw new Error(`API Error: ${response.status}`);
  }
  
  const data = await response.json();
  return data.results;
} catch (error) {
  console.error('Failed to fetch data:', error);
  // Implement fallback or user notification
}
```

---

## Usage Limits and Best Practices:

### Pagination

- ✅ Use `limit` parameter (max 50) to control response size
- ✅ Implement pagination for large datasets
- ✅ Cache results when appropriate

### Query Optimization

- ✅ Use specific `info` parameters to request only needed data
- ✅ Combine filters to reduce result sets
- ✅ Use predefined lists for common queries

### Performance

- ✅ Implement client-side caching to reduce API calls
- ✅ Use server-side API routes to protect API keys
- ✅ Batch requests using the `/x/titles-by-ids` endpoint

### Data Freshness

| Data Type | Update Frequency |
|-----------|------------------|
| Recent titles | Weekly |
| Ratings & episodes | Daily |

### Security

- 🔒 Never expose API keys in client-side code
- 🔒 Use environment variables for API credentials
- 🔒 Implement server-side proxy routes for API calls

### Rate Limits

- Monitor response headers for rate limit information
- Implement exponential backoff for rate limit errors
- Rate limits vary by subscription tier

---

## Support

💖 Support the developer at [Buy Me A Coffee](https://www.buymeacoffee.com/SAdrian13)

---

## License

This documentation is for the MoviesDatabase API available on RapidAPI.