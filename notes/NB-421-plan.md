# 🗺️ NB-421 · Plan

## Steps

1. Upload goes straight to file storage (presigned URL)
2. `Catalog::ReadFileJob` reads the file and writes `import_drafts`
3. Review grid groups drafts into New / Update / Fix / Decide
4. Import writes the approved rows in one transaction

## Files

| File | Change |
|---|---|
| `app/services/catalog/file_reader.rb` | new |
| `app/jobs/catalog/read_file_job.rb` | new |
| `app/models/import_draft.rb` | new |
| `app/views/admin/catalog/imports/new.html.erb` | card next to CSV import |

## Estimate

About 6 days, review grid included.
