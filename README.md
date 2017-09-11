# Run Tensorflow on any infrastructure using BOSH

Deploy this BOSH release to bring up Tensorflow on any supported infrastructure (vSphere, Google Compute, Amazon EC2, Microsoft Azure, OpenStack, and more).

**STATUS:** This BOSH release packages tensorflow and its python dependencies and makes python/tensorflow available. See [Usage](#usage) section. I'm still investigating useful ways to use a cluster of Tensorflow servers.

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
