# amazon-sagemaker-custom-container

본 레포는 AWS SageMaker에서 custom model을 docker container로 빌드하고 이것을 ECR에 푸쉬하여 사용하는 것을 다룬다.

https://github.com/aws-samples/amazon-sagemaker-custom-container 해당 링크의 원 글에서 코드를 적절하게 수정하여 재배포하였다.

1. 노트북 인스턴스 생성
2. git clone
3. 스크립트 실행
4. 모델 생성 & 배포

## 노트북 인스턴스 생성
노트북 인스턴스를 생성할 때, 볼륨 크기를 50GB로 변경해서 생성하자.

InService상태가 되면 jupyterLab을 실행하고, terminal을 켜도록 하자. 
커서가 깜빡인다면 여기까지 바르게 잘 수행한 것이다. 

## git clone
아래 명령어를 차례로 입력하여 해당 레포를 clone 하도록 한다.

~~~
# 클론
git clone https://www.github.com/silverstar0727/amazon-sagemaker-custom-container
# 클론 한 디렉토리로 위치를 변경하자.
cd amazon-sagemaker-custom-container
~~~

## 스크립트 실행
아래의 명령어를 통해 스크립트를 실행하자. 이때 뒤에 오는 'test' 명령어는 ecr에 생성되는 이미지 이름으로 본인에 맞게 변경이 가능하다.
~~~
# 스크립트를 실행이 가능하도록 설정
chmod +x build-and-push.sh
# 스크립트 실행
./build-and-push.sh test
~~~

## 모델 생성 & 배포
jupyter lab에서 새로운 노트북을 하나 만들고 아래의 명령어를 통해 모델을 생성하고 배포하자

~~~
import sagemaker

estimator = sagemaker.estimator.Estimator(image_uri = <계정아이디>.dkr.ecr.<리전이름>.amazonaws.com/test, role = sagemaker.get_execution_role(), instance_count = 1, instance_type = 'ml.m5.xlarge')

estimator.fit()

estimator.deploy(1, 'ml.m5.xlarge')
~~~
