
# Practicing Redis with Laravel

This project is a learning exercise demonstrating Redis functionalities within a Laravel application. The articles and videos used here are fictional and serve purely as a means to understand and practice Redis integration.

## Project Structure

Key functionalities covered in this project include:

- **Caching articles** to reduce database queries.
- **Tracking video downloads** using Redis counters.
- **Managing trending articles** with Redis sorted sets.
- **Tracking recently viewed articles** for users using Redis sorted sets.

## Prerequisites

Ensure you have the following installed:

- **PHP** (>= 7.4 or your preferred version)
- **Redis** (for caching and data storage)
- **Laravel** (PHP framework)

## Getting Started

1. Clone the repository:

   ```bash
   git clone https://github.com/Youssef11khaled99/Practicing-Redis-with-Laravel.git
   ```

2. Navigate to the project directory:

   ```bash
   cd Practicing-Redis-with-Laravel
   ```

3. Install dependencies with Composer:

   ```bash
   composer install
   ```

4. Configure Redis in your `.env` file:

   ```plaintext
   CACHE_DRIVER=redis
   REDIS_HOST=127.0.0.1
   REDIS_PORT=6379
   ```

5. Run the application:

   ```bash
   php artisan serve
   ```

## Routes and Functionalities

### 1. Video Downloads Tracking

- **`GET /videos/{id}`**: Retrieves the download count for a specific video from Redis and displays it.
- **`GET /videos/{id}/download`**: Increments the download count in Redis each time the video is downloaded.

### 2. Trending Articles

- **`GET /articles/trending`**: Fetches trending articles based on a sorted set in Redis. Article IDs with higher scores appear first.
- **`GET /articles/{article}`**: Increments the trending score for the article in Redis when it’s viewed, helping identify popular articles.

### 3. Recently Viewed Articles

- **`GET /articles/{article}`**: Adds the article to a sorted set of recently viewed articles for a specific user. Articles are sorted by the time they were viewed.
- **`GET /articles/`**: Displays all articles, recent views, and trending articles by retrieving data from Redis and the database.

### Caching Articles

The `cacheAbleArticles` method in the `Article` model caches the results of frequently accessed articles, reducing database load and improving response times.

## Example Code Snippets

### Incrementing a Counter with Redis

```php
Route::get('videos/{id}/download', function ($id) {
    Redis::incr("videos.{$id}.downloads");
    return back();
});
```

### Working with Sorted Sets for Trending Articles

```php
Route::get('articles/{article}', function (Article $article) {
    Redis::zincrby('trending_articles', 1, $article->id);
    return $article;
});
```
