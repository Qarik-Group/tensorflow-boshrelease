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
for dir in $(ls -d /var/vcap/packages/*/bin); do export PATH=$dir:$PATH; done
for dir in $(ls -d /var/vcap/packages/*/lib); do export LD_LIBRARY_PATH=$dir:${LD_LIBRARY_PATH:-}; done
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

To see a simple TensorFlow cluster in action, execute the following:

```
import tensorflow as tf
hello = tf.constant("Hello, distributed TensorFlow!")
server = tf.train.Server.create_local_server()
sess = tf.Session(server.target)  # Create a session on the server.
sess.run(hello)
```

To explicitly run a server on a specific port without any peers:

```
import tensorflow as tf
hello = tf.constant("Hello, distributed TensorFlow!")
cluster = tf.train.ClusterSpec({"local": ["localhost:2222"]})
server = tf.train.Server(cluster, job_name="local", task_index=0)
sess = tf.Session(server.target)  # Create a session on the server.
sess.run(hello)
```
