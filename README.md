Bosh release for CloudFoundry NG Admin UI
=========================================

One of the fastest ways to get [admin-ui](https://github.com/cloudfoundry-incubator/admin-ui) running on any infrastructure is too deploy this bosh release.

Usage
-----

To use this BOSH release, first upload it to your bosh:

```
bosh upload release https://admin-ui-boshrelease.s3.amazonaws.com/boshrelease-admin-ui-6.tgz
```

To deploy it you will need the source repository that contains templates:

```
git clone https://github.com/cloudfoundry-community/admin-ui-boshrelease.git
cd admin-ui-boshrelease
git checkout v6
```

### bosh-lite/warden deployments

Make sure you have [deployed Cloud Foundry](https://github.com/cloudfoundry/bosh-lite#deploy-cloud-foundry) first. With Cloud Foundry deployed to your bosh-lite run:

```
./make_manifest warden
bosh deploy
bosh run errand register_admin_ui
```

Now you can browse to [http://admin.10.244.0.34.xip.io](http://admin.10.244.0.34.xip.io) and login with your cloud foundry admin user.

### Errands
When deployed you can register the admin-ui with the uaa by running:
```
bosh run errand register_admin_ui
```

To deregister the admin-ui you can run:
```
bosh run errand deregister_admin_ui
```

Create new final release
------------------------

To create a new final release you need to get read/write API credentials to the [@cloudfoundry-community](https://github.com/cloudfoundry-community) s3 account.

Please email [Dr Nic Williams](mailto:&#x64;&#x72;&#x6E;&#x69;&#x63;&#x77;&#x69;&#x6C;&#x6C;&#x69;&#x61;&#x6D;&#x73;&#x40;&#x67;&#x6D;&#x61;&#x69;&#x6C;&#x2E;&#x63;&#x6F;&#x6D;) and he will create unique API credentials for you.

Create a `config/private.yml` file with the following contents:

```yaml
---
blobstore:
  s3:
    access_key_id:     ACCESS
    secret_access_key: PRIVATE
```

You can now create final releases for everyone to enjoy!

```
bosh create release
# test this dev release
git commit -m "updated admin-ui"
bosh create release --final
git commit -m "creating vXYZ release"
git tag vXYZ
git push origin master --tags
```
