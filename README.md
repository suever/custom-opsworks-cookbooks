See http://docs.aws.amazon.com/opsworks/latest/userguide/customizing.html

Changes in this branch:

Far-future expiration headers for rails assets.

_NOTE: Since our particular setup involves a load balancer forwarding decrypted requests to port 80 on OpsWorks instances, we don't care about the NGINX SSL settings. If we **did** care about them, we'd probably also need to disable SSLv3 due to the POODLE vulnerability._

## Testing Chef customizations (on a non-prod Stack!!!)

(Do all these things as the root user)

### Deploying via CLI
`$: opsworks-agent-cli run_command deploy`

### Editing recipes directly

See the "To edit a recipe on an instance" section here: http://docs.aws.amazon.com/opsworks/latest/userguide/cookbooks-101-opsworks-opsworks-instance.html#cookbooks-101-opsworks-opsworks-instance-bugs

### Testing before_migrate hook

To test the before_migrate.rb hook under the app's deploy/ dir, it's easiest to update the custom cookbooks on the instance to override the [default `deploy` recipe](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/deploy/definitions/opsworks_deploy.rb) to point to a different location for the hook so that you can edit it directly without worrying about it being overwritten by the deploy.

To update the local cookbook recipe, first navigate to `/opt/aws/opsworks/current/site-cookbooks`.

Next, add `deploy/definitions/opsworks_deploy.rb` to this directory (copy and paste the content from AWS's GitHub repo).

Next, edit the opsworks_deploy file and replace:

`run_callback_from_file("#{release_path}/deploy/before_migrate.rb")`

with:

`run_callback_from_file("/srv/www/[app_name]/before_migrate.rb")`

so that you can continually edit this file and run the CLI deploy command -- no need for several git commits and UI-triggered deploys.

**IMPORTANT:** Obviously, don't forget to revert all of these changes on the instance when done.

### Testing other deploy hooks

The git clone and replacing of current code by the deploy process is the issue here. TODO: replace with proper instructions if this is ever really needed / figured out.

## Testing nginx settings (on a non-prod Stack!!!)

`$ sudo vi /etc/nginx/sites-enabled/[environment]`
`$ sudo service nginx restart`
