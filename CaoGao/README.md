# Usage of the restful API

### Export saved model (Mnist-model):

Download [tenserfow serving source code](https://github.com/tensorflow/serving) and run:

	$  python path/to/tensorflow-serving/example/mnist_saved_model.py [—training_iteration=x] [—model_version=y] export-directory 


This will generate a saved model in the given directory.


### Build image:

`$ CARGO_SERVER=cargo.caicloud.io IMAGE_TAG=v1.0.2 make image`


### Start the grpc service:

Copy the saved model and paste it in the image under the folder : 

`//caicloud/serving/examples/models/`

Start the grpc service on port 50051: 

	$ docker run -d -p 50051:50051 \
	
	cargo.caicloud.io/caicloud/tensorflow-serving-grpc:v1.0.2 \ 
	
	python       /caicloud/serving/grpc_server.py  \
	
	--model_path=//caicloud/serving/examples/models/savedmodel-folder-name\ 
	
	—max_workers=20 

### Start the restful service:


**Run the image in docker:**

`$ docker run -ti -p 8080:8080 cargo.caicloud.io/caicloud/tensorflow-serving-restful:v1.0.2   bash`

**Start the service:**

`# ./gateway grpc_endpoint=ip_of_your_pc:50051`_ ![](Screen%20Shot%202018-03-20%20at%2011.24.01%20AM.png)

### Build and run the client:

**Write Client code:**

Download [caicloud.tensorflow-2.1.0](https://pypi.python.org/pypi/caicloud.tensorflow) and install it, make sure that the path of the source is correctly added to the client code so that you can make properly import in the client code:

`from caicloud.clever.serving.client import restfulclient
``from caicloud.clever.serving.client import servingerror
`
Set up client:

`client = restful_client.RESTfulClient('localhost:8080')`

Manage inputs:

	inputs = {
	
	 ‘image': tf.contrib.util.make_tensor_proto(input_data, shape=input_data_shape),
	
	}

For Mnist, use its given test inputs( I used 10 images in the input):

![](Screen%20Shot%202018-03-20%20at%2011.32.52%20AM.png)

Then make prediction.


**Use the client code:**
Simply run `$ python restful_client` and the proper output should be:
![](Screen%20Shot%202018-03-20%20at%2011.43.54%20AM.png)








