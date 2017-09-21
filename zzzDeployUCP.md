# Deploy/Update the service

This is a separate section since the process depends a lot on what you are looking for. One option is to have the build process stop at the moment it has successfully built the app and pushed the image to the registry. Another option is for the build process to continue to actually deploy the new image to a runtime environment. 

Both are possible. This is purely a matter of preference. 

## Deploy manually

Go to UCP
Navigate to services
Select your service to update or
Create a new service


## Deploy automated

Add a third step to your build process. Connect to 