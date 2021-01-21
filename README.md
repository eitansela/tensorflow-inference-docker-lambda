# Pay as you go TensorFlow inference with AWS Lambda (Docker image)

This repo contains resources to help you deploy a Lambda function based on Python Docker Image. 
The application illustrates how to perform inference with breast cancer XGBoost ML model.

## Building the Lambda Function Docker image, testing locally, and pushing to Amazon ECR registry
In the next steps you'll build the Docker image, and optionally test it on your machine. 
Next, you'll tag the Docker image and push it to [Amazon ECR Repository](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Repositories.html).

### Create an image from an AWS base image for Lambda
Build your Docker image with the `docker build` command. Enter a name for the image. The following example names the image tensorflow-inference-docker-lambda.

`docker build -t tensorflow-inference-docker-lambda .`  

### (Optional) Test your application locally using the [runtime interface emulator](https://docs.aws.amazon.com/lambda/latest/dg/images-test.html)

Run your container image locally using the docker run command

`docker run -p 9000:8080 tensorflow-inference-docker-lambda:latest`

From a new terminal window, post an event to the following endpoint using a curl command:

```bash
curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{"url":"https://images.pexels.com/photos/310983/pexels-photo-310983.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940"}' .
```

This command invokes the function running in the container image and returns a response.

You should get a response as follows: 

`{
  "statusCode": 200,
  "body": "{\"detection_boxes\": [[0.4908023476600647, 0.29575255513191223, 0.9392691254615784, 0.7548272609710693], ... ,  [0.777909517288208, 0.17248520255088806, 0.8303424119949341, 0.2794511020183563], [0.7956447005271912, 0.7707855701446533, 0.8414708971977234, 0.8657528162002563], [0.4194037616252899, 0.38772422075271606, 0.5819851160049438, 0.6199124455451965], [0.4991573393344879, 0.660798966884613, 0.5842983722686768, 0.7324734330177307], [0.6248317956924438, 0.4229450225830078, 0.714311957359314, 0.4761013984680176], [0.5048995614051819, 0.3360440731048584, 0.9578962922096252, 0.747931957244873], [0.25196680426597595, 0.5040083527565002, 0.3461447060108185, 0.5859017968177795], [0.3686038851737976, 0.5651803016662598, 0.5246869325637817, 0.6727312803268433]], \"detection_scores\": [0.916389524936676, 0.5633864402770996, 0.5071750283241272, 0.48786163330078125, 0.4274715185165405, 0.34820130467414856, 0.34308966994285583, 0.2551177144050598, 0.22764959931373596, 0.19553837180137634, 0.18307867646217346, 0.18057134747505188, 0.17650437355041504, 0.17296314239501953, 0.16335374116897583, 0.1418268382549286, 0.14004772901535034, 0.1332404911518097, 0.13123473525047302, 0.13102367520332336, 0.1298324167728424, 0.12939614057540894, 0.12896358966827393, 0.12534162402153015, 0.12301257252693176, 0.1213630735874176, 0.12025535106658936, 0.12021270394325256, 0.11892074346542358, 0.11779901385307312, 0.1163802444934845, 0.1140052080154419, 0.110423743724823, 0.10846152901649475, 0.10566648840904236, 0.1050737202167511, 0.10462230443954468, 0.10421887040138245, 0.1037231981754303, 0.10283547639846802, 0.10196498036384583, 0.09892857074737549, 0.0961516797542572, 0.09614330530166626, 0.09533616900444031, 0.09457707405090332, 0.09454470872879028, 0.0943322479724884, 0.09380272030830383, 0.0934898853302002, 0.09256741404533386, 0.091624915599823, 0.0910930335521698, 0.09107786417007446, 0.09001454710960388, 0.0892321765422821, 0.08818307518959045, 0.08758324384689331, 0.08625560998916626, 0.0861017107963562, 0.08543151617050171, 0.08521309494972229, 0.08314070105552673, 0.08303701877593994, 0.08074915409088135, 0.08056560158729553, 0.08041906356811523, 0.08026280999183655, 0.08019626140594482, 0.07966309785842896, 0.07950219511985779, 0.07922336459159851, 0.07921189069747925, 0.07914260029792786, 0.07889220118522644, 0.07847416400909424, 0.07667499780654907, 0.07641705870628357, 0.07548648118972778, 0.07533806562423706, 0.07530558109283447, 0.07393765449523926, 0.07384201884269714, 0.07302078604698181, 0.072294682264328, 0.07189682126045227, 0.071207195520401, 0.07094147801399231, 0.07071718573570251, 0.07029151916503906, 0.06974032521247864, 0.06958404183387756, 0.06949862837791443, 0.06899949908256531, 0.06866246461868286, 0.06792470812797546, 0.06728839874267578, 0.06699958443641663, 0.06661209464073181, 0.0656241774559021], \"detection_class_entities\": [\"Bicycle\", \"Person\", \"Wheel\", \"Wheel\", \"Bicycle wheel\", \"Bicycle wheel\", \"Man\", \"Footwear\", \"Footwear\", \"Clothing\", \"Footwear\", \"Footwear\", \"Footwear\", \"Person\", \"Footwear\", \"Footwear\", \"Clothing\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Tire\", \"Footwear\", \"Footwear\", \"Footwear\", \"Man\", \"Footwear\", \"Footwear\", \"Man\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Person\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Human arm\", \"Human arm\", \"Footwear\", \"Jeans\", \"Tire\", \"Footwear\", \"Footwear\", \"Footwear\", \"Clothing\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Clothing\", \"Footwear\", \"Footwear\", \"Human arm\", \"Footwear\", \"Footwear\", \"Man\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Bicycle\", \"Footwear\", \"Footwear\", \"Footwear\", \"Footwear\", \"Clothing\", \"Footwear\", \"Clothing\", \"Footwear\", \"Footwear\", \"Wheel\", \"Clothing\", \"Footwear\", \"Person\", \"Person\", \"Clothing\", \"Footwear\", \"Man\", \"Skyscraper\", \"Footwear\", \"Person\", \"Footwear\", \"Footwear\", \"Human arm\", \"Footwear\", \"Footwear\", \"Person\", \"Human hair\", \"Human arm\"]}"
}`

