## Installation and Set-Up
### AWS with Python and `boto3`
to use the Python boto3 library and connect it to an AWS account we need to:
- set up an aws account
- install boto3
```bash
pip install boto3
uv add boto3 # if you're using uv
```
- configure access keys for boto3
	- when configuring access keys for boto3, if you only have one access key profile (usually `[default]` in the `~/.aws/credentials` file, then go ahead and run:
	  ```python
		import boto3
		s3 = boto3.resource('s3') # access aws client using [default] profile
		```
	- OR, if you have more than one access key profile (ex: `[boto3]`) in the `~/.aws/credentials` file, run:
	  ```python
		import boto3
		session = boto3.Session(profile_name='boto3') # create a session that contains your aws config
		s3 = session.resource('s3') # use `session` to create an `s3` resource
		```
### Different Types of `boto3` Calls
- client
- resources
- paginators
- waiters
#### Client
- a client in `boto3` represents the connection to a specific AWS service
- provides low-level access to the API operations offered by that service
- creating a `boto3` client for a specific AWS service allows you to make direct API requests and manage resources associated with that service
#### Paginators
- AWS service APIs often return large amounts of that can be paginated for easier handling
- paginators in `boto3` are helpers that allow you to iterate over paginated API responses seamlessly
#### Waiters
- waiters in `boto3` are used to wait for a certain condition or state to be met in an AWS resource before proceeding with further actions
- they help you poll an AWS service API until a specific state is reached

> For example: you might use a waiter to wait for a specific AWS EC2 instance to reach the "running" state after launching it.
> 
> Waiters are especially helpful in cases where you need to ensure that an operation has completed before moving on.

#### Resources
- resources in `boto3` provide a higher-level, more pythonic interface to interact with AWS services
- they abstract away the details of the API calls and provide a more object-oriented way of working with AWS resources
- resources represent AWS entities like instances, s3 buckets, DynamoDB tables, etc.
- resources make it easier to use methods and attributes to manipulate services without directly dealing with the underlying API calls
#### Which to Choose?
- client
	- low-level API interaction with an AWS service
- paginators
	- handle paginated results from AWS service APIs
- waiters
	- wait for specific conditions to be met in AWS resources
- resources
	- high-level, object-oriented interface to interact with AWS resources

