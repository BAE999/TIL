# CLI
Amazon Web Services Command Line Interface

AWS 리소스(S3, EC2, IAM 등)를 터미널이나 명령 프롬프트로 제어할 수 있는 도구

콘솔을 클릭하지 않고도 명령어만으로 S3 버킷 생성, 파일 업로드, EC2 실행 같은 작업가능

## S3 CLI 주요 명령어
버킷 목록 확인
- aws s3 ls

버킷 안의 파일 목록
- aws s3 ls s3://버킷이름/
    ```
    aws s3 ls s3://test-123-s3
    ```

파일 업로드
- aws s3 cp 로컬경로 s3://버킷이름/경로
    ```
    aws s3 cp test.log s3://test-123-s3
    ```

파일 다운로드
- aws s3 cp s3://버킷이름/경로 로컬경로
    ```
    aws s3 cp s3://test-123-s3/test.txt .
    ```

디렉토리(폴더) 업로드
- aws s3 cp ./로컬경로 s3://버킷이름/경로 --recursive
    ```
    aws s3 cp ./up_s3 s3://test-123-s3/up_s3 --recursive
    ```

디렉토리(폴더) 다운로드
- aws s3 cp s3://버킷이름/경로 ./로컬경로 --recursive
    ```
    aws s3 cp s3://test-123-s3/ . --recursive
    ```