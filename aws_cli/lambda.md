# aws s3 [subcommand]

This is the aws subcommand used to handle s3 buckets it will receive a second subcommand indicating the action to be done over the referenced bucket.

The following subcommands could be useful to deal with bucket tasks:

## sync

Will let you sync/copy the elements inside a buckets in this ways:

`<LocalPath> <S3Uri> or <S3Uri> <LocalPath> or <S3Uri> <S3Uri>`

```bash
aws s3 sync s3://path.to.bucket s3://path.to.backup.bucket/document --storage-class=STANDARD_IA --only-show-errors
# will move the content of path.to.bucket to a folder inside path.to.other.bucket
```

You can specify adicional arguments and flags to the command as you can see:

* `--quiet` (boolean) Does not display the operations performed from the specified command.
* `--storage-class` (string) The type of the storage to use for the object. Defaults STANDARD
* `--only-show-errors` (boolean) Only errors and warnings are displayed. All other output is suppressed
* `--exact-timestamps` (boolean) When syncing from S3 to local, same-sized items will be ignored only when the timestamps match exactly. The default behavior is to ignore same-sized items unless the local version is newer than the S3 version.
* `--delete` (boolean) Files that exist in the destination but not in the source are deleted during sync.

[More information :link:](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/s3/sync.html) about **sync**

