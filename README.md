# AWS Container Lambda

This project demonstrates how to deploy a Lambda function using a container image on AWS.

## Prerequisites

- AWS Account
- AWS CLI configured
- Docker installed
- Go installed

## Setup

1. Clone the repository:
    ```sh
    git clone https://github.com/yourusername/aws_container_lambda.git
    cd aws_container_lambda
    ```

2. Build the Docker image:
    ```sh
    docker build --platform linux/amd64 --provenance=false -t lambda:latest .
    ```

3. Test the Docker image locally:
    ```sh
    docker run -d -p 9000:8080 --entrypoint /usr/local/bin/aws-lambda-rie lambda:latest ./main
    ```

4. Create an ECR repository and push the image:
    ```sh
    aws ecr create-repository --repository-name aws_container_lambda
    $(aws ecr get-login --no-include-email)
    docker tag aws_container_lambda:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/aws_container_lambda:latest
    docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/aws_container_lambda:latest
    ```

5. Deploy the Lambda function:
    ```sh
    aws lambda create-function --function-name aws_container_lambda \
        --package-type Image \
        --code ImageUri=<aws_account_id>.dkr.ecr.<region>.amazonaws.com/aws_container_lambda:latest \
        --role <execution_role_arn>
    ```

## Usage

Invoke the Lambda function:
```sh
aws lambda invoke --function-name aws_container_lambda response.json
```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.

## Author

Subir Adhikari
