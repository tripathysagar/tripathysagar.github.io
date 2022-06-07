
# Deploying ONNX model using flask in Heroku
Assumptions: You have already trained the model on your specific image classification problem.

This blog post is the result of reading chapter 2 of fastbook. The assignment for this chapter is to deploy the notebook as a real app. So I tried to deploy to Heroku by converting the notebook to a web server via voila. But unfortunately, I was not able to optimize the app size 500Mb. 

So I tried to build the app using the lightweight flask framework. To minimize the app runtime dependency on PyTorch by using ONNX. 

  1.    Train model foryour perticular problem
  2.    Export the model to ONNX format:
         ```
         dummy_input = torch.randn(1, 3, 224, 224,requires_grad=True)
         onnx_path =  "./model_2_resnet34.onnx"
         torch.onnx.export(learn.model.cpu(), dummy_input, onnx_path, input_names=['input'], output_names=['output'])
         ```
   
   3.    - Clone the [flask app](https://github.com/tripathysagar/imageUploader.git). The app.py is the flask app file where we get the input image from the user. Then we resize it to our desired shape and save the file. And we run the inference on the uploaded image using the pred.py file. Upon running the model, we return the image and the classification.

         - Please update the label.json folder with the class name of your project's category.
         - Substitute our model(onnx file) with model_2_resnet34.onnx. Please note that the name of the file should be updated in "pred.py" with your file name.
         - Update the heading in "templates/layout.html" which suits your app.
   
   4.    Run the flask app. Go to the cloned project directory.And run :
          ```
          export FLASK_APP=app
          flask run
          ```
          Please goto "http://127.0.0.1:5000/" and check if your app is running fine wrt your model.
   
   5.    Create a git repo and push all the changes.
   
   6.    Create an account on the Heroku website. And install Heroku CLI in your system. Command for deploying can be found [here](https://devcenter.heroku.com/articles/github-integration)
   
   7.    Goto your webapp to check the working @ https://your-app-name>.herokuapp.com/.

Happy learning.👌
