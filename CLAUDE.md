# SDG Platform — Montenegro (montestat/site)

This is the Jekyll site layer for Montenegro's Open SDG platform. It consumes built data from `montestat/data` and renders the public-facing website.

## Repository Layout

```
site/
  _config.yml          ← staging config (used on develop branch)
  _config_prod.yml     ← production overrides (environment: production)
  _data/
    site_config.yml    ← site-level settings (languages, goals, navigation)
    site_config_prod.yml
  _includes/           ← custom template overrides
  _layouts/            ← custom layout overrides
  _pages/              ← static pages (about, faq, etc.)
  _posts/              ← news/blog posts
  _sass/               ← custom CSS overrides
  assets/              ← images, fonts, JS
```

## Branches

- `develop` — staging environment
- `master` — production; a push here triggers the deployment pipeline
- `gh-pages` — built output (managed by CI, do not edit manually)

## How to Trigger a Production Deployment

After the data repo (`montestat/data`) has been updated and its build passes, trigger a site deployment by making a cosmetic change and pushing master:

1. Edit `_config.yml` — update the date comment on line 2:
   ```yaml
   # Last data refresh: YYYY-MM-DD
   ```
2. Commit, push develop, merge to master, push master:
   ```
   git add _config.yml
   git commit -m "refresh prod YYYY-MM-DD"
   git push https://nikolabetf@github.com/montestat/site.git develop
   git checkout master && git merge develop
   git push https://nikolabetf@github.com/montestat/site.git master
   git checkout develop
   ```

The master push triggers the GitHub Actions workflow that builds and deploys to production.

## Git Push Auth

Windows Credential Manager caches the wrong account (`nikolabildstudio`). Always embed the correct username in the remote URL:
```
https://nikolabetf@github.com/montestat/site.git
```

## Site Config Key Settings (`_data/site_config.yml`)

Common fields you may need to update:
- `languages` — list of active languages (`en`, `cnr`)
- `goals_page` — layout of the goals overview
- `data_edit_url` — link to edit data on GitHub
- `progress_status` — whether to show progress bars per indicator

## Adding/Editing Content

- **Static pages**: add/edit files in `_pages/`
- **News posts**: add files in `_posts/` following the `YYYY-MM-DD-title.md` naming convention
- **Navigation**: edit `_data/site_config.yml` under `menu`
- **Translations** (UI strings): these live in the data repo at `translations/cnr/` and `translations/en/`

## Theme Version

```yaml
remote_theme: open-sdg/open-sdg@2.4.0
```

To upgrade: change the version tag in `_config.yml` and test on `develop` before merging to `master`.
