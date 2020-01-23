## Test your CloudFormation templates

It is commonly known, we do not test Infrastructure as a Code.
How many times you run your template, revert, change, run, revert, change....

In DevOps world we have the tools to do proper CI/CD not only for applications, but for IaC too!

### Repository content

This repo contains one very simple CloudFormation template. It creates VPC with two subnets, and S3 bucket.

This template is executed through CI/CD tool, Travis CI (file `.travis.yml`).

Travis is executing three tests. 

* lint template
* validate template
* execute test builds 

### Linter

Linter is using my container for linting CloudFormation files. It checks the structure of the template.

### Validation

By running `aws cloudformation template-validate` it checks the syntax of the template. It is a container as well.

### TaskCat

TaskCat is a great tool for testing CF templates. Shortly speaking, TaskCat is building the template in real account, and create the 
real infrastructure.  
In my example, TaskCat is building infrastructure in three separate regions, to check (and prove) if template is buildable.

TaskCat can also lint template.

### Template preparation

Important information. to properly test CloudFormation template using TaskCat, the template must be properly prepared. 
You need to focus on proper parametrization to be able to execute tests effectively.

### TaskCat config

Configuration is stored in `.taskcat.yml` file.  `.taskcat_ovverides.yml` contains parameters to ovveride, when template is executed. 
By this we can avoid the situation, when your resources cannot be build, because of names clash (or whatever).

### Output

TaskCat writes summary in `taskcat_outputs` directory, in case of this repo, this directory is ignored it `.gitignore`.
