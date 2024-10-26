pipeline{
  agent any
  stages{
     stage('Create VPC') {
       steps{
          sh 'gcloud compute networks create vpc-auto --project=hari-cloud-first-project  --subnet-mode=auto'
          sh 'gcloud compute networks create vpc-custom --project=hari-cloud-first-project  --subnet-mode=custom'
          sh 'gcloud compute networks create vpc1-custom --project=hari-cloud-first-project  --subnet-mode=custom'
    }
  }
  stage('Create subnets') {
       steps{
           sh 'sleep 40'
           sh 'gcloud compute networks subnets create vpc-custom-subnets --project=hari-cloud-first-project --range=10.0.0.0/24 --network=vpc-custom  --region=us-central1'
           sh 'gcloud compute networks subnets create vpc1-custom-subnets --project=hari-cloud-first-project --range=10.0.0.0/24 --network=vpc1-custom  --region=us-central1'
    }
  }
  stage('Create Firewall rules') {
       steps{
           sh 'sleep 40'
           sh 'gcloud compute --project=hari-cloud-first-project firewall-rules create enable-vpc-auto-fw --description=enable-vpc-auto-fw --direction=INGRESS --priority=1000 --network=vpc-auto --action=ALLOW --rules=tcp:22,icmp --source-ranges=0.0.0.0/0'
           sh 'gcloud compute --project=hari-cloud-first-project firewall-rules create enable-vpc-custom-fw --description=enable-vpc-custom-fw --direction=INGRESS --priority=1000 --network=vpc-custom --action=ALLOW --rules=tcp:22,icmp --source-ranges=0.0.0.0/0'
           sh 'gcloud compute --project=hari-cloud-first-project firewall-rules create enable-vpc1-custom-fw --description=enable-vpc1-custom-fw --direction=INGRESS --priority=1000 --network=vpc1-custom --action=ALLOW --rules=tcp:22,icmp --source-ranges=0.0.0.0/0'
    }
  }
  stage('Create instance with VPC') {
       steps{
           sh 'sleep 40'
           sh 'gcloud compute instances create instance-vpc-auto --project=hari-cloud-first-project --zone=us-central1-f --machine-type=e2-medium --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=vpc-auto --service-account=jenkis-github@hari-cloud-first-project.iam.gserviceaccount.com'
           sh 'gcloud compute instances create instance-vpc-custom --project=hari-cloud-first-project --zone=us-central1-f --machine-type=e2-medium --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=vpc-custom-subnets --service-account=jenkis-github@hari-cloud-first-project.iam.gserviceaccount.com'
           sh 'gcloud compute instances create instance-vpc1-custom --project=hari-cloud-first-project --zone=us-central1-f --machine-type=e2-medium --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=vpc1-custom-subnets --service-account=jenkis-github@hari-cloud-first-project.iam.gserviceaccount.com'
         
    }
  }
  stage('Create VPC Peering auto to custom and custom to auto') {
       steps{
           sh 'sleep 40'
           sh 'gcloud compute networks peerings create vpc-auto-to-custom --network=vpc-auto --peer-network=vpc-custom --export-custom-routes --import-custom-routes --export-subnet-routes-with-public-ip --import-subnet-routes-with-public-ip'
           sh 'gcloud compute networks peerings create vpc-custom-to-auto --network=vpc-custom --peer-network=vpc-auto --export-custom-routes --import-custom-routes --export-subnet-routes-with-public-ip --import-subnet-routes-with-public-ip'
    }
  }
  stage('Create VPC Peering Custom to custom and custom to custom') {
       steps{
           sh 'sleep 40'
           sh 'gcloud compute networks peerings create vpc-custom-to-custom --network=vpc-custom --peer-network=vpc1-custom --export-custom-routes --import-custom-routes --export-subnet-routes-with-public-ip --import-subnet-routes-with-public-ip'
           sh 'gcloud compute networks peerings create vpc1-custom-to-custom --network=vpc1-custom --peer-network=vpc-custom --export-custom-routes --import-custom-routes --export-subnet-routes-with-public-ip --import-subnet-routes-with-public-ip'
    }
  }
 }
}
