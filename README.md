# BOSH release for tensorflow

This BOSH release and deployment manifest deploy a cluster of tensorflow.

## Install

```
export BOSH_ENVIRONMENT=<bosh-alias>
export BOSH_DEPLOYMENT=tensorflow
bosh deploy manifests/tensorflow.yml --vars-store tmp/creds.yml
```

If your BOSH has Credhub, then you can omit `--vars-store` flag. It is used to generate any passwords/credentials/certificates required by `manifests/tensorflow.yml`.

## Usage

SSH into the running instance:

```
bosh ssh tensorflow/0
```

Then activate the virtualenv containing tensorflow:

```
cd /var/vcap/packages/tensorflow
. venv/bin/activate
python
```

To verify, run a hello world in the python repl:

```
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```

## Development

As a developer of this release, create new releases, upload and deploy them:

```
bosh create-release --force && \
  bosh -n upload-release && \
  bosh deploy manifests/tensorflow.yml --vars-store tmp/creds.yml
```

If your BOSH has Credhub, then you can omit `--vars-store` flag. It is used to generate any passwords/credentials/certificates required by `manifests/tensorflow.yml`.
