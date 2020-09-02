# aws configure

This is the **aws** subcommand used to handle credentials. You can add multiple of them and they will be used by another tools having dependency from `aws cli`.

**aws configure** uses two files to keep that information
```bash
~/.aws/credentials
~/.aws/config
```
The first one saves your `access_key` and `secret_key` and the second your default `region` and the `format` to retrieve data. 

If you place more than one set of credentials you must follow the format

```bash
# inside ~/.aws/credentials file
[default]
AWS_ACCESS_KEY_ID=<MYACCESSKEY>
AWS_SECRET_ACCESS_KEY=<MYSECRETACCESSKEY>

[profileA]
AWS_ACCESS_KEY_ID=<MYACCESSKEY>
AWS_SECRET_ACCESS_KEY=<MYSECRETACCESSKEY>
```

To choose one of those credentials set you must set the variable `AWS_PROFILE=profileA`. If you run it without any argument it will refers to `default` profile. 

