## POC to setup VPC & launch EC2 using a simple CFT ##

#### Steps ####
Markup: * Setup VPC
        * Launch EC2
        * Install Airflow in EC2
        * Place data in S3
        * Create Crawler and read schema to Data Catalog (Athena)
        * Launch RDS MySQL instance
        * Launch EMR
        * Write sprak to read data from S3 and write to RDS
        * Trigger Spark job from Airflow
            * Launch EMR
            * Run PySpark
            * Decommission EMR
        * Clean up
        * Write CFT for the POC -> whole or VPC/EC2/EMR


1. Copy CFT to S3
2. Create CFT Stack in CF using CFT template from S3
    select proper parameters for instance type, keypair & local ip for ssh
3. ssh into instance using .pem from a secure place and check httpd status
    ssh -i ~/.ssh/aws-keypair/airflow-poc-keypair.pem ec2-user@18.189.119.235
    sudo /etc/init.d/httpd status
    cat /etc/os-release
4. open public ip now you should be able to see the httpd running
    yum update -y
    yum install gcc python3 python-setuptool
    yum insyall pip -y
    vi ~/.bashrc
    alias python='/usr/local/bin/python2.7'
    alias pip='/usr/local/bin/pip'
    source ~/.bashrc
    pip install apache-airflow
    pip install --upgrade six
    mkdir ~/airflow
    cd ~/airflow
    mkdir dags
    cd ..
    airflow initdb
    airflow scheduler -D
    airflow webserver -D
    

  $> aws datapipeline create-pipeline --name hello_world_pipeline --unique-id hello_world_pipeline
 
  $> aws datapipeline put-pipeline-definition --pipeline-id df-0554887H4KXKTY59MRJ \
  --pipeline-definition file://samples/helloworld/helloworld.json \
  --parameter-values myS3LogsPath="s3://<your s3 logging path>"
  
  $> aws datapipeline activate-pipeline --pipeline-id df-0554887H4KXKTY59MRJ
  
  $> aws datapipeline list-runs --pipeline-id df-0554887H4KXKTY59MRJ