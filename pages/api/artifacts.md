# Artifacts API

<%= toc %>

## Artifact data model

An artifact is a file uploaded by your agent during the execution of a build’s job. The contents of the artifact can be retrieved using the `download_url` and the [artifact download API](#download-an-artifact).

<table>
<tbody>
  <tr><th><code>id</code></th><td>ID of the artifact</td></tr>
  <tr><th><code>job_id</code></th><td>ID of the job</td></tr>
  <tr><th><code>url</code></th><td>Canonical API URL of the artifact</td></tr>
  <tr><th><code>download_url</code></th><td>Artifact Download API URL for the artifact</td></tr>
  <tr><th><code>state</code></th><td>State of the artifact (<%= Artifact::State::ALL.map { |x| "<code>#{x}</code>" }.join(", ") %>)</td></tr>
  <tr><th><code>path</code></th><td>Path of the artifact</td></tr>
  <tr><th><code>dirname</code></th><td>Path of the artifact excluding the filename</td></tr>
  <tr><th><code>filename</code></th><td>Filename of the artifact</td></tr>
  <tr><th><code>mime_type</code></th><td>Mime type of the artifact</td></tr>
  <tr><th><code>file_size</code></th><td>File size of the artifact in bytes</td></tr>
  <tr><th><code>glob_path</code></th><td>Original glob path used to upload the artifact</td></tr>
  <tr><th><code>original_path</code></th><td>Original full file path of the artifact</td></tr>
  <tr><th><code>sha1sum</code></th><td>SHA-1 hash of artifact contents as calculated by the agent</td></tr>
</tbody>
</table>

## List artifacts for a build

Returns a [paginated list](<%= paginated_resource_docs_url %>) of a build’s artifacts across all of its jobs.

```bash
curl "https://api.buildkite.com/v2/organizations/{org.slug}/pipelines/{pipeline.slug}/builds/{build.number}/artifacts"
```

```json
[
  {
    "id": "76365070-34d5-4104-8b91-952780f8029f",
    "job_id": "aae578fe-994c-44e6-84da-4102616928ba",
    "url": "https://api.buildkite.com/v2/organizations/my-great-org/pipelines/my-pipeline/builds/1/jobs/aae578fe-994c-44e6-84da-4102616928ba/artifacts/76365070-34d5-4104-8b91-952780f8029f",
    "download_url": "https://api.buildkite.com/v2/organizations/my-great-org/pipelines/my-pipeline/builds/1/jobs/aae578fe-994c-44e6-84da-4102616928ba/artifacts/76365070-34d5-4104-8b91-952780f8029f/download",
    "state": "finished",
    "path": "dist/app.tar.gz",
    "dirname": "dist",
    "filename": "app.tar.gz",
    "mime_type": "application/x-gzip",
    "file_size": 529371,
    "glob_path": "dist/*",
    "original_path": "/var/lib/buildkite-agent/builds/bk-agent-1/my-great-org/my-pipeline/dist/app.tar.gz",
    "sha1sum": "884c4ad3f2545c85c69d0d0ef50c5d4f5266f0b7"
  },
  {
    "id": "89f4ce5c-6e1d-482c-9ca6-88c050291c77",
    "job_id": "ea3cfae9-a565-4353-8a5e-16436c164e43",
    "url": "https://api.buildkite.com/v2/organizations/my-great-org/pipelines/my-pipeline/builds/1/jobs/ea3cfae9-a565-4353-8a5e-16436c164e43/artifacts/5c12c7f7-8fb1-419d-b979-48a9e45c7bd7",
    "download_url": "https://api.buildkite.com/v2/organizations/my-great-org/pipelines/my-pipeline/builds/1/jobs/ea3cfae9-a565-4353-8a5e-16436c164e43/artifacts/5c12c7f7-8fb1-419d-b979-48a9e45c7bd7/download",
    "state": "new",
    "path": "tmp/screenshots/155b0d82-4d8e-4b07-9fea-49b58c1c6f1b.png",
    "dirname": "tmp/screenshots",
    "filename": "155b0d82-4d8e-4b07-9fea-49b58c1c6f1b.png",
    "mime_type": "image/png",
    "file_size": 1521347,
    "glob_path": "tmp/screenshots/*",
    "original_path": "/var/lib/buildkite-agent/builds/bk-agent-2/my-great-org/my-pipeline/dist/app.tar.gz",
    "sha1sum": "7a788f56fa49ae0ba5ebde780efe4d6a89b5db47"
  }
]
```

Required scope: `read_artifacts`

Success response: `200 OK`

## List artifacts for a job

Returns a [paginated list](<%= paginated_resource_docs_url %>) of a job’s artifacts.

```bash
curl "https://api.buildkite.com/v2/organizations/{org.slug}/pipelines/{pipeline.slug}/builds/{build.number}/jobs/{job.id}/artifacts"
```

```json
[
  {
    "id": "76365070-34d5-4104-8b91-952780f8029f",
    "job_id": "aae578fe-994c-44e6-84da-4102616928ba",
    "url": "https://api.buildkite.com/v2/organizations/my-great-org/pipelines/my-pipeline/builds/1/jobs/aae578fe-994c-44e6-84da-4102616928ba/artifacts/76365070-34d5-4104-8b91-952780f8029f",
    "download_url": "https://api.buildkite.com/v2/organizations/my-great-org/pipelines/my-pipeline/builds/1/jobs/aae578fe-994c-44e6-84da-4102616928ba/artifacts/76365070-34d5-4104-8b91-952780f8029f/download",
    "state": "finished",
    "path": "dist/app.tar.gz",
    "dirname": "dist",
    "filename": "app.tar.gz",
    "mime_type": "application/x-gzip",
    "file_size": 529371,
    "glob_path": "dist/*",
    "original_path": "/var/lib/buildkite-agent/builds/bk-agent-1/my-great-org/my-pipeline/dist/app.tar.gz",
    "sha1sum": "884c4ad3f2545c85c69d0d0ef50c5d4f5266f0b7"
  }
]
```

Required scope: `read_artifacts`

Success response: `200 OK`

## Get an artifact

```bash
curl "https://api.buildkite.com/v2/organizations/{org.slug}/pipelines/{pipeline.slug}/builds/{build.number}/artifacts/{id}"
```

```json
{
  "id": "76365070-34d5-4104-8b91-952780f8029f",
  "job_id": "aae578fe-994c-44e6-84da-4102616928ba",
  "url": "https://api.buildkite.com/v2/organizations/my-great-org/pipelines/my-pipeline/builds/1/jobs/aae578fe-994c-44e6-84da-4102616928ba/artifacts/76365070-34d5-4104-8b91-952780f8029f",
  "download_url": "https://api.buildkite.com/v2/organizations/my-great-org/pipelines/my-pipeline/builds/1/jobs/aae578fe-994c-44e6-84da-4102616928ba/artifacts/76365070-34d5-4104-8b91-952780f8029f/download",
  "state": "finished",
  "path": "dist/app.tar.gz",
  "dirname": "dist",
  "filename": "app.tar.gz",
  "mime_type": "application/x-gzip",
  "file_size": 529371,
  "glob_path": "dist/*",
  "original_path": "/var/lib/buildkite-agent/builds/bk-agent-1/my-great-org/my-pipeline/dist/app.tar.gz",
  "sha1sum": "884c4ad3f2545c85c69d0d0ef50c5d4f5266f0b7"
}
```

Required scope: `read_artifacts`

Success response: `200 OK`

## Download an artifact

Returns a 302 response to a URL for downloading an artifact. The URL will be returned in the response body and the `Location` HTTP header.

You should assume the URL returned will only be valid for 60 seconds, unless you've used your own S3 bucket where the URL will be the standard public S3 URL to the artifact object.

```bash
curl "https://api.buildkite.com/v2/organizations/{org.slug}/pipelines/{pipeline.slug}/builds/{build.number}/artifacts/{id}/download"
```

```json
{
  "url": "https://buildkiteartifacts.com/artifacts/2196c80a1ff393a88482aebe929f9648/dist/app.tar.gz?AWSAccessKeyId=AKIAIPPJ2IPWN5U3O1OA&Expires=1288526454&Signature=5i4%2B99rUwhpP2SbNsJKhT/nSzsQ"
}
```

Required scope: `read_artifacts`

Success response: `302 Found`