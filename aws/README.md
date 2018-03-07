Run a demo case on Amazon AWS. This example creates a single server with required following utilities included,

* Spark
* Maven
* Ant
* Gcc compiler
* Git

Here is the procedure to build the demo server and run sample case.

1. Login Amazon AWS account
2. Create keypair, if no keypair exists in your account. Save private key to your local computer.
3. Open `CloudFormation` web console.
4. Create new stack.
5. Choose template. Specify an Amazon S3 template URL. https://s3.amazonaws.com/umccs-public/random.forest.src.demo.json
6. Click `Next`. Finish stack configuration.
    * Specify stack name. No space is allowed in stack name
    * Instance type. `t2.micro` is a tiny default. Change to more powerful instance type when necessary.
    * Select keypair keyname
    * Root disk space. There is a mininum 5G.
    * Select default VPC and Subnet.
7. Click `Next`. Accept default settings. Click `Next` again.
8. Click `Create`.
9. Wait until stack status becomes `CREATE_COMPLETE`. While you are waiting, you can open `EC2` console to check server building progress.
10. Click `Outputs` to see the final virtual public name. For example, `
ec2-54-87-53-224.compute-1.amazonaws.com`
11. Bring up a terminal. Run `ssh` to login.
    ```
    $ ssh -i private_key_path ec2-user@ec2-54-87-53-224.compute-1.amazonaws.com
    
    The authenticity of host 'ec2-54-87-53-224.compute-1.amazonaws.com (54.87.53.224)' can't be established.
    ECDSA key fingerprint is SHA256:QVBqw3KCcIBMqI5YNv1UD+bXgh2vHfwNv+gQh9FBGAo.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added 'ec2-54-87-53-224.compute-1.amazonaws.com,54.87.53.224' (ECDSA) to the list of known hosts.
    [ec2-user@ip-172-31-7-12 ~]$ 
    ```
12. Check out source code and compile.
    ```
    $ sample_case
    ```
    Source code will be clone from `github` to a local folder `randomForestSRC` and `ant build-spark` is run automatically.
13. Run sample task.
    ```
    $ cd randomForestSRC/target/spark/target/
    $ ./hello.sh
    ```
14. Clean up demo environment. On `CloudFormation` console, select the stack name you just created, from `Actions`, select `Delete`.
