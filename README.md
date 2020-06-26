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

## Log Insight Queries used in Demo

#### All Audit Records
```
fields args.created_at, args.type, args.origin, args.origin_path, args.status, args.status_code,
args.run_time, args.status_description, args.meta, args.trace_id,args.environment,
args.app_version, args.client_id, args.user_id, @logStream
| filter level = "audit" 
| sort args.created_at asc
```

#### All Audit Records for a specific TraceID
```
fields args.created_at, args.type, args.origin, args.origin_path, args.status, args.status_code,
args.run_time, args.status_description, args.meta, args.trace_id,args.environment,
args.app_version, @logStream
| filter level = "audit" and traceId = "7279de77-a13f-418e-9042-d29dde7e0c29"
| sort args.created_at asc
```

#### All Audit Records for a specific User
```
fields args.created_at, args.type, args.origin, args.origin_path, args.status, args.status_code,
args.run_time, args.status_description, args.meta, args.trace_id,args.environment,
args.app_version, @logStream
| filter level = "audit" and args.user_id = "rehan"
| sort args.created_at asc
```

#### All Hard errors
```
fields date, replace(@log, '<AWS ACCOUNT NUMBER>:/aws/lambda/', '') as Service, coalesce(errorType, 'Time out') as ErrorType, errorMessage, @logStream
| filter ispresent(errorType) or strcontains(@message, 'Task timed out after')
| sort @timestamp asc
```

#### All Soft errors
```
fields date, replace(@log, '<AWS ACCOUNT NUMBER>:/aws/lambda/', ''), msg, @logStream
| filter level == "error"
| sort @timestamp asc
```

#### All log lines for TraceID
```
fields date, replace(@log, '<AWS ACCOUNT NUMBER>:/aws/lambda/', '') as Service, level, env, args.status, args.status_code,@logStream
| filter traceId = "c8392e95-f19d-469f-99d7-3ead309768a1"
| sort @timestamp asc
```

#### Most expensive API calls
Go to Visualization, choose Bar graph
```
fields args.created_at, args.origin, args.origin_path, args.run_time
| sort args.created_at desc
| filter level="audit" AND args.status_code < 3000
| stats percentile(args.run_time,95) by concat(args.origin, " ", args.origin_path)
```