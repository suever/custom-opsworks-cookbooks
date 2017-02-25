See http://docs.aws.amazon.com/opsworks/latest/userguide/customizing.html

Changes in this branch:

Far-future expiration headers for rails assets.

_NOTE: Since our particular setup involves a load balancer forwarding decrypted requests to port 80 on OpsWorks instances, we don't care about the NGINX SSL settings. If we **did** care about them, we'd probably also need to disable SSLv3 due to the POODLE vulnerability._

### Helpful info

To run a deploy on an OpsWorks instance via the command-line: `$: sudo opsworks-agent-cli run_command deploy`

To test OpsWorks hooks under the app's deploy/ dir, update this custom cookbook to override the [default `deploy` recipe](https://github.com/aws/opsworks-cookbooks/blob/release-chef-11.10/deploy/definitions/opsworks_deploy.rb) to NOT pull from github so that you can edit the hook files directly in the current/ dir without them being overwritten. _TODO: write better instructions for this._