### Create an Amazon ECR Repository

`aws ecr create-repository --repository-name tensorflow-inference-docker-lambda`

### Authenticate the Docker CLI to your Amazon ECR registry

Replace `<YOUR AWS ACCOUNT ID>` with your AWS account id

`aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <YOUR AWS ACCOUNT ID>.dkr.ecr.us-east-1.amazonaws.com`

### Tag your image to match your repository name, and deploy the image to Amazon ECR using the docker push command

Replace `<YOUR AWS ACCOUNT ID>` with your AWS account id

`docker tag  tensorflow-inference-docker-lambda:latest <YOUR AWS ACCOUNT ID>.dkr.ecr.us-east-1.amazonaws.com/tensorflow-inference-docker-lambda:latest`

`docker push <YOUR AWS ACCOUNT ID>.dkr.ecr.us-east-1.amazonaws.com/tensorflow-inference-docker-lambda:latest`

## Creating the Lambda Function in AWS Console

In the next steps, you'll create the Lambda function in AWS console, using the container image built previously. 

### Create Function

1. Browse to the [Lambda console](https://console.aws.amazon.com/lambda).
2. Choose **_Create function_**.
3. On the next page, choose **_Container image_**. 
4. For Function name, use _tensorflow-inference-docker-lambda_.
5. For Container image URI, choose the **_Browse images_**, and choose the container image you pushed to ECR previously.
6. Choose **_Create function_**.

![Create Function](./img/create_function.png)

Wait for the function to be created.

### Modify Lambda memory and time out
Since the Lambda will load an XGBoost model and use it for inference you'll need to increase the time out and allocate more memory.

1. On the Basic settings section, choose _**Edit**_.
2. For Memory (MB), choose 4096 MB.
3. For Timeout, choose 2 minutes.
4. Choose _**Save**_.

![Edit basic settings](./img/edit_basic_settings.png)

## Testing your Lambda function

1. In the Lambda console, select Configure test events from the Test events dropdown.
2. For Event Name, enter InferenceTestEvent.
3. Copy the event JSON from [here](./test-event/test-event-1.json) and paste in the dialog box.
4. Choose _**Create**_.

![Configure test event](./img/configure_test_event.png)

After saving, you see InferenceTestEvent in the Test list. Now choose _**Test**_.

You see the Lambda function inference result, log output, and duration:

![Lambda execution result](./img/execution_result.png)
