# Astro Starter Kit: Basics

```sh
npm create astro@latest -- --template basics
```

> 🧑‍🚀 **Seasoned astronaut?** Delete this file. Have fun!

## 🚀 Project Structure

Inside of your Astro project, you'll see the following folders and files:

```text
/
├── public/
│   └── favicon.svg
├── src
│   ├── assets
│   │   └── astro.svg
│   ├── components
│   │   └── Welcome.astro
│   ├── layouts
│   │   └── Layout.astro
│   └── pages
│       └── index.astro
└── package.json
```

To learn more about the folder structure of an Astro project, refer to [our guide on project structure](https://docs.astro.build/en/basics/project-structure/).

## 🧞 Commands

All commands are run from the root of the project, from a terminal:

| Command                   | Action                                           |
| :------------------------ | :----------------------------------------------- |
| `npm install`             | Installs dependencies                            |
| `npm run dev`             | Starts local dev server at `localhost:4321`      |
| `npm run build`           | Build your production site to `./dist/`          |
| `npm run preview`         | Preview your build locally, before deploying     |
| `npm run astro ...`       | Run CLI commands like `astro add`, `astro check` |
| `npm run astro -- --help` | Get help using the Astro CLI                     |

## 💌 Postcards setup

The postcard wall on the home page stores messages in Supabase. Until it is
configured the section shows a "not connected yet" notice instead of the form —
the rest of the site works either way.

**1. Create the table.** In your Supabase project, open the SQL Editor and run:

```sql
create table postcards (
  id         uuid primary key default gen_random_uuid(),
  name       text,
  message    text not null,
  created_at timestamptz not null default now()
);

alter table postcards enable row level security;

-- Anyone can read the wall.
create policy "postcards are public" on postcards
  for select using (true);

-- Anyone can send one, within these limits. `name` is null = anonymous.
create policy "anyone can send a postcard" on postcards
  for insert with check (
    char_length(message) between 1 and 500
    and (name is null or char_length(name) between 1 and 40)
  );
```

**2. Add your keys locally.** Copy `.env.example` to `.env` and fill in the
Project URL and anon key from *Project Settings → API*. `.env` is gitignored.

```sh
cp .env.example .env
```

**3. Add the same keys to GitHub.** In the repo, go to *Settings → Secrets and
variables → Actions* and add two repository secrets with the same names:
`PUBLIC_SUPABASE_URL` and `PUBLIC_SUPABASE_ANON_KEY`. The deploy workflow reads
them at build time.

Both values are meant to be visible in the browser — the anon key only permits
what the policies above allow. To remove a postcard, delete its row from the
Table Editor.

## 👀 Want to learn more?

Feel free to check [our documentation](https://docs.astro.build) or jump into our [Discord server](https://astro.build/chat).
