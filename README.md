# MicroService

This is the main README for the MicroService project. Part 1 creates a micro service infrastructure to showcase the 
native logging ability of AWS. WARNING: This is not how you build Microservices. I particularly choose/made the 
example this way to illustrate my point. The major problem being that we created tight coupling between our services.
Stay tuned for Part 2 which fixes these issues and will focus more on the code & CDK used.
                               
The micro services are defined in these repos:
- https://github.com/rehanvdm/MicroServicePerson
- https://github.com/rehanvdm/MicroServiceCommon
- https://github.com/rehanvdm/MicroServiceClient


## White board diagram
![Alt text](images/MicroService-Overview.png?raw=true "Part 1 - System overview")


Each service has an OpenAPI definition:
- https://github.com/rehanvdm/MicroServicePerson/blob/master/part1/src/lambda/api/api-definition.yaml
- https://github.com/rehanvdm/MicroServiceCommon/blob/master/part1/src/lambda/api/api-definition.yaml
- https://github.com/rehanvdm/MicroServiceClient/blob/master/part1/src/lambda/api/api-definition.yaml


The focus path that we focus on: 
![Alt text](images/MicroService-FocusPath.png?raw=true "Part 1 - Focus path for presentation & blog")


Part 2 will focus on reducing the coupling between service and will produce an architecture similar to below (coming soon).
![Alt text](images/MicroService-Part2.png?raw=true "Part 2 - Coming soon!")
