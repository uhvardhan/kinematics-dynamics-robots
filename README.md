# Kinematics and Dynamics of Robots — course website

Jekyll site for **25PC1RA204 Kinematics and Dynamics of Robots**, B.Tech. IV Semester, CSE
(Robotics & Artificial Intelligence), VNR VJIET. It is built from the Sensors and Actuators Laboratory site — same
layout and type, its own colours — and deploys on GitHub Pages with no build configuration.

Live at `https://uhvardhan.github.io/kinematics-dynamics-robots/`

---

## Deploy

1. Create a **new public repository** named `kinematics-dynamics-robots` under your account.
2. Push these files to the `main` branch, or drag them into the repository's
   **Add file → Upload files** page.
3. Repository → **Settings** → **Pages** → Source: **Deploy from a branch** → `main` / `/ (root)`.
4. Wait for the first build (about a minute), then open the URL above.

If you name the repository something else, change `baseurl` in `_config.yml` to match —
it must be `/your-repo-name`, with a leading slash and no trailing slash. Every link on
the site is built from it, so a mismatch breaks navigation and CSS.

## Run it locally

Optional — only needed if you want to preview before pushing.

```bash
gem install bundler
bundle install
bundle exec jekyll serve --livereload
# http://127.0.0.1:4000/kinematics-dynamics-robots/
```

## Where things live

| I want to change… | Edit |
| --- | --- |
| Course code, scheme, timings, venue, assessment, instructors | `_data/course.yml` |
| The course description on the home page | `index.md` |
| The session-by-session schedule, class dates and files | `_data/schedule.yml` |
| The key under the schedule | `_data/schedule_key.yml` |
| Announcements on the home page | `_data/updates.yml` |
| The nav bar | `_data/nav.yml` |
| Institute name, logo, semester label, `baseurl` | `_config.yml` |
| Colours, type, spacing | `assets/css/style.css` |

In normal use you edit YAML and `index.md`, not markup.

## Before you go live

- [ ] Search the project for `PLACEHOLDER` and fill in each one — timings, venue,
      prerequisites and instructors in `_data/course.yml`.
- [ ] Read over the course description in `index.md`, drafted from the R25 syllabus.
- [ ] Once the timetable is out, give each session its `date` in `_data/schedule.yml`.
      In any week where a session has a date, the Date column shows each session's
      date instead of the week's span.
- [ ] Set the date on the welcome entry in `_data/updates.yml` to the day the site goes live.

## Theory or lab

`teaching_scheme` in `_data/course.yml` takes `lecture`, `tutorial` and `practical` hours
per week. The home page lists only the non-zero ones, so the same files serve a theory
course or a laboratory. The heading of the schedule's Topic column is in `schedule.md`.

## The schedule

`_data/schedule.yml` was generated from the Teaching Plan 2026–27: one entry per week,
each with its sessions (C01–C45, the "No class" rows for Sessional Examination I, and
the Week 17 review and buffer row). Each session is a row on the Schedule page, with the
columns Week, Date, Session, Unit, Topic, Lecture Notes, Reading and Additional
Resources. The Week and Date cells span that week's rows. On phones the table turns
into a list, one block per session.

## Adding lecture notes or other files

Drop the file in `assets/files/`, then list it under the session's `notes` in
`_data/schedule.yml`:

```yaml
    - id: "C01"
      ...
      notes:
        - name: "Lecture notes (PDF)"
          url: "/assets/files/C01%20-%20Robots%20as%20mechanisms.pdf"
```

`notes` fills the Lecture Notes column and `resources` fills Additional Resources.
Write a space in a file name as `%20` and an `&` as `%26`.

## Adding an update

Newest first in `_data/updates.yml`:

```yaml
- date: "12 Jan"
  body: >-
    Unit I notes are on the [Schedule](/schedule/) page.
```

Write internal links starting with `/` — the layout rewrites them to include `baseurl`.

## Structure

```
_config.yml           site config, institute block
_data/                course.yml, schedule.yml, schedule_key.yml, updates.yml, nav.yml
_layouts/             default, page, home
_includes/            head, masthead, nav, footer
assets/css/style.css  all styling, no framework
assets/images/        logo, favicon, instructor photos
assets/files/         lecture notes, slides, scripts
```

## Notes on the design

The skeleton is the lab site's: IBM Plex throughout (Condensed for headings, Sans for body,
Mono for codes, dates and labels), the Updates panel at the top of the home page, and a thin
strip under the masthead. What changes is the colour, and it still carries meaning.
Blueprint blue marks kinematics, the geometry of motion; brick red marks dynamics, the forces
and torques behind it. The strip runs blue to red, from describing motion to explaining it.
Links and the Updates panel take the blue. The neutrals are cooled to match, and every text
colour clears 4.5:1 contrast on every background it sits on.
