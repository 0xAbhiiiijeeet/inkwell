# Inkwell

A blogging app where users sign up, write posts with a cover image and topics, and read what others have published. Previously loaded blogs stay available offline.

## Features

- Sign up, log in and stay logged in
- Write a blog with a title, content, cover image and one or more topics
- Browse all blogs, each card showing its estimated reading time
- Open a blog to read it in full, with its author, topics, date and reading time
- Offline reading: blogs are cached on the device and shown when there is no internet
- Uploading a blog needs an internet connection

## Tech Stack

| Part | Technologies |
| --- | --- |
| App | Flutter, Bloc, get_it |
| Backend | Supabase (authentication, database, storage) |
| Local storage | Hive |
| Error handling | fpdart (`Either`) |

## Architecture

The app follows clean architecture, with each feature split into three layers:

```
lib/
  core/            Shared code: theme, errors, network check, use case base, utils
  features/
    auth/
      data/          Remote data source, models, repository implementation
      domain/        Repository interface, use cases (sign up, log in, current user)
      presentation/  Bloc, pages, widgets
    blog/
      data/          Remote and local data sources, models, repository implementation
      domain/        Entities, repository interface, use cases (upload blog, get all blogs)
      presentation/  Bloc, pages, widgets
  init_dependencies.dart   get_it setup
```

- Repositories return `Either<Failure, T>` instead of throwing exceptions.
- Blogs are fetched from Supabase and saved to a Hive box. When the connection checker reports no internet, the app reads from Hive instead.

## Supabase Setup

Create a Supabase project and add:

- A `profiles` table with `id` and `name` columns, linked to the signed-up user
- A `blogs` table with `id`, `poster_id`, `title`, `content`, `image_url`, `topics` and `updated_at` columns
- A storage bucket named `blog_images`

Then put your project URL and anon key in `lib/core/secrets/app_secrets.dart`.

## Getting Started

```bash
flutter pub get
flutter run
```
